# claude-code-setup プラグインの使い方

Claude Code公式プラグイン「claude-code-setup」の概要と、実際にインストールして使うための手順です。

## 概要

`claude-code-setup` は Anthropic 公式の Claude Code プラグインで、コードベースを解析し、開発ワークフローに合った Claude Code の自動化設定(hooks・skills・MCPサーバー・subagentsなど)を提案してくれます。読み取り専用で動作するため、提案するだけでプロジェクトファイルを勝手に書き換えることはありません。

公式マーケットプレイス `claude-plugins-official`(GitHub: `anthropics/claude-plugins-official`)上で配布されており、以下のコマンドで実在・詳細を確認できます。

```bash
claude plugin list --available --json | python3 -c "
import json,sys
data = json.load(sys.stdin)
for p in data['available']:
    if p['name'] == 'claude-code-setup':
        print(p['pluginId'], '-', p['description'])
"
```

出力例:

```
claude-code-setup@claude-plugins-official - Analyze codebases and recommend tailored Claude Code automations such as hooks, skills, MCP servers, and subagents.
```

## 主な機能

プロジェクトの構造・依存関係・コーディングパターンを検出し、以下のカテゴリにわたって提案を生成する。

- **MCPサーバー** — 技術スタックに合った連携の提案
- **Skills** — プロジェクトに合わせたカスタムコマンド
- **Hooks** — コマンド実行前後の自動処理
- **Subagents** — 特定タスク用の専門エージェント

## インストール手順

WSL側のターミナルで、いつも使っている作業ディレクトリに移動してから以下を実行する(Claude Code実行中なら `/plugin install` でも同様)。

```bash
# 公式マーケットプレイスが未登録の場合のみ実行(通常は最初から登録済み)
claude plugin marketplace add anthropics/claude-plugins-official

# プラグイン本体をインストール(デフォルトはユーザースコープ)
claude plugin install claude-code-setup@claude-plugins-official
```

Claude Codeの対話セッション内から行う場合は、スラッシュコマンドで同様の操作ができる。

```
/plugin install claude-code-setup@claude-plugins-official
```

インストール後、変更を反映させるにはセッションの再起動、または `/reload-plugins` を実行する。

マーケットプレイスの登録状況は次のコマンドで確認できる。

```bash
claude plugin marketplace list
```

## 典型的な使用場面

- プロジェクト立ち上げ時に「Claude Codeのセットアップを手伝って」と依頼する
- 「どんなhookを使うべき?」のようにワークフロー最適化を相談する
- 新しいコードベースに着手する際に、パーソナライズされた推奨設定を得る
- プロジェクトの技術スタックに合ったMCPサーバーを発見する

## 補足: このプロジェクトでの必要性

このプロジェクト(仕事探し用資料の作成・編集)自体は一般的なソフトウェア開発プロジェクトではないため、`claude-code-setup` が提案する自動化(コード解析ベースのhooks/MCP/subagent提案など)が直接役立つ場面は少ない。興味本位で試す分には問題ないが、導入は必須ではない。
