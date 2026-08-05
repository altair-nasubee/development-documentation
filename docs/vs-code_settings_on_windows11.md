# Windows11でVS Codeのセットアップ

## Windows 11 Pro + WSL2 + VS Codeで実現する最強AIコーディング＆漢字化エスペラント環境の構築手順
https://qiita.com/Esperanto_kanjijigo/items/8980962a9f33791071e8

## 作業手順のメモ
予めWSL2、Ubuntu24.04、VSCodeがインストール済みであるものとする。

VSCodeをまだインストールしていない場合は、wingetコマンドがオススメ。
以下のコマンドで一発インストールできる
```sh
winget install --id Microsoft.VisualStudioCode
```


### 1. Ubuntuをインストールしたら、VS Codeを起動
コマンドラインから、VSCodeを起動する場合は、
```sh
> code.cmd
```
で起動できるようになっているはず。

### 2. 拡張機能：Remote Developmentをインストール

- 本件についての説明
https://learn.microsoft.com/ja-jp/windows/wsl/tutorials/wsl-vscode


- Remote Development
https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack


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


# 仮想環境構築
python3 -m venv .venv
source .venv/bin/activate

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
```


### Tips: vi コマンドのおさらい

vi使うならvimを使おう
- `sudu vim {filep/etc/wsl.conf`
- コマンドモードと入力モードの切り替え
    - デフォルトはコマンドモード
    - [i] 入力モード ⇒ [Esc] コマンドモードに戻る
    - [i] でカーソル位置に文字を入力する
- コマンドモードで [x] ⇒ 1文字削除
- コマンドモードで :wq  ⇒ Saveして終了
    - Saveしない場合は、 :q で終了
    - Saveのみは :w

 