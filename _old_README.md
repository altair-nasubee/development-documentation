## Steps to Setup
```sh
cd C:\dev\Streamlit\dmm_ai_learning
mise install
mise use python@3.11

python --version
確認したら

python -m venv .venv
.venv\Scripts\activate

python.exe -m pip install --upgrade pip
pip install -r requirements.txt


streamlit run app.py

個別の.pyを実行宇するなら
python xxx.py
```

## Steps to Re-Setup

- VSCodeのエディタを閉じる

- `.venv`フォルダ削除

- PowerShell7のコンソールで以下を実行
```sh
python -m venv .venv
.venv\Scripts\activate

python.exe -m pip install --upgrade pip
pip install -r requirements.txt
```

## Pythonの開発環境をWSL上においてVSCode + Codexの拡張機能を使う。

### 1. VSCodeの拡張機能：Remote Developmentをインストール

- 本件についての説明
https://learn.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-vscode


- Remote Development
https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack

### 2. VSCodeの拡張機能：Codex – OpenAI’s coding agentをインストール
https://marketplace.visualstudio.com/items?itemName=openai.chatgpt

### 3. Ubuntuの設定更新

```sh
sudo apt-get update

sudo apt-get install wget ca-certificates
```

### 4. PythonのプロジェクトをUbuntu上に作成する

```sh
sudo apt update
sudo apt install -y python3 python3-pip python3-venv

mkdir -p ~/work/dmm_ai_learning
cd ~/work/dmm_ai_learning

# Windows環境からUbuntuへ環境をコピーする場合
rsync -avh /mnt/c/dev/Streamlit/dmm_ai_learning/ ~/work/dmm_ai_learning/

# プロジェクトフォルダ内__pycache__フォルダを全削除
find . -type d -name "__pycache__" -exec rm -r {} +

# Homeディレクトリ以下の*:Zone.Identifierファイルを全削除
find ~ -name "*:Zone.Identifier" -delete 2>/dev/null

# 仮想環境構築
python3 -m venv .venv
source .venv/bin/activate
# Note:
# Windows上でPythonの仮想環境に入るコマンドは`source .venv/bin/activate`じゃなくて
#     `.venv\Scripts\Activate.ps1`
# もしくは
#     `.venv\Scripts\Activate.bat`
# ちょっと違うので注意。

python -m pip install -U pip
python -m pip install <必要なパッケージ>

# 現在の環境をrequirements.txtに保存
# （指定する必要ないパッケージも書き出されるので実行後に手作業でメンテする必要あり。）
pip freeze > requirements.txt

# 環境を再現するにはrequirements.txtでまとめてインストール
pip install -r requirements.txt

# 仮想環境から抜ける
deactivate

# VSCodeのPathを通すためにシンボリックリンク作成
sudo ln -s "/mnt/c/Users/kiyoshi/AppData/Local/Programs/Microsoft VS Code/bin/code" /usr/local/bin/code

# Ubuntu上からVSCodeを起動
code .

# CursorのPathを通すためにシンボリックリンク作成
sudo ln -s "/mnt/c/Users/kiyoshi/AppData/Local/Programs/cursor/resources/app/bin/cursor" /usr/local/bin/cursor

# Ubuntu上からCursorを起動
cursor .


# Windows上でWSL2のファイルをコピーするとコピー後に不要なファイルが生成される
# （ファイル末尾が:Zone.Identifierのファイル）
# これをhomeディレクトリ以下に対して一括削除するには、
find ~ -name "*:Zone.Identifier" -delete 2>/dev/null
```

## CodexもGitHubCopilotもお金がかかるのでGemini Code Assistにする。
他のAIの拡張機能をすべてアンインストール
（Windows上と、WSL上に残っている拡張機能のキャッシュはすべて削除する）

Gemini Code Assistをインストール
https://marketplace.visualstudio.com/items?itemName=Google.geminicodeassist

VSCodeを再起動

Gemini Code Assistにログイン
Google Workspaceのビジネス用のアカウントを使用すると、有料プランになる。
今回は、個人のGoogleアカウントnasubee@pa2.so-net.ne.jpでログインしなおして使用することにした。

.aiexcludeというファイルを作成して以下のように記載して、APIキーなどの情報をAIが見ないようにする
```
# 個人情報や環境変数が書かれたファイルを対象外にする
.env
```

Gemini Code Assistのペインの右上にある3点リーダーのアイコン > Privacy Settings
のCheckをOFFにして、開発した成果物をAIが学習に使用しないようにしておく。

※詳しくは↓
https://developers.google.com/gemini-code-assist/docs/set-up-gemini?hl=ja


