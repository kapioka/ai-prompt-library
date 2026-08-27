# AI Prompt Library

ChatGPTやその他のAIツールで再利用できる、日本語中心の公開プロンプト集です。

単にプロンプト本文を置くだけでなく、用途、必要機能、バージョン、更新履歴を含めて管理し、継続的に改善できる形を目指します。

## Prompts

### 個別株・企業分析

- [日本株 企業業績・ファンダメンタル分析 — ChatGPT Project Package](prompts/equity-analysis/japanese-company-fundamental-analyzer/README.md)  
  日本の上場企業を、過去・現在・将来の順に調査し、KPI・セグメント、シナリオ・バリュエーション、最新開示・経営方針の3方向から段階的に分析するChatGPT Project向けパッケージです。

### 市場分析

- [週次グローバル市場モニター](prompts/market-analysis/weekly-global-market-monitor.md)  
  世界の主要株式4地域とVIX、米10年金利、DXY、HYG、金、原油を横断し、市場レジーム、急変リスク、Compression、レジーム転換、Cycle、地域順位を週次評価します。Exact Quantが利用できない場合も、定義済みRobust Fallbackでレポートを完成させる設計です。

### 食・飲料

- [ボトル・ラベルから楽しむ酒類ガイド](prompts/food-drink/bottle-label-drink-guide.md)  
  ワイン、日本酒、ウイスキー、焼酎のボトルやラベル画像から商品を識別し、酒類別の星評価、初心者向け解説、料理との相性、飲み方、条件を満たす場合は雑誌風の商品紹介画像まで生成するプロンプトです。

すべてのプロンプトは [プロンプト一覧](prompts/README.md) から確認できます。

## 使い方

### 単一プロンプト

1. 使用したいプロンプトのMarkdownファイルを開く
2. `プロンプト本文` 以降をコピーする
3. 対応するAIへ貼り付ける
4. 必要に応じて定期実行や追加条件を設定する

### Project Package

複数ファイルで構成されるProject Packageは、各パッケージの `README.md` を入口にします。

`project-instructions.md` をChatGPT ProjectのProject Instructionsへ貼り、READMEで「推奨」とされている補助ファイルをProject Filesへ追加してください。

各プロンプトには必要な機能や前提条件を記載しています。Web検索、市場データ、ファイル参照、高度計算などが必要なものは、利用環境によって結果が変わる場合があります。

## リポジトリ構成

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

詳しい命名・運用ルールは [リポジトリ構成ルール](docs/repository-structure.md) を参照してください。

## 新しいプロンプトの追加

単一プロンプトは [共通テンプレート](templates/prompt-template.md) を基準に作成します。

原則として、

- 単一プロンプトは1プロンプト1ファイル
- 複数ファイルが実行品質や保守性を実際に改善する場合だけProject Packageを使用
- 用途別カテゴリで整理
- 英小文字 `kebab-case` のパス
- 本文は日本語可
- バージョンと更新日を明記

とします。

## 方針

- 未確認の事実や数値をプロンプト側で要求しない
- AI固有機能への依存がある場合は明示する
- 長大なプロンプトでも、目的・前提・Fallback・停止条件を明確にする
- 既存プロンプトの意味を変更するときはバージョンを更新する
- Project Packageでも同じ指示を複数ファイルへ無用に重複させない

このリポジトリは今後、実際に公開するプロンプトが増えた時点でカテゴリや補助構造を追加していきます。

## ライセンス

このリポジトリで公開する内容は、個別に別条件を明記したものを除き、[CC0 1.0 Universal](LICENSE) の下で公開します。

コピー、改変、再配布、商用利用を含め、可能な限り自由に利用できます。クレジット表記も必須ではありません。

ただし、第三者が権利を有する商標、外部データ、引用物などの権利まで放棄するものではありません。
