# Claude Code 内部データ クリーンアップレポート

**実施日:** 2026年6月25日

削除済みワークスペースに紐づく Claude Code の履歴・キャッシュ・設定（`~/.claude/` 配下および `~/.claude.json`）を整理した記録。

## 概要

| 項目 | 値 |
|------|-----|
| 削除対象ワークスペース数 | 6 |
| `~/.claude/projects/`（会話履歴） | 6 ディレクトリ / 37 ファイル |
| `~/.claude/session-env/`（セッション環境） | 10 ディレクトリ |
| `~/.claude/file-history/`（ファイル編集履歴） | 9 ディレクトリ / 136 ファイル |
| `~/.claude/plans/`（プラン） | 2 ファイル |
| 変更ファイル | `~/.claude.json` / `history.jsonl` / `plugins/installed_plugins.json` |
| バックアップ | `~/claude-cleanup-backup-20260625/` |

### 削除済みと判定したワークスペース（6件）

各プロジェクトのトランスクリプト内 `cwd` を確認し、実体ディレクトリが存在しないことを検証して特定。

| 実パス | スラッグ |
|------|---------|
| `/home/defaultuser/work/Claude_Code_Tutorial_01_Home_Page_Creation` | `-home-defaultuser-work-Claude-Code-Tutorial-01-Home-Page-Creation` |
| `/home/defaultuser/work/Claude_Code_Tutorial_02_Streamlit_Project_Creation` | `-home-defaultuser-work-Claude-Code-Tutorial-02-Streamlit-Project-Creation` |
| `/home/defaultuser/work/ClaudeCodeLearning` | `-home-defaultuser-work-ClaudeCodeLearning` |
| `/home/defaultuser/work/claude_code_tutorial` | `-home-defaultuser-work-claude-code-tutorial` |
| `/home/defaultuser/work/claude_code_tutorial/sample_hp` | `-home-defaultuser-work-claude-code-tutorial-sample-hp` |
| `/home/defaultuser/work/favorite-video-curation` | `-home-defaultuser-work-favorite-video-curation` |

### 保持したワークスペース（現存・対象外）

- `ai-docs-rag-bot`
- `development-documentation`
- `grocery-deal-recipes-streamlit`
- `japan-weather-demo`
- `nyarchive`
- `test`

---

## 1. ~/.claude/projects/ 削除対象（6 ディレクトリ / 37 ファイル）

#### `-home-defaultuser-work-Claude-Code-Tutorial-01-Home-Page-Creation`

- **元ワークスペース:** `Claude_Code_Tutorial_01_Home_Page_Creation`
- **ファイル数:** 1

<details>
<summary>削除ファイル一覧（1 件）</summary>

- `175adc7f-5a3f-42e1-9b54-9c26780d68b2.jsonl`

</details>

#### `-home-defaultuser-work-Claude-Code-Tutorial-02-Streamlit-Project-Creation`

- **元ワークスペース:** `Claude_Code_Tutorial_02_Streamlit_Project_Creation`
- **ファイル数:** 2

<details>
<summary>削除ファイル一覧（2 件）</summary>

- `b4319b0e-5b9d-4968-92ba-ea2c8fa1fd9d.jsonl`
- `f761978d-ffe1-4dcd-94cd-33dd2c9e34cf.jsonl`

</details>

#### `-home-defaultuser-work-ClaudeCodeLearning`

- **元ワークスペース:** `ClaudeCodeLearning`
- **ファイル数:** 4

<details>
<summary>削除ファイル一覧（4 件）</summary>

- `45dc730a-5bb5-49f9-abda-54b39b75731d.jsonl`
- `65ce4d26-4eac-4466-8a67-28663951caf9.jsonl`
- `65ce4d26-4eac-4466-8a67-28663951caf9/tool-results/toolu_01229cZB4CDbLYHqHeR1SUCZ.txt`
- `cc1f3367-1969-4704-9c27-c2b3d00b2370.jsonl`

</details>

#### `-home-defaultuser-work-claude-code-tutorial`

- **元ワークスペース:** `claude_code_tutorial`
- **ファイル数:** 8

<details>
<summary>削除ファイル一覧（8 件）</summary>

- `4af9ebbc-08da-4773-8d5e-4f4f866a1f98.jsonl`
- `4af9ebbc-08da-4773-8d5e-4f4f866a1f98/subagents/agent-aba3ddf6f8e3723df.jsonl`
- `4af9ebbc-08da-4773-8d5e-4f4f866a1f98/subagents/agent-aba3ddf6f8e3723df.meta.json`
- `4af9ebbc-08da-4773-8d5e-4f4f866a1f98/tool-results/toolu_01XX6Msqcz7MHfdW1pUNdtQk.txt`
- `76b42cdb-d6e2-4406-844d-253087282da7.jsonl`
- `8de6c7c9-c8c3-419c-a09c-86d29109082c.jsonl`
- `d29ec072-c69f-43be-85cc-eeae50083c5f.jsonl`
- `d9933847-4106-4677-bd5d-5d186126034a.jsonl`

</details>

#### `-home-defaultuser-work-claude-code-tutorial-sample-hp`

- **元ワークスペース:** `claude_code_tutorial/sample_hp`
- **ファイル数:** 10（memory 含む）

<details>
<summary>削除ファイル一覧（10 件）</summary>

- `1cfe8926-baf4-4ec2-817b-e959cacebfc5.jsonl`
- `9e48f57e-a825-4f18-bd7e-5645109bf75d.jsonl`
- `b19a2153-e60e-4b4b-a2a1-b539a3789e11.jsonl`
- `b19a2153-e60e-4b4b-a2a1-b539a3789e11/subagents/agent-a7f6ee8a2b573fc13.jsonl`
- `b19a2153-e60e-4b4b-a2a1-b539a3789e11/subagents/agent-a7f6ee8a2b573fc13.meta.json`
- `b19a2153-e60e-4b4b-a2a1-b539a3789e11/subagents/agent-ace1b59e9718e6209.jsonl`
- `b19a2153-e60e-4b4b-a2a1-b539a3789e11/subagents/agent-ace1b59e9718e6209.meta.json`
- `ee0cc2ae-43e7-44cf-94c8-c0babb2f47d5.jsonl`
- `memory/MEMORY.md`
- `memory/design-preference.md`

</details>

#### `-home-defaultuser-work-favorite-video-curation`

- **元ワークスペース:** `favorite-video-curation`
- **ファイル数:** 12（memory 含む）

<details>
<summary>削除ファイル一覧（12 件）</summary>

- `0a81221b-ddfd-4343-8a3f-18a612ce7fed.jsonl`
- `1a903f9b-feff-423d-a80d-8a29e4467c24.jsonl`
- `1ac6e4b7-1019-4d5b-8ea3-6d5955f0f74c.jsonl`
- `3431b061-b9de-4d17-a64a-f1eada949da2.jsonl`
- `7ad22916-bb7f-4452-ad2a-eca411d675e1.jsonl`
- `902cad36-a906-463a-a40f-b1787c4e6963.jsonl`
- `902cad36-a906-463a-a40f-b1787c4e6963/tool-results/toolu_01TQWH9eCh1AAfu2Eb6jcgc6.txt`
- `d19086bf-af52-423f-820b-89de6fb659e5.jsonl`
- `e7eec790-b5af-408f-aaca-da8f356264a6.jsonl`
- `memory/MEMORY.md`
- `memory/deployment-urls.md`
- `memory/project-spec-decisions.md`

</details>

---

## 2. ~/.claude/session-env/ 削除対象（10 ディレクトリ）

セッションUUID単位のセッション環境ディレクトリ。削除済みワークスペースのセッションに該当するもののみ削除。

| セッションUUID | 元ワークスペース |
|---------------|-----------------|
| `175adc7f-5a3f-42e1-9b54-9c26780d68b2` | Claude_Code_Tutorial_01 |
| `b4319b0e-5b9d-4968-92ba-ea2c8fa1fd9d` | Claude_Code_Tutorial_02 |
| `f761978d-ffe1-4dcd-94cd-33dd2c9e34cf` | Claude_Code_Tutorial_02 |
| `45dc730a-5bb5-49f9-abda-54b39b75731d` | ClaudeCodeLearning |
| `9e48f57e-a825-4f18-bd7e-5645109bf75d` | claude_code_tutorial/sample_hp |
| `b19a2153-e60e-4b4b-a2a1-b539a3789e11` | claude_code_tutorial/sample_hp |
| `1a903f9b-feff-423d-a80d-8a29e4467c24` | favorite-video-curation |
| `7ad22916-bb7f-4452-ad2a-eca411d675e1` | favorite-video-curation |
| `902cad36-a906-463a-a40f-b1787c4e6963` | favorite-video-curation |
| `d19086bf-af52-423f-820b-89de6fb659e5` | favorite-video-curation |

---

## 3. ~/.claude/file-history/ 削除対象（9 ディレクトリ / 136 ファイル）

セッションUUID単位のファイル編集履歴（`<hash>@vN` 形式のスナップショット）。

| セッションUUID | 元ワークスペース | ファイル数 |
|---------------|-----------------|-----------|
| `175adc7f-5a3f-42e1-9b54-9c26780d68b2` | Claude_Code_Tutorial_01 | 1 |
| `f761978d-ffe1-4dcd-94cd-33dd2c9e34cf` | Claude_Code_Tutorial_02 | 2 |
| `65ce4d26-4eac-4466-8a67-28663951caf9` | ClaudeCodeLearning | 1 |
| `9e48f57e-a825-4f18-bd7e-5645109bf75d` | claude_code_tutorial/sample_hp | 11 |
| `b19a2153-e60e-4b4b-a2a1-b539a3789e11` | claude_code_tutorial/sample_hp | 3 |
| `1a903f9b-feff-423d-a80d-8a29e4467c24` | favorite-video-curation | 5 |
| `7ad22916-bb7f-4452-ad2a-eca411d675e1` | favorite-video-curation | 106 |
| `902cad36-a906-463a-a40f-b1787c4e6963` | favorite-video-curation | 1 |
| `e7eec790-b5af-408f-aaca-da8f356264a6` | favorite-video-curation | 6 |

> 各ファイルは内容スナップショット（ハッシュ名）のため個別ファイル名は省略。完全な実体はバックアップに保全済み。

---

## 4. ~/.claude/plans/ 削除対象（2 ファイル）

トランスクリプトから所有プロジェクトを逆引きして判定。

| ファイル | 由来ワークスペース |
|---------|-------------------|
| `polymorphic-greeting-rose.md` | favorite-video-curation（削除済） |
| `zippy-wibbling-papert.md` | favorite-video-curation（削除済） |

> `gemini-velvet-wigderson.md`（ai-docs-rag-bot 由来）、`web-web-web-precious-eich.md`（nyarchive 由来）は現存プロジェクトのため **保持**。

---

## 5. 変更（編集）したファイル

### `~/.claude.json`

`projects` セクションから削除済み6ワークスペースのキーを除去（残り6キーは保持）。

<details>
<summary>除去したキー（6 件）</summary>

- `/home/defaultuser/work/claude_code_tutorial`
- `/home/defaultuser/work/claude_code_tutorial/sample_hp`
- `/home/defaultuser/work/Claude_Code_Tutorial_01_Home_Page_Creation`
- `/home/defaultuser/work/Claude_Code_Tutorial_02_Streamlit_Project_Creation`
- `/home/defaultuser/work/favorite-video-curation`
- `/home/defaultuser/work/ClaudeCodeLearning`

</details>

### `~/.claude/history.jsonl`

コマンド入力履歴から削除済み6ワークスペース由来の **166 行** を除去（182 行を保持）。

> 現存プロジェクト（ai-docs-rag-bot）のエントリ本文が旧名（favorite-video-curation）に言及している1行は、所属が現存プロジェクトのため保持。

### `~/.claude/plugins/installed_plugins.json`

favorite-video-curation に紐づく `project` スコープのプラグイン導入エントリ **1 件** を除去（`user` スコープは保持）。

---

## 6. 対象外として保持したもの

| 場所 | 理由 |
|------|------|
| `~/.claude/backups/` | グローバル設定 `.claude.json` のローリングバックアップ（全プロジェクト横断・ワークスペース固有ではない） |
| `~/.claude/tasks/` | 現存プロジェクト（nyarchive）のもののみ |
| `~/.claude/sessions/`, `shell-snapshots/`, `cache/`, `paste-cache/`, `ide/` | 現行セッション／汎用キャッシュのみ |

---

## 7. バックアップと注意点

### バックアップ（復元用）

削除・変更前の全データを下記に保全済み。不要であればフォルダごと削除可。

- ディレクトリ: `~/claude-cleanup-backup-20260625/`
- 含むもの: 上記 1〜4 の削除データ一式、`claude.json.orig`、`history.jsonl.orig`、`installed_plugins.json.orig`、`CLEANUP-REPORT.md`

### 注意点

1. **memory の扱い** — `favorite-video-curation` / `claude-code-tutorial-sample-hp` の memory には、デプロイURL（`deployment-urls.md`）や設計判断（`project-spec-decisions.md`、`design-preference.md`）が含まれていた。ワークスペース削除に伴い対象としたが、外部にデプロイ済みのアプリ等が残っている場合はバックアップから内容を確認できる。
2. **書き戻しの可能性** — `.claude.json` と `history.jsonl` は、本クリーンアップを実行中の Claude Code セッション終了時に書き戻される可能性がある。クリーン結果を確定させるには、現セッション終了後の再確認を推奨。
