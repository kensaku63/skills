# Skills by kensaku63

AI エージェント向けのカスタムスキル集です。Claude Code をはじめ、様々なエージェントで使えるスキルを公開しています。

A collection of custom skills for AI agents — Claude Code, Cursor, and beyond.

## Installation

### Claude Code

```bash
npx skills add kensaku63/skills
```

特定のスキルだけをインストール：

```bash
npx skills add kensaku63/skills --skill deep-research
```

### Other Agents

各スキルの `SKILL.md` を対象エージェントの設定ディレクトリにコピーして使えます。詳細は各スキルの説明を参照してください。

---

## Skills

### Claude Code

| Skill | Description |
|-------|-------------|
| [deep-research](#deep-research) | X・Web・論文を横断した多角的ディープリサーチ |

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

> **Note**: このスキルは Phase 2 の X (Twitter) 調査で [bird](https://github.com/kensaku63/bird) スキルを使用します。
>
> bird スキルがインストールされていない場合、Phase 2 はスキップされ、Phase 3 の Web 調査のみで実施されます。フル機能を使うには、事前に bird スキルをセットアップしてください。

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

## Repository Structure

```
skills/
├── README.md
├── LICENSE
├── deep-research/          # Claude Code skill
│   └── SKILL.md
├── your-new-skill/         # Add new skills here
│   └── SKILL.md
└── ...
```

スキルを追加するには：

1. スキル名のディレクトリを作成
2. `SKILL.md` を配置（YAML frontmatter + 本文）
3. 必要に応じて `references/` や `scripts/` サブディレクトリを追加

## License

MIT
