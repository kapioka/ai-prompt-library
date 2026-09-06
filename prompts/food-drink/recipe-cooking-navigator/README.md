---
title: Recipe Cooking Navigator — ChatGPT Project Package
slug: recipe-cooking-navigator
category: food-drink
language: ja
status: active
version: 1.0.1
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

## ファイル

### 必須

- `project-instructions.md` — ChatGPT Projectへ設定する中核Instructions

### 推奨Project source

Recipe / Feedback SchemaとChatGPT連携仕様の正本は `kapioka/recipe-cooking-navigator` に置く。

Project sourceとして次の3ファイルを追加することを推奨する。

- `schemas/recipe-v1.schema.json`
- `schemas/feedback-v1.schema.json`
- `docs/chatgpt-integration.md`

2026-09-06時点では上記仕様は `feat/initial-spec` branchに存在する。アプリ側でmainへ統合された後はmainを優先する。

Project Instructionsは実行方針だけに圧縮し、厳密なfield定義はSchema、詳細な往復契約は`chatgpt-integration.md`へ分離する。

## セットアップ

1. ChatGPTで新規Projectを作成する。
2. Project名は例として `レシピ・Cooking Navigator` とする。
3. `project-instructions.md` の本文をProject Instructionsへ設定する。
4. 上記3ファイルをProject sourceへ追加する。
5. 料理候補の相談は通常会話で行う。
6. 「レシピお願い」「これで作る」「アプリに送る」等で最終Recipe JSONを生成する。
7. Android MVPでは共有またはファイル経由でアプリへ渡す。
8. 調理後はアプリからFeedback JSONをChatGPTへ共有し、新Revisionを受け取る。

## Source of Truth

- 再利用可能なPrompt: このProject Package
- Recipe / Feedback Schema: `kapioka/recipe-cooking-navigator`
- アプリ連携・製品仕様: `kapioka/recipe-cooking-navigator/docs/`

Schemaの意味をPrompt側で独自変更しない。

## 既知の制約

Feedback v1には改善対象snapshotのrevisionは含まれるが、アプリ側で現在存在する最大revision番号を明示するフィールドがない。

そのため過去Versionから枝分かれ改善する場合、ChatGPTが現在の最大revisionをProject contextから把握できなければ、暫定的に `parent_revision + 1` を使用する。将来、Revision衝突を完全に避けるにはFeedback契約またはアプリ側採番責務の明確化を検討する。

## 更新履歴

### 1.0.1 — 2026-09-06

- Project Instructionsを短い実行指示へ圧縮。
- Recipe / Feedback Schemaと`chatgpt-integration.md`をProject sourceとして分離する構成へ整理。

### 1.0.0 — 2026-09-06

- 初版。
- Recipe JSON / Feedback JSON連携をProject Instructionsとして整理。
- 通常相談と機械可読payload出力を分離。
- Recipe Schema v1準拠、Revision改善、安全性、Step単独実行性を定義。
