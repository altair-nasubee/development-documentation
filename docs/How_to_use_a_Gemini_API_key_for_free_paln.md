## Google Workspaceの管理コンソール で Google AI Studio を許可
https://admin.google.com/u/1/ac/home?journey=218&hl=ja&utm_source=google

アプリ > その他のGoogleサービス で、
Google Developers （またはGoogle AI Studio）
を「すべてのユーザー」に対して「オン」にする

※デフォルトで、すでに設定が「オン」だった。


## Google AI Studio にログイン
https://aistudio.google.com/prompts/new_chat

- アカウントの切り替え
画面左下のSettingsの下にアカウント名が表示されているのでGoogle Workspaceのアカウントに切り替える

- セキュリティについて
入力情報をAIに学習させないようにしたり、情報を外部に参照されないようにしたいが、
無料枠の Google AI Studio では無理。
見られて困る情報は入力しないこと。

- APIキーの取得
画面左下のメニューに Get API Key があるので選択
画面左上の [Get API Key] もしくは [APIキーを作成] を押下

名前を設定して、プロジェクトを選択、キーを作成
（これをコピーしておく）

Google AI Studioにおける「プロジェクト」とは、APIキー、利用枠（クォータ）、請求（支払い設定）、そしてアクセス権限を管理するための種別


## Free plan の利用制限
Google AI StudioのAPIは、利用状況に応じて「無料プラン（Free Tier）」と「有料プラン（Paid Tier）」に分かれており、それぞれ使用制限（レートリミット）のルールが異なります。 [1, 2] 
新しく作成した「Default Gemini Project」は初期状態で無料プランが適用されています。
------------------------------
## 1. 無料プラン（Free Tier）の使用制限
クレジットカード登録なしで利用できますが、制限は比較的厳しめ。
主要モデルごとの制限の目安は以下の通り。

| 対象モデル | 1分間の上限 (RPM) | 1日の上限 (RPD) | 1分間の最大トークン数 (TPM) | データの扱い |
|---|---|---|---|---|
| Gemini 2.5 Flash-Lite | 15回 | 1,000回 | 250,000 | Googleの品質向上（学習）に使用される可能性あり |
| Gemini 2.5 Flash | 10回 | 250回 | 250,000 | Googleの品質向上（学習）に使用される可能性あり |
| Gemini 2.5 Pro | 5回 | 100回 （または50回） | 250,000 | Googleの品質向上（学習）に使用される可能性あり |

* ※無料枠で入力したプロンプトや生成データは、Googleのモデル学習に利用される規約になっているため、機密情報の入力は避けてください。
* ※いずれかの制限を1つでも超えると 429 RESOURCE_EXHAUSTED エラーが発生し、一時的に呼び出せなくなります。


## LangChainで使用する
- `.env` ファイルに環境変数を追加する（`<YOUR GOOGLE API KEY>`部分を差し替える）
```
GOOGLE_API_KEY=<YOUR GOOGLE API KEY>
```

- Pythonのサンプルコード
```python
from langchain_google_genai import ChatGoogleGenAI

# 通常はAPIキーは指定しなくてOK
llm_dev = ChatGoogleGenAI(
    model="gemini-1.5-flash",
)

# 別のキーで呼び出す場合
llm_prod = ChatGoogleGenAI(
    model="gemini-1.5-flash",
    google_api_key="<YOUR GOOGLE API KEY>"
)


# .env を使用しないときのサンプル
import os
from langchain_google_genai import ChatGoogleGenAI
from langchain_core.messages import HumanMessage

# 1. APIキーを環境変数に設定
os.environ["GOOGLE_API_KEY"] = "あなたのAPIキー"

# 2. モデルの初期化 (例: gemini-1.5-flash または gemini-2.5-pro など)
llm = ChatGoogleGenAI(model="gemini-1.5-flash")

# 3. メッセージの定義
messages = [
    HumanMessage(content="Google AI StudioとLangChainの連携について教えてください。")
]

# 4. 実行
response = llm.invoke(messages)
print(response.content)
```

- Free plan では 1分間に15回などの制限があるのでリトライを設定しておくべき
```python
from langchain_google_genai import ChatGoogleGenAI

# max_retriesを指定すると、429エラー時に自動で待機してくれます
llm = ChatGoogleGenAI(
    model="gemini-2.5-flash",
    max_retries=6  # エラー時に最大6回まで自動でリトライする
)
```

- Freeプランで選択できるLLMのモデル
```
# gemini-3.1-flash-lite
# gemini-3-flash-preview
# gemini-2.5-pro 
# gemini-2.5-flash

# 最もオススメの組み合わせ
llm = ChatGoogleGenAI(
    model="gemini-2.5-pro",  # 無料枠で一番賢いマルチモーダル
    max_retries=6            # 回数制限対策
)
```

