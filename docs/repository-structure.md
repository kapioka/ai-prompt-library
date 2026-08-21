# リポジトリ構成ルール

このリポジトリは、AI向けプロンプトを長期的に追加・更新しやすい形で公開するために、用途別カテゴリと共通テンプレートを分離して管理します。

## 基本構成

```text
ai-prompt-library/
├─ README.md
├─ LICENSE
├─ prompts/
│  ├─ README.md
│  └─ <category>/
│     └─ <prompt-name>.md
├─ templates/
│  └─ prompt-template.md
└─ docs/
   └─ repository-structure.md
```

## 各ディレクトリの役割

### `prompts/`

公開プロンプトの正本を置きます。

プロンプトは用途別カテゴリに分け、1プロンプト1ファイルを原則とします。

例：

```text
prompts/market-analysis/weekly-global-market-monitor.md
prompts/research/fact-check.md
prompts/writing/rewrite-assistant.md
```

実際にプロンプトが追加された時点でカテゴリを作成し、将来用途のためだけの空カテゴリは作りません。

### `templates/`

新しいプロンプトを追加するときの共通雛形を置きます。

### `docs/`

命名規則、運用ルール、将来的な公開方法など、プロンプト本文ではない説明文書を置きます。

### `LICENSE`

リポジトリ全体の既定ライセンスを定義します。

個別ファイルに別条件が明記されていない限り、このリポジトリの公開内容には CC0 1.0 Universal を適用します。

第三者が権利を有する商標、外部データ、引用物などの権利は、このライセンス適用によって放棄されるものではありません。

## 命名規則

ディレクトリ名とファイル名は英小文字の `kebab-case` を使用します。

例：

- `market-analysis`
- `weekly-global-market-monitor.md`
- `fact-check.md`

表示タイトルと本文は日本語で構いません。

## プロンプトファイルの基本項目

各プロンプトは原則として次を持ちます。

1. YAML front matter
   - `title`
   - `slug`
   - `category`
   - `language`
   - `status`
   - `version`
   - `last_updated`
2. 概要
3. 想定用途
4. 前提・必要機能
5. プロンプト本文
6. 更新履歴

## バージョン管理

意味のある変更があった場合は、プロンプト単位でバージョンを更新します。

- 誤字修正・説明文のみ：PATCH
- 判定条件・出力形式など互換性を維持した改善：MINOR
- プロンプトの目的、主要ロジック、互換性が大きく変わる変更：MAJOR

例：`1.0.0` → `1.1.0` → `2.0.0`

## READMEとの関係

ルート `README.md` は公開入口、`prompts/README.md` はプロンプト索引として使用します。

プロンプトを追加した場合は `prompts/README.md` にリンクを追加し、代表的なものはルート `README.md` にも掲載します。

## 将来の拡張

必要になった時点で、次のようなカテゴリや補助ファイルを追加できます。

- `research/`
- `writing/`
- `coding/`
- `work/`
- `data-analysis/`
- `image-generation/`
- `.github/` のIssue / Pull Requestテンプレート
- GitHub Pages用の公開サイト

ただし、実際の用途が発生するまでは構造だけを先回りして増やしません。
