# Windows11ネイティブ環境でのClaude Codeインストール

WSL(Windows Subsystem for Linux)を使わず、Windows 11のPowerShell/CMD上に直接Claude Codeをインストール・セットアップする手順。

参考(公式ドキュメント):
- https://code.claude.com/docs/en/setup.md
- https://code.claude.com/docs/en/troubleshoot-install.md
- https://code.claude.com/docs/en/settings.md
- https://code.claude.com/docs/en/authentication.md

## 動作要件

- **OS**: Windows 10 (build 1809以降) / Windows 11 / Windows Server 2019以降、いずれも**64bit**のみ(32bit版は非対応)
- **メモリ**: 4GB以上
- **ネットワーク**: インターネット接続必須。[Anthropicがサービス提供している国](https://www.anthropic.com/supported-countries)であること
- **シェル**: PowerShellは標準搭載。加えて[Git for Windows](https://git-scm.com/downloads/win)を入れておくとGit Bashが使え、クロスプラットフォームなスクリプトとの互換性が上がる(推奨)
- **Node.js**: npm経由でインストールする場合のみNode.js 22以上が必要。公式インストーラやwingetを使う場合は不要

## インストール方法

### 方法1: 公式インストーラ(推奨)

PowerShellを開いて(プロンプトが `PS C:\Users\...>` になっているか確認)以下を実行する。

```powershell
irm https://claude.ai/install.ps1 | iex
```

CMD(プロンプトが `C:\Users\...>` )の場合はこちら。

```bat
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```

インストール先は `%USERPROFILE%\.local\bin\claude.exe`。バックグラウンドで自動アップデートされるため、追加の依存関係なしで最新状態を維持できる。管理者権限は不要。

バージョンを固定したい場合:

```powershell
# 1週間遅れの安定版チャンネルを使う
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) stable

# 特定バージョンを指定
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) 2.1.89
```

### 方法2: winget (`claude update`で更新できないので非推奨)

```powershell
winget install Anthropic.ClaudeCode
```

自動アップデートはされないので、更新は手動 (`winget upgrade Anthropic.ClaudeCode`)。パッケージマネージャー経由でアンインストールしたい場合はこちらが楽。

### 方法3: npm ※この方法でインストールした。

#### 事前準備としてNode.jsをインストールする
- Node.jsがインストールされているか確認
```sh
# PowerShellやコマンドプロンプトで
node -v
```

- Webで「node.js」を検索して公式サイトを開く
https://nodejs.org/ja

- ダウンロードでWin x64の最新の安定板をダウンロードする
    - Docker版ではなくて、画面の下のほうにあるx64用のWindowsのビルド済みのNode.jsをダウンロードする
    - Windowsインストーラー（.msi）
    - Typicalの設定で特に何も変えずに [Next] でインストールする

- Node.jsがインストールされたかどうか再度確認するときには、ターミナルを再起動する必要あり
- Node.jsがインストールされているかPowerShellやコマンドプロンプトで確認
```sh
node -v

npm -v
```

#### npm で Claude Code をインストール

- 以下のコマンドでインストールする
```powershell
npm install -g @anthropic-ai/claude-code
```

- 最新版をいきなりインストールするにはこちらで実行。
```powershell
npm install -g @anthropic-ai/claude-code@latest
```

> **補足**: npmインストールは実は公式インストーラ版と同じネイティブバイナリを裏側で取得する方式なので、更新の仕組みも共通。通常はバックグラウンドで自動アップデートされ、`claude update` も実際に更新を実行する。自動アップデートが効かないのはnpmのグローバルディレクトリに書き込み権限がない場合のみで、その場合は起動時に通知が出て `claude doctor` で対処法を確認できる(手動更新は `npm install -g @anthropic-ai/claude-code@latest`)。<br>これに対しHomebrew/WinGet/apt/dnf/apkでインストールした場合は自動アップデートなしで、`claude update` を実行しても実際には更新されず「Claude is up to date!」と表示されるだけ。パッケージマネージャー側の専用コマンド(`winget upgrade` 等)で更新する必要がある。

### インストール確認

```powershell
claude --version
claude doctor   # 診断コマンド
```

インストール直後の同じターミナルではPATHが反映されていないことがあるので、反映されない場合は**新しいターミナルウィンドウを開き直す**。

## 初回起動と認証

```powershell
claude
```

を実行すると対話セッションが起動する。認証方法は以下のいずれか。

1. **Claude.aiアカウント(Pro/Max/Team/Enterprise)** — 初回起動時にブラウザが自動で開きOAuthログイン。ブラウザが自動で開かない場合は `c` キーでURLをクリップボードにコピーして手動で開く。無料プランはCLIの対象外。
2. **Anthropic Console APIキー** — 環境変数 `ANTHROPIC_API_KEY` を設定してから起動。

```powershell
$env:ANTHROPIC_API_KEY = 'sk-ant-...'
claude
```

   恒久的に設定したい場合はPowerShellプロファイルに追記する。

```powershell
# プロファイルファイルのパスを確認
$profile
```

   表示されたパスのファイルに上記の `$env:ANTHROPIC_API_KEY = ...` を追記する。

3. Bedrock / Vertex AI / Microsoft Foundry経由の利用(上記とは別方式、詳細は公式ドキュメント参照)

## 設定ファイルの場所(ネイティブWindows)

| 種別 | パス |
|---|---|
| ユーザー設定 | `%USERPROFILE%\.claude\settings.json` |
| プロジェクト設定 | `.claude\settings.json`(プロジェクト直下) |
| ローカル専用設定(gitignore対象) | `.claude\settings.local.json` |
| MCPサーバー(ユーザースコープ) | `%USERPROFILE%\.claude.json` |
| MCPサーバー(プロジェクトスコープ) | `.mcp.json`(プロジェクト直下) |
| プロジェクトのコンテキスト | `CLAUDE.md`(プロジェクト直下) |
| 実行ファイル本体 | `%USERPROFILE%\.local\bin\claude.exe` |

PowerShellから確認する場合:

```powershell
$env:USERPROFILE
Get-Item "$env:USERPROFILE\.claude"
```

### シェルの切り替え設定

`%USERPROFILE%\.claude\settings.json` で使用シェルを指定できる。

```json
{
  "defaultShell": "powershell"
}
```

Git for Windowsを入れている場合はデフォルトでGit Bashが使われる。自動検出に失敗する場合はパスを明示する。

```json
{
  "env": {
    "CLAUDE_CODE_GIT_BASH_PATH": "C:\\Program Files\\Git\\bin\\bash.exe"
  }
}
```

## Windows特有の注意点

- **サンドボックス機能は非対応**: コマンド実行のサンドボックス化はWSL2上でのみ利用可能。ネイティブWindowsではすべてのコマンドがホスト環境で直接実行される。サンドボックス化が必要ならWSL2を使うこと。
- **PowerShell実行ポリシーでブロックされる場合**: `running scripts is disabled on this system` のようなエラーが出たら以下を実行。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

- **インストール時のファイルロックエラー**: 他のPowerShellウィンドウやアンチウイルスのスキャンが原因のことが多い。他のウィンドウを閉じてからダウンロードキャッシュを削除して再実行。

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\downloads"
irm https://claude.ai/install.ps1 | iex
```

- **アンチウイルス/エンドポイント保護**: `claude.exe` やそこから呼び出される `cmd.exe` / `bash.exe` が誤検知されることがある。必要に応じて許可リストに追加する。
- **推奨ターミナル**: Windows Terminal(UTF-8対応が良好)。`winget install Microsoft.WindowsTerminal` で導入可能。PowerShell/CMD/Git Bashいずれも動作する。
- **社内プロキシ配下**: インストール前に環境変数を設定する。

```powershell
$env:HTTP_PROXY = 'http://proxy.example.com:8080'
$env:HTTPS_PROXY = 'http://proxy.example.com:8080'
irm https://claude.ai/install.ps1 | iex
```

## アップデート

```powershell
claude update          # 手動更新(インストーラ版・npm版どちらも有効)
winget upgrade Anthropic.ClaudeCode   # winget版
npm install -g @anthropic-ai/claude-code@latest   # npm版(claude updateが使えない場合の代替)
```

インストーラ版・npm版はどちらもデフォルトでバックグラウンド自動更新される(npmはグローバルディレクトリに書き込み権限がある場合のみ)。チャンネルは `settings.json` で切り替え可能。

```json
{
  "autoUpdatesChannel": "latest"
}
```

## アンインストール

```powershell
# インストーラ版
Remove-Item -Path "$env:USERPROFILE\.local\bin\claude.exe" -Force
Remove-Item -Path "$env:USERPROFILE\.local\share\claude" -Recurse -Force

# winget版
winget uninstall Anthropic.ClaudeCode

# npm版
npm uninstall -g @anthropic-ai/claude-code
```

設定・MCPサーバー・セッション履歴も含めて完全に削除する場合(要注意、元に戻せない):

```powershell
Remove-Item -Path "$env:USERPROFILE\.claude" -Recurse -Force
Remove-Item -Path "$env:USERPROFILE\.claude.json" -Force
```
