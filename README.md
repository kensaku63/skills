# Skills by kensaku63

AI エージェント向けのカスタムスキル集です。Claude Code をはじめ、様々なエージェントで使えるスキルを公開しています。

A collection of custom skills for AI agents — Claude Code, Cursor, and beyond.

## Installation

### Claude Code

すべてのスキルをインストール：

```bash
npx skills add kensaku63/skills
```

特定のスキルだけをインストール：

```bash
npx skills add kensaku63/skills --skill deep-research
npx skills add kensaku63/skills --skill bird
```

### Other Agents

各スキルの `SKILL.md` を対象エージェントの設定ディレクトリにコピーして使えます。詳細は各スキルの説明を参照してください。

---

## Skills

### Claude Code

| Skill | Description |
|-------|-------------|
| [deep-research](#deep-research) | X・Web・論文を横断した多角的ディープリサーチ |
| [bird](#bird) | X (Twitter) の閲覧・検索・投稿を CLI で操作 |

<!-- ### Cursor / Other Agents -->
<!-- 今後追加予定 -->

---

## deep-research

> **Agent**: Claude Code
>
> **Category**: Research

X (Twitter)・Web・論文を横断した多角的ディープリサーチスキル。

チームエージェントを活用し、6つのフェーズで徹底的な調査を行います：

| Phase | Content |
|-------|---------|
| 1. Needs Analysis | ヒアリングで調査テーマを具体化し、リサーチプランを策定 |
| 2. X Research | X (Twitter) で専門家の動向・論点を先行調査 |
| 3. Web Deep-Dive | 3つの Web リサーチャーが並列で深掘り調査 |
| 4. Integration | 全情報をファクトベースで整理・統合 |
| 5. Hypothesis | 事実を基に独自の仮説を構築 |
| 6. Delivery | レポート提出とフォローアップ |

### Output

2つのレポートファイルが生成されます：

- `~/deep-research-[theme]-[YYYYMMDD].md` — ファクトレポート（事実の整理）
- `~/deep-research-[theme]-hypothesis-[YYYYMMDD].md` — 仮説レポート（独自の考察）

### Prerequisites

> **Note**: このスキルは Phase 2 の X (Twitter) 調査で [bird](#bird) スキルを使用します。
>
> bird スキルがセットアップされていない場合、Phase 2 はスキップされ、Phase 3 の Web 調査のみで実施されます。フル機能で使うには、先に bird スキルのセットアップを完了してください。

### Usage

Claude Code で以下のように呼び出します：

```
deep research on [your topic]
```

```
[テーマ] についてディープリサーチして
```

Claude がヒアリングから始めて、自動的にチームエージェントを立ち上げて調査を進めます。

---

## bird

> **Agent**: Claude Code
>
> **Category**: Social / X (Twitter)
>
> **Original Author**: [Peter Steinberger](https://github.com/steipete) — bird CLI の作者に感謝します
>
> **Based on**: [openclaw/openclaw](https://github.com/openclaw/openclaw) (MIT License) の bird スキルをベースに、一部改変して再配布しています

X (Twitter) の閲覧・検索・投稿・エンゲージメントを CLI で操作するスキルです。

### Setup Guide

bird スキルを使うには、**bird CLI のインストール** と **X アカウントの認証** が必要です。

#### Step 1: bird CLI をインストール

Node.js 20 以上が必要です。お好みの方法でインストールしてください：

```bash
# npm
npm install -g @steipete/bird

# pnpm
pnpm add -g @steipete/bird

# bun
bun add -g @steipete/bird

# macOS のみ: Homebrew
brew install steipete/tap/bird
```

> **Tip**: インストールせずに試すなら `bunx @steipete/bird whoami` で OK

#### Step 2: X アカウントの認証

bird はブラウザの Cookie を使って X にアクセスします。

**方法 A: 環境変数（推奨）**

X にログイン中のブラウザから `auth_token` と `ct0` の Cookie 値を取得し、`~/.bashrc` (or `~/.zshrc`) に設定：

```bash
export AUTH_TOKEN="your_auth_token_here"
export CT0="your_ct0_here"
```

**方法 B: ブラウザ Cookie の自動取得**

```bash
bird check --cookie-source=chrome   # Chrome の Cookie を使う
bird check --cookie-source=firefox  # Firefox の Cookie を使う
```

#### Step 3: 動作確認

```bash
bird whoami    # ログイン中のアカウントが表示されれば OK
bird check     # 認証元の確認
```

### What You Can Do

| Command | Description |
|---------|-------------|
| `bird read <url>` | ツイートを読む |
| `bird search "query" -n 10` | キーワード検索 |
| `bird home` | ホームタイムライン |
| `bird user-tweets @handle` | ユーザーのツイート一覧 |
| `bird thread <url>` | スレッド全体を表示 |
| `bird bookmarks` | ブックマーク一覧 |
| `bird tweet "text"` | ツイートを投稿 |
| `bird reply <url> "text"` | リプライ |

全コマンドの詳細は `bird/SKILL.md` または [bird.fast](https://bird.fast/) を参照してください。

### Troubleshooting

**404 エラーが出る場合：**

```bash
bird query-ids --fresh    # GraphQL クエリ ID のキャッシュを更新
```

**Cookie の取得に失敗する場合：**

- ブラウザで X にログインしているか確認
- 別の `--cookie-source` を試す
- Arc / Brave の場合: `--chrome-profile-dir` でプロファイルパスを指定

---

## Repository Structure

```
skills/
├── README.md
├── LICENSE
├── deep-research/          # Deep research skill
│   └── SKILL.md
├── bird/                   # X (Twitter) CLI skill
│   └── SKILL.md
└── ...
```

スキルを追加するには：

1. スキル名のディレクトリを作成
2. `SKILL.md` を配置（YAML frontmatter + 本文）
3. 必要に応じて `references/` や `scripts/` サブディレクトリを追加

## Credits

- **bird CLI** — Created by [Peter Steinberger](https://github.com/steipete) ([@steipete](https://x.com/steipete)). The bird skill in this repository is based on the [openclaw/openclaw](https://github.com/openclaw/openclaw) bird skill (MIT License) with modifications. The bird CLI tool itself (`@steipete/bird`) is a separate package — please visit [bird.fast](https://bird.fast/) for the official documentation.

## License

MIT
