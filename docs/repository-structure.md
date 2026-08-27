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
│     ├─ <prompt-name>.md
│     └─ <project-package>/
│        ├─ README.md
│        ├─ project-instructions.md
│        └─ <support-files>.md
├─ templates/
│  └─ prompt-template.md
└─ docs/
   └─ repository-structure.md
```

## 各ディレクトリの役割

### `prompts/`

公開プロンプトの正本を置きます。

通常は用途別カテゴリに分け、1プロンプト1ファイルを原則とします。

例：

```text
prompts/market-analysis/weekly-global-market-monitor.md
prompts/research/fact-check.md
prompts/writing/rewrite-assistant.md
```

実際にプロンプトが追加された時点でカテゴリを作成し、将来用途のためだけの空カテゴリは作りません。

### Project Package

ChatGPT Projectなどで、**Instructionsと補助ファイルを分離することが実行品質、理解しやすさ、保守性を実際に改善する場合**は、1プロンプト1ファイルの例外としてProject Packageを使用できます。

```text
prompts/<category>/<package-name>/
├─ README.md
├─ project-instructions.md
└─ <support-files>.md
```

ルール：

- `README.md` を公開入口とする
- `project-instructions.md` を中核の実行指示とする
- 補助ファイルは、テンプレート、評価・レビュー、参照資料など実際に必要なものだけ置く
- READMEで各ファイルを「必須」「推奨」「任意」に分類する
- 同じルールを複数ファイルへ無用に重複させない
- 補助ファイルがなくても中核機能が成立することが望ましい
- 単一ファイルで同等品質を維持できる場合はProject Packageを作らない
- パッケージ全体のversion / last_updatedはREADMEのfront matterを正本とする

### `templates/`

新しい単一プロンプトを追加するときの共通雛形を置きます。

Project Package用テンプレートは、同形式のパッケージが複数必要になった時点で追加を検討します。最初の1件だけを理由に空の共通構造を先回りして増やしません。

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
- `japanese-company-fundamental-analyzer`

表示タイトルと本文は日本語で構いません。

## プロンプトファイルの基本項目

単一プロンプトは原則として次を持ちます。

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

Project Packageでは、パッケージの `README.md` に同等のmetadata、概要、必要機能、セットアップ、更新履歴を持たせます。

## バージョン管理

意味のある変更があった場合は、プロンプト単位またはProject Package単位でバージョンを更新します。

- 誤字修正・説明文のみ：PATCH
- 判定条件・出力形式など互換性を維持した改善：MINOR
- プロンプトの目的、主要ロジック、互換性が大きく変わる変更：MAJOR

例：`1.0.0` → `1.1.0` → `2.0.0`

## READMEとの関係

ルート `README.md` は公開入口、`prompts/README.md` はプロンプト索引として使用します。

単一プロンプトを追加した場合は `prompts/README.md` にリンクを追加し、代表的なものはルート `README.md` にも掲載します。

Project Packageはパッケージ内 `README.md` へリンクします。

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
