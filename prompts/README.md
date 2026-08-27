# プロンプト一覧

このディレクトリには、用途別に整理した公開プロンプトを格納します。

## カテゴリ

### 個別株・企業分析

- [日本株 企業業績・ファンダメンタル分析 — ChatGPT Project Package](equity-analysis/japanese-company-fundamental-analyzer/README.md)  
  日本の上場企業を、過去・現在・将来の順に整理し、KPI・セグメント、シナリオ・バリュエーション、最新開示・経営方針を番号選択で深掘りするChatGPT Project向けパッケージ。

### 市場分析

- [週次グローバル市場モニター](market-analysis/weekly-global-market-monitor.md)  
  世界の主要株式市場とクロスアセットを横断し、市場レジーム、急変リスク、ボラティリティ収縮、レジーム転換兆候、地域順位を週次で評価するプロンプト。

### 食・飲料

- [ボトル・ラベルから楽しむ酒類ガイド](food-drink/bottle-label-drink-guide.md)  
  ワイン、日本酒、ウイスキー、焼酎のボトルやラベル画像から商品を識別し、酒類別の星評価、初心者向け解説、料理との相性、飲み方、条件を満たす場合は雑誌風の商品紹介画像まで生成するプロンプト。

## 追加ルール

単一プロンプトは原則として次の形式で配置します。

```text
prompts/<category>/<prompt-name>.md
```

複数ファイルが実行品質や保守性を実際に改善するChatGPT Project向けプロンプトは、Project Packageとして次の形式を使用できます。

```text
prompts/<category>/<package-name>/
├─ README.md
├─ project-instructions.md
└─ <support-files>.md
```

Project Packageでは `README.md` を公開入口にし、必須・推奨・任意ファイルを明示します。

ファイル名とカテゴリ名は英小文字の `kebab-case` を使用します。

詳細は [リポジトリ構成ルール](../docs/repository-structure.md) を参照してください。
