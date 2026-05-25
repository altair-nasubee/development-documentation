## Slack の appと連携

### 1. Slack API の管理画面へアクセス
https://api.slack.com/apps

→ Google Workspace のメールで Slack にログイン

### 2. 「Create New App」をクリック
- 2つの選択肢が出ます：
    - From scratch（ゼロから作る）
    - From manifest（YAML/JSON で一括設定）

From scratch が安全。

### 3. アプリ名とワークスペースを選択
- App Name：自由に設定
- Development Workspace：あなたのワークスペースを選ぶ

### 4. アプリに必要な機能を追加
- 設定画面が表示される
- 画面左のサイドバーのメニューでOAuth & Permissionsを開く
- Scopes > Bot Token Scopes > [Add an OAuth Scope] で以下の権限を追加する
    | OAuth scope | comment |
    |-------------|---------|
    | chat:write  | app としてメッセージを送信する |
    | users:read  | Workspaceのメンバーを閲覧 |
    | channels:read  | publicのチャンネルの基本情報を読む |
    | channels:history  | app が連携されたpublicチャンネルのメッセージなどのコンテンツにアクセスする |
- OAuth Tokens > [Install to xxxxxx] を押して、許可する
- OAuth Tokens > Bot User OAuth Token の値をコピー（`xoxb-` で始まる文字列。各自の値を貼り付ける）
- コピーした値で環境変数SLACK_USER_TOKENを設定する
- あとは、Slackと連携したアプリでSLACK_USER_TOKENを使用すればアクセスできる。
```python
"""Slack連携サンプルコード
- 実行条件
    - .envファイルにSLACK_USER_TOKENをかいておき、initialize_dotenv()で環境変数に読み込む
"""
# OpenAI API 共通処理
from ..common.prepare_env import initialize_dotenv
import os
initialize_dotenv()

# SlackのToolを準備
from langchain_community.agent_toolkits import SlackToolkit

toolkit = SlackToolkit()
tools = toolkit.get_tools()

print(f"\n=== tools ===\n{tools}")

# LangChainのAgentでSlackのToolを使用する
from langchain.chat_models import ChatOpenAI
from langchain.agents import AgentType, initialize_agent

llm = ChatOpenAI(model="gpt-4o-mini", temperature=0.5)

agent_executor = initialize_agent(
    llm=llm,
    tools=tools,
    agent=AgentType.STRUCTURED_CHAT_ZERO_SHOT_REACT_DESCRIPTION,
)

result = agent_executor.invoke({"input": "「ai-functionality-tests」チャンネルに「Hello」と送信して"})
print(f"\n=== result['input'] ===\n{result['input']}")
print(f"\n=== result['output'] ===\n{result['output']}")

result = agent_executor.invoke({"input": "「ai-functionality-tests」チャンネルに投稿された最新のメッセージを表示して"})
print(f"\n=== result['input'] ===\n{result['input']}")
print(f"\n=== result['output'] ===\n{result['output']}")

result = agent_executor.invoke({"input": "チャンネル名を一覧化して"})
print(f"\n=== result['input'] ===\n{result['input']}")
print(f"\n=== result['output'] ===\n{result['output']}")

member_id = os.getenv("SLACK_TARGET_USER")

result = agent_executor.invoke({"input": f"「ai-functionality-tests」チャンネルで、ID「{member_id}」のメンバーにメンションを当てて「こんにちは」と送信して"})
print(f"\n=== result['input'] ===\n{result['input']}")
print(f"\n=== result['output'] ===\n{result['output']}")
```

- 備考：チャンネルにメッセージを投稿するには
    - Slackで該当のチャンネルの詳細を開く
    - インテグレーション > App > アプリを追加する で作成したアプリを追加しておく必要がある。


