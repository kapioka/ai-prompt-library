---
title: "英会話スピーキングトレーナー — ChatGPT Project Package"
slug: "english-speaking-trainer"
category: "language-learning"
language: "ja"
status: "stable"
version: "1.0.0"
last_updated: "2026-09-15"
---

# 英会話スピーキングトレーナー — ChatGPT Project Package

## 概要

英語を「知っている」状態ではなく、**必要な場面で英語を即座に口から出せる状態**を目指す、Speaking中心のChatGPT Project向けトレーニングパッケージです。

完成英文を先に見せるのではなく、まず自力で英語を取り出させ、詰まった場合だけ段階的にヒントを出します。軽微なミスで会話を止めず、重要表現はその場で直して終わらせず、別の状況でも再利用させます。

## 想定用途

- 日常英会話の瞬発力を高めたい
- Duolingoなどで学んだ語彙・文法を実際の発話へつなげたい
- 英文を読めるが、会話になるとすぐに出てこない状態を改善したい
- 職場、旅行、買い物など実用場面のロールプレイをしたい
- 間違いを細かく止められすぎず、会話量を確保したい
- 過去に詰まった表現をRetrieval Practiceで定着させたい

## 対象環境

- ChatGPT Projects
- Project Instructions
- Project Files

## パッケージ構成

```text
english-speaking-trainer/
├─ README.md
├─ project-instructions.md
└─ english-speaking-project.md
```

### [`project-instructions.md`](project-instructions.md) — 必須

ChatGPT Projectの **Project Instructions** に全文を貼り付けます。

Projectの目的、主要原則、5つの操作コマンド、セッション開始条件を短く定義する入口です。

### [`english-speaking-project.md`](english-speaking-project.md) — 必須

ChatGPT Projectの **Project Files** に追加します。

Speaking First、難易度調整、訂正方法、再利用、復習、Duolingo連携、発音、操作コマンドの詳細仕様を定義する正本です。

`project-instructions.md` は入口としてこのファイルを参照し、詳細仕様を重複して持たない構成にしています。

## セットアップ

1. ChatGPTで新しいProjectを作成する
2. [`project-instructions.md`](project-instructions.md) の全文をProject Instructionsへ貼る
3. [`english-speaking-project.md`](english-speaking-project.md) をProject Filesへ追加する
4. Project内で `今日の練習`、`5分だけ` などのコマンドを入力する

## 操作コマンド

| コマンド | 内容 |
|---|---|
| `今日の練習` | 標準20〜25分のSpeakingセッション |
| `5分だけ` | 約5分のSpeaking Sprint |
| `仕事英語` | 製造現場・海外出身の同僚との実戦ロールプレイ |
| `復習` | 過去に詰まった重要表現を別場面で再利用 |
| `週テスト` | 過去表現を事前提示せず、自然な会話で定着度を確認 |

`英語練習しよう`、`英会話`、`始めよう` などでも標準セッションを開始します。

## 学習設計

このパッケージでは、次の流れを重視します。

```text
実際の会話
  ↓
自力で英語を取り出す
  ↓
必要な場合だけ段階的ヒント
  ↓
重要な詰まりを修正
  ↓
少し違う場面で再利用
  ↓
後日の復習・週テストで再登場
```

一度の練習で重点対象を増やしすぎず、原則として最大3表現に絞ります。

## カスタマイズ

[`english-speaking-project.md`](english-speaking-project.md) の「練習テーマ」は、現在以下を中心にしています。

- 製造現場
- 海外出身の同僚との会話
- 作業説明・安全確認
- 職場の短い雑談
- 買い物・飲食店
- 旅行・ホテル・駅・交通
- 日常生活

利用者の用途に合わせて、この部分を自分の仕事・趣味・生活場面へ置き換えて構いません。

ただし、Speaking First、段階的ヒント、会話を止めすぎないこと、重要表現の再利用、重点表現数の制限など、中核となる学習原則は残すことを推奨します。

## 設計上の特徴

- 完成英文を先回りして提示しない
- 軽微な文法ミスで会話を止めない
- ChatGPT側の通常発話を短く抑える
- 正解提示だけで終わらず、言い直しと再利用まで行う
- 過去のProject内の練習状況を利用できる場合は同じレベル判定を繰り返さない
- Duolingo等のインプットを現実の会話へ変換する
- ネイティブらしさより、相手へ誤解なく伝わる発音を優先する

## ライセンス

このパッケージは、リポジトリ全体の既定ライセンスである CC0 1.0 Universal の対象です。

## 更新履歴

- v1.0.0 — Speaking First、Retrieval Practice、5つの操作コマンドを含むChatGPT Project Packageとして初版公開
