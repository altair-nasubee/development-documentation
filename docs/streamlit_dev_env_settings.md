# Python and Streamlit 開発環境構築手順
WSL上でPython（Streamlit）のプロジェクトを開発

## 開発条件
- Windows11 and WSL2
- Ubuntu24.04をインストール
- Ubuntu内のフォルダにGitHubのリポジトリをCloneする
- IDEはCursorを使用
- Streamlitの安定動作のために Python 3.11 を使用する

## 開発環境構築手順
### Ubuntuのターミナルを開いてSSHキーを作成（作成済みならスキップ）
```sh
# {your email}の部分を自分のものに置換して実行する
$ ssh-keygen -t ed25519 -C "{your email}"
# 色々聞いてくるけど[Enter]3回押下

# 公開鍵を表示
$ cat ~/.ssh/id_ed25519.pub

# 表示された公開鍵をコピーしておく
```

### GitHubに公開鍵を登録（登録済みならスキップ）
- GitHubの自分のアカウントのページで Settings > SSH and GPG keys を開く
- SSH Keys > [New Key] 押下
- 適当な名前を付けて公開鍵を貼り付けて登録完了


### Ubuntuのコンソールを開いて以下のコマンドでリポジトリをクローンする
```sh
# 念のためGitを準備するコマンド実行
sudo apt update && sudo apt upgrade -y
sudo apt install git -y

# gitが使えるか確認
$ git --version

$ mkdir -p ~/work
$ cd ~/work

# Gitの初期設定（１度だけ実行）
# {your name}と{your email}は各自差し替えて実行
$ git config --global user.name "{your name}"
$ git config --global user.email "{your email}"

# 内容確認
$ git config --global --list
# git config に以下の設定が追加されたことを確認
# user.name=名前
# user.email=メールアドレス

$ git clone git@github.com:altair-nasubee/streamlit-llm-app.git
# GitHubへの初回アクセス時に
# Are you sure you want to continue connecting (yes/no/[fingerprint])? 
# と表示されたら yes [Enter]
```

ここまでで `~/work` の下にリポジトリ名のフォルダがクローンされているはず

### ローカルのリポジトリに入ってCursor起動
```sh
#$ cd ~/work
$ cd streamlit-llm-app
$ cursor .
```


### Steps to create python development
以下は、Cursorのターミナルで実行

```sh
# Ubuntuの事前準備
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3 python3-pip python3-venv

# Streamlitでは安定板のPython 3.11を使用して仮想環境を構築する
# 1. 外部リポジトリ（PPA）を追加するためのツールをインストール
sudo apt install -y software-properties-common

# 2. Python 3.11が提供されているPPAを追加（Enterを求められたら押す）
sudo add-apt-repository ppa:deadsnakes/ppa

# 3. リポジトリを追加したため、パッケージリストを最新化
sudo apt update

# 4. Python 3.11本体と、3.11専用のvenvパッケージを追加インストール
sudo apt install -y python3.11 python3.11-venv

# ビルド用パッケージ（hnswlib などで使用）も追加
sudo apt install -y python3.11-dev build-essential

# この状態だと python --version は失敗する
#   python3 --version (Python 3.12.3)
#   や
#   python3.11 --version (Python 3.11.15)
#   を使う必要あり。

# 仮想環境を作りたいディレクトリに移動
cd ~/work/myProject

# .venvフォルダにpython3.11の仮想環境構築
python3.11 -m venv .venv

# 仮想環境に入る
source .venv/bin/activate

# pip更新
pip install --upgrade pip

# streamlitのインストール
pip install streamlit==1.41.1

# 現在の環境をrequirements.txtに保存
# （指定する必要ないパッケージも書き出されるので実行後に手作業でメンテする必要あり。）
pip freeze > requirements.txt

# 環境を再現するにはrequirements.txtでまとめてインストール
pip install -r requirements.txt
```

## 開発方針

### 環境変数

- アプリで使用する `OPENAI_API_KEY` などの秘匿情報は環境変数で設定。
- ローカルでの動作は `.env` をロードして環境変数に設定する
- [Streamlit Community Cloud](https://share.streamlit.io) ではデプロイ時に環境変数を設定する

## コマンドメモ

### ローカルでのデバッグ
```sh
streamlit run app.py
```

### Git commands
```sh
# 編集したファイルの追加
git add .

# コミット
git commit -m "first commit."

# プッシュ
git push -u origin main

# Gitの設定確認
git config -l
```

### 仮想環境から抜ける
```sh
deactivate
```

### 仮想環境を削除する
```sh
# .venv フォルダごと削除
rm -r ./.venv

# プロジェクトフォルダ内の__pycache__フォルダを全削除
find . -type d -name "__pycache__" -exec rm -r {} +
```

### その他
- 不要なファイルの削除
```sh
# Windows上でWSL2のファイルをコピーすると不要なファイルが生成される
# （ファイル末尾が:Zone.Identifierのファイル）
# これをhomeディレクトリ以下に対して一括削除するには、
find ~ -name "*:Zone.Identifier" -delete 2>/dev/null
```
