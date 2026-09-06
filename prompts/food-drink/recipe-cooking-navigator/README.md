---
title: Recipe Cooking Navigator — ChatGPT Project Package
slug: recipe-cooking-navigator
category: food-drink
language: ja
status: active
version: 1.0.2
last_updated: 2026-09-06
---

# Recipe Cooking Navigator — ChatGPT Project Package

## 概要

料理相談から完成Recipeの生成、Recipe Cooking NavigatorアプリへのRecipe JSON受け渡し、調理後Feedback JSONからのRevision生成までを1つのChatGPT Projectで扱うためのProject Package。

ChatGPTはレシピ内容の生成・改善を担当し、アプリは表示、調理進行、記録、Version管理、共有を担当する。

## 想定用途

- 材料や条件から料理候補を相談する
- 実際に調理するレシピを確定する
- Recipe Cooking Navigatorへ取り込めるRecipe JSONを生成する
- アプリから共有されたFeedback JSONを使ってレシピを改善する
- 過去Versionを改善元として新Revisionを作る

## ChatGPT Projectに必要なファイル

このPackageをChatGPT Projectとして使う場合、次の4項目をセットで使用する。

### 1. Project Instructions

- [`project-instructions.md`](project-instructions.md) — 本文をChatGPT ProjectのInstructionsへ設定する。

### 2. Project sourceとして追加する3ファイル

次の3ファイルをChatGPT Projectのsourceへ追加する。

- [`recipe-v1.schema.json`](recipe-v1.schema.json) — Recipe JSON Schema v1
- [`feedback-v1.schema.json`](feedback-v1.schema.json) — Feedback JSON Schema v1
- [`chatgpt-integration.md`](chatgpt-integration.md) — ChatGPT ↔ Recipe Cooking Navigator連携仕様

**このREADMEが表示されているGitHubページのファイル一覧から、上記3ファイルを直接開いて内容を確認できる。**

Project Instructionsは実行方針だけに圧縮しているため、意図したSchema準拠とアプリ連携を行うには、この3ファイルもProject sourceとして追加することを前提とする。

## 配布用コピーとSource of Truth

このPackage内の3ファイルは、ChatGPT Projectへそのまま追加しやすくするための**配布用コピー**であり、仕様の正本ではない。

正本は `kapioka/recipe-cooking-navigator` に置く。

| このPackage内の配布用コピー | 正本 |
|---|---|
| `recipe-v1.schema.json` | `kapioka/recipe-cooking-navigator/schemas/recipe-v1.schema.json` |
| `feedback-v1.schema.json` | `kapioka/recipe-cooking-navigator/schemas/feedback-v1.schema.json` |
| `chatgpt-integration.md` | `kapioka/recipe-cooking-navigator/docs/chatgpt-integration.md` |

2026-09-06時点では正本候補の仕様は `recipe-cooking-navigator` の `feat/initial-spec` branchに存在する。アプリ側でmainへ統合された後はmainを正本として優先する。

Schemaや連携仕様を変更するときは、まず `recipe-cooking-navigator` 側を更新し、このPackageの配布用コピーを追従させる。このPackage側から独自にSchemaの意味を変更しない。

## セットアップ

1. ChatGPTで新規Projectを作成する。
2. Project名は例として `レシピ・Cooking Navigator` とする。
3. [`project-instructions.md`](project-instructions.md) の本文をProject Instructionsへ設定する。
4. [`recipe-v1.schema.json`](recipe-v1.schema.json) をProject sourceへ追加する。
5. [`feedback-v1.schema.json`](feedback-v1.schema.json) をProject sourceへ追加する。
6. [`chatgpt-integration.md`](chatgpt-integration.md) をProject sourceへ追加する。
7. 料理候補の相談は通常会話で行う。
8. 「レシピお願い」「これで作る」「アプリに送る」等で最終Recipe JSONを生成する。
9. Android MVPでは共有またはファイル経由でアプリへ渡す。
10. 調理後はアプリからFeedback JSONをChatGPTへ共有し、新Revisionを受け取る。

## Source of Truth

- 再利用可能なPrompt / Project Instructions: このProject Package
- Recipe / Feedback Schema: `kapioka/recipe-cooking-navigator`
- アプリ連携・製品仕様: `kapioka/recipe-cooking-navigator/docs/`

## 既知の制約

Feedback v1には改善対象snapshotのrevisionは含まれるが、アプリ側で現在存在する最大revision番号を明示するフィールドがない。

そのため過去Versionから枝分かれ改善する場合、ChatGPTが現在の最大revisionをProject contextから把握できなければ、暫定的に `parent_revision + 1` を使用する。将来、Revision衝突を完全に避けるにはFeedback契約またはアプリ側採番責務の明確化を検討する。

## 更新履歴

### 1.0.2 — 2026-09-06

- `recipe-v1.schema.json`、`feedback-v1.schema.json`、`chatgpt-integration.md` の配布用コピーをProject Package直下へ追加。
- READMEから3ファイルを直接確認できるようにリンクを追加。
- ChatGPT Project作成時にProject Instructionsと3つのProject sourceが必要であることを明示。
- 配布用コピーと`recipe-cooking-navigator`側のSource of Truthを区別。

### 1.0.1 — 2026-09-06

- Project Instructionsを短い実行指示へ圧縮。
- Recipe / Feedback Schemaと`chatgpt-integration.md`をProject sourceとして分離する構成へ整理。

### 1.0.0 — 2026-09-06

- 初版。
- Recipe JSON / Feedback JSON連携をProject Instructionsとして整理。
- 通常相談と機械可読payload出力を分離。
- Recipe Schema v1準拠、Revision改善、安全性、Step単独実行性を定義。
