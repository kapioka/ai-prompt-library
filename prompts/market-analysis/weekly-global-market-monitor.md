---
title: "週次グローバル市場モニター"
slug: "weekly-global-market-monitor"
category: "market-analysis"
language: "ja"
status: "stable"
version: "1.0.0"
last_updated: "2026-08-22"
---

# 週次グローバル市場モニター

## 概要

世界の主要株式4地域とクロスアセットを横断し、市場レジーム、急変リスク、ボラティリティ収縮、レジーム転換兆候、トレンド・サイクル状態、地域順位、前週からの変化を週次で評価するためのプロンプトです。

厳密な時系列計算が可能な環境では Exact Quant を使用し、取得環境や計算機能に制約がある場合は定義済みの Robust Fallback へ切り替えて、レポート全体を停止させない設計です。

## 想定用途

- Web検索や市場データ取得が可能なAIでの週次市場モニタリング
- Scheduled Task / 定期実行での定点観測
- 個人ポートフォリオから独立した市場環境評価
- Exact計算とFallback計算を明示的に分離した再現可能なレポート作成

## 前提・必要機能

- 最新の市場データを取得できることが望ましい
- Exact Quantには十分な時系列データと計算手段が必要
- Exact Quantが利用できない場合も、定義済みFallbackで処理を継続する
- 個別銘柄やユーザーの保有状況に基づく売買判断は対象外

## プロンプト本文

---

## Goal

毎週、世界の主要株式市場、ボラティリティ、金利、ドル、クレジット、金、原油を横断し、

* 現在の市場レジーム
* 急変リスク
* ボラティリティ収縮
* レジーム転換兆候
* 地域ごとのトレンド・サイクル状態
* 4地域の相対優先順位
* 前週からの重要な変化

を客観的かつ再現可能な形で日本語レポートとして報告する。

単なるニュース要約ではなく、市場データを中心に評価する。

高度な時系列計算が利用可能な場合は実行するが、取得環境の制約によって一部の高度計算ができない場合でも、レポート全体をN/Aにしない。

---

# 最重要原則

優先順位は次のとおり。

1. 確認できた市場データを正しく報告する
2. 毎週比較可能で実用的なレポートを完成させる
3. 取得可能なら厳密な定量計算を行う
4. 厳密計算不能なら、定義済みFallback方式へ切り替える
5. 確認できない精密値を創作しない

「厳密計算不能」と「市場判断不能」を同一視しない。

一部の高度計算ができないことを理由に、地域順位、レジーム判定、トレンド評価までN/Aにしない。

---

# ポートフォリオからの独立性

このレポートはユーザー個人の、

* 保有資産
* 保有数量
* 取得価格
* 損益
* 投資履歴
* 投資方針
* リスク許容度
* 過去の売買相談
* 過去の強気・弱気意見

を分析へ使用しない。

会話履歴やメモリからこれらを取得できても参照しない。

地域順位、Composite Score、レジーム、警報をユーザーの保有状況に合わせて変更しない。

個別の買い・売り・保有継続・買い増し判断は行わない。

---

# 対象

## 株式4地域

1. S&P 500
2. EURO STOXX 50
3. TOPIX
4. Emerging Markets

## クロスアセット

* VIX
* 米国10年国債利回り
* DXY
* HYG
* Gold
* WTI

---

# 表示系列と計算系列

最新水準を示す「表示系列」と、定量計算を行う「計算系列」を分離する。

## 表示

可能な限り次を使用する。

* S&P 500指数
* EURO STOXX 50指数
* TOPIX指数
* MSCI Emerging Markets
* CBOE VIX
* 米国10年国債利回り
* DXY
* HYG
* XAU/USD
* WTI

## 計算用固定系列

定量計算では原則として次を使用する。

* 米国：SPY
* 欧州：FEZ
* 日本：1306.T
* 新興国：EEM
* VIX：VIX指数
* クレジット：HYG
* 金：GLD
* 原油：USO
* ドル：DXY。取得不能ならUUP
* 米10年金利：信頼できる日次10年利回り系列

同一計算項目で週ごとに系列を変更しない。

表示指数と代理系列を混同しない。

重大な警報が代理系列で発生した場合は、可能な範囲で公式指数の方向も確認する。

---

# データ取得方針

ニュース記事は市場変動の背景説明に使用してよい。

ただし定量計算では、ニュース本文の数値から時系列を再構成しない。

市場履歴、ETF履歴、指数履歴、金融データページなど、連続した市場データを優先する。

最初の取得失敗だけでN/Aにしない。

次の順で試す。

1. 固定計算系列
2. 同じ系列の別の信頼できるデータソース
3. この指示で許可された代理系列
4. Fallback Mode
5. それでも意味のある判定ができない項目だけN/A

無制限に検索を続けない。

---

# 分析モード

毎週、最初に利用可能データを確認し、次のどちらかを選ぶ。

## Mode A：Exact Quant

必要な履歴と計算手段が利用可能な場合。

以下を厳密計算する。

* 63取引日高値
* 14日ATR
* 126日ATR平均
* 20日・60日相関
* Hilbert Cycle
* Exact Composite Score

## Mode B：Robust Fallback

Exact Quantに必要な履歴または計算手段の一部が不足する場合。

Exact値を推測しない。

代わりに、この指示で定義した、

* Trend Proxy
* Cycle Proxy
* Relative Strength
* Macro Alignment
* Fallback Composite Score

を使用する。

レポート冒頭に、

分析モード：Exact Quant

または

分析モード：Robust Fallback

と表示する。

一部だけExactの場合は、

分析モード：Mixed

とする。

---

# 条件1：VIX警戒

最新確定値について判定する。

VIX > 28

28ちょうどは不成立。

最新値、前週値、1か月方向を示す。

---

# 条件2：3か月高値から12％以上下落

Exact Modeでは計算系列の直近63取引日最高終値を使用する。

下落率：

最新終値 ÷ 63取引日最高終値 − 1

-12％以下で成立。

## Fallback

63日系列を取得できない場合、52週高値を確認する。

52週高値からの下落率が12％未満なら、

「不成立」

と確定してよい。

理由：

63日高値は52週高値以下であるため、63日高値からの下落率は52週高値からの下落率より大きくならない。

52週高値から12％以上下落している場合だけ、63日高値の追加取得を試す。

それでも確認できなければ、

「要確認」

とし、N/Aとはしない。

---

# 条件3：ボラティリティ収縮

この条件は段階判定する。

## Step 1：20取引日レンジ

最初に直近20取引日のHigh/Lowを確認する。

レンジ幅：

（20日最高値 − 20日最安値）
÷ 20日最安値 × 100

20日レンジが3％を超える場合：

ボラティリティ収縮＝不成立

この時点で終了する。

ATRは計算不要。

ATR欄にはN/Aではなく、

「不要（レンジ条件不成立）」

と表示する。

## Step 2：レンジ拡張

20日レンジが3％以下の場合のみ、21～60取引日まで順に期間を延ばし、3％以内を維持する最長期間を求める。

## Step 3：ATR

レンジ条件成立時だけ、

* Wilder方式14日ATR
* 直近126取引日の14日ATR平均

を計算する。

条件：

14日ATR < 126日ATR平均

両方成立した場合にCompression成立。

## ATRが計算不能な場合

レンジ条件は成立しているがATRを計算できない場合は、

Compression Candidate — ATR未確認

とする。

N/Aだけで終了しない。

---

# Compression位置判定

Compressionが成立した場合のみ使用する。

レンジ内位置：

（最新終値 − レンジ最安値）
÷（レンジ最高値 − レンジ最安値）×100

* 99.5％以上：🔴 STRONG BULLISH ALERT
* 99.0～99.5％未満：Bullish Watch
* 0.5％以下：🔵 STRONG BEARISH ALERT
* 0.5％超～1.0％以下：Bearish Watch
* その他：Neutral Compression

STRONGと通常Watchは重複表示しない。

---

# 条件4：レジーム転換

## Exact

可能なら20日・60日相関を計算する。

確認対象：

* 株式 × 米10年金利
* 株式 × DXY
* 株式 × HYG
* 株式 × VIX
* 株式 × Gold
* 株式 × Oil

## Fallback

同期時系列を取得できない場合は、相関係数を推測しない。

代わりに、

「Directional Regime Matrix」

を作る。

各項目について、

* 1週間方向
* 1か月方向

を、

↑ / → / ↓

で比較する。

次のような複数資産の整合した変化をレジーム転換候補とする。

* 株式↓ + VIX↑ + HYG↓
* 株式↓ + DXY↑
* 株式↓ + 金利↑
* 株式↓ + 金利急低下 + HYG↓
* DXY↑ + Emerging Markets↓
* 原油急上昇 + 金利↑ + 株式↓
* 株式↑だがHYG↓
* 地域間トレンドが大きく分岐

Fallbackによる判定は、

Qualitative Regime Change Watch

と表示する。

Exact相関による判定と混同しない。

---

# STRONG ALERT時の過去類似局面

🔴または🔵STRONG ALERTが発生した場合のみ実施する。

過去10年間から最大2事例。

比較：

* Compression
* VIX
* 米10年金利
* DXY
* HYG
* 対象地域のトレンド

可能なら、

* 1か月後上昇率
* 3か月後上昇率
* 1か月Maximum Drawdown
* 3か月Maximum Drawdown
* 3か月以内回復率
* 3か月以内の直前高値更新率

を計算し、Nを表示する。

十分な時系列を取得できない場合は、数値を作らず定性的比較だけにする。

N<=2であるため、統計的予測ではなく参考情報と明記する。

STRONG ALERTがなければこの処理を行わない。

---

# Hilbert Cycle

## Exact Hilbert

104週以上の週次終値と計算手段を利用できる場合のみ計算する。

方法：

1. 週次終値の自然対数
2. 20週EMAを差し引いてトレンド除去
3. 可能なら26週反射パディング
4. Hilbert変換
5. 位相角を0～360度に正規化

分類：

* 337.5～22.5：Peak
* 22.5～67.5：Early Down
* 67.5～112.5：Late Down
* 112.5～157.5：Trough Approach
* 157.5～202.5：Trough
* 202.5～247.5：Early Up
* 247.5～292.5：Late Up
* 292.5～337.5：Peak Approach

Strength Index：

現在振幅の直近52週パーセンタイル、0～100。

## Hilbertを計算できない場合

Hilbertの位相角を推測しない。

ただし、地域分析全体をN/Aにしない。

代わりにCycle Proxyを計算する。

---

# Cycle Proxy

取得可能な、

* 1か月騰落率
* 3か月騰落率

を使用する。

Momentum Acceleration：

A = 1か月騰落率 − 3か月騰落率 ÷ 3

単位はpercentage point。

分類：

### Proxy Early Up

* 1か月 > 0
* かつ3か月 <= 0

または

A > +1pp

### Proxy Late Up

* 1か月 > 0
* 3か月 > 0
* Aが-1～+1pp

### Proxy Early Down

* 1か月 < 0
* かつ3か月 >= 0

または

A < -1pp

### Proxy Late Down

* 1か月 < 0
* 3か月 < 0
* 明確な改善なし

### Proxy Turning

上記に明確に該当しない場合。

Cycle Proxy Strength：

4地域の|A|を順位化し、0～100のパーセンタイルで示す。

必ず、

「Cycle ProxyでありHilbert Cycleではない」

と表示する。

---

# Trend State

Exact Modeでは、

* 20週EMA
* 20週EMAの4週間変化
* 週次終値

を使用する。

### Uptrend

価格 > 20週EMA、EMA傾き >0

### Downtrend

価格 < 20週EMA、EMA傾き <0

### Improving

価格 < EMA、EMA傾き >0

### Weakening

価格 > EMA、EMA傾き <0

## Trend Proxy

Exact Trendが計算できない場合は、

* 1か月
* 3か月
* 6か月
* 52週高値・安値内位置

を使用する。

利用できない期間は残りの項目へウェイトを比例再配分する。

最低でも1か月・3か月騰落率があればTrend Proxyを作成する。

---

# Exact Composite Score

Exact Quantに必要なデータが揃う場合のみ使用する。

* Trend Strength：30％
* Cycle Phase：25％
* Direction-adjusted Cycle Strength：15％
* Relative Strength：15％
* Macro Alignment：15％

0～100。

---

# Fallback Composite Score

Exact Compositeが計算できない場合は必ずこちらを使用する。

Hilbert欠損だけを理由に地域順位をN/Aにしない。

ウェイト：

* Trend Proxy：35％
* Cycle Proxy：20％
* Relative Strength：20％
* Macro Alignment：25％

0～100。

## Trend Proxy Score

各地域について、

* 1か月
* 3か月
* 6か月

の騰落率を4地域間でそれぞれ0～100の順位スコアへ変換する。

さらに取得可能なら52週レンジ内位置も使用する。

基本ウェイト：

* 1か月：25％
* 3か月：35％
* 6か月：25％
* 52週位置：15％

欠損項目は残りへ比例再配分する。

## Cycle Proxy Score

* Proxy Early Up：100
* Proxy Late Up：75
* Proxy Turning：50
* Proxy Early Down：25
* Proxy Late Down：0

## Relative Strength Score

各地域の、

* 1週間
* 1か月
* 3か月

騰落率を4地域間で順位化し、平均する。

取得不能期間は残りへ比例再配分する。

## Macro Alignment

次の5項目を使用する。

* VIX
* HYG
* 米10年金利
* DXY
* Oil

各項目を、

* Favorable = 100
* Neutral / Mixed = 50
* Adverse = 0

のいずれかに分類する。

判定根拠を1行で示す。

地域ウェイト：

### S&P 500

VIX 30 / HYG 30 / 金利25 / DXY10 / Oil5

### EURO STOXX 50

VIX25 / HYG30 / 金利20 / DXY10 / Oil15

### TOPIX

VIX25 / HYG25 / 金利15 / DXY20 / Oil15

### Emerging Markets

VIX25 / HYG25 / 金利20 / DXY25 / Oil5

---

# 地域順位

原則として毎週必ず1～4位を出す。

表示：

* 順位
* 地域
* Score
* 計算モード：Exact / Fallback
* 前週比
* Trend
* Cycle
* Relative Strength
* Macro
* 主な理由

Exact Compositeが使えない場合はFallback Compositeで順位付けする。

「Hilbertがないため順位なし」としてはいけない。

---

# 前週比較

前回の同一Scheduled Taskの結果を参照できる場合は、

* 前週Score
* 前週レジーム
* 前週Cycle
* 前週Trend

を比較する。

同じ計算モード同士を優先して比較する。

計算モードが変わった場合、可能なら現在方式で前週値を再計算する。

再計算不能なら、

前週比：比較不能（計算モード変更）

とする。

現在値を使って架空の前週値を作らない。

---

# 市場レジーム

次のいずれか。

* Risk-on
* Neutral
* Risk-off
* Transition / Regime Change Watch

必ず確信度を、

高 / 中 / 低

で表示する。

強気材料と弱気材料の両方を確認する。

---

# 独自閾値の禁止

この指示に定義されていない、

* VIX水準
* 指数価格
* HYG価格
* 金利水準
* DXY水準
* 金・原油価格

について、新しい固定閾値を勝手に作成してはならない。

例：

「S&P 500が○○を割ればRisk-off」
「HYGが○○なら危険」
「WTIが○○なら売り」

などを根拠なく新設しない。

追加の数値条件を示す場合は、

* 過去高値・安値
* 直近レンジ
* 移動平均
* 定義済みパーセンタイル
* 明示した計算結果

から導かれた値に限定し、その根拠を記載する。

---

# 出力順序

1. YYYY-MM-DD
2. 分析モード：Exact / Mixed / Robust Fallback
3. STRONG ALERT
4. 今週のレジームと3～5個の重要変化
5. 市場スナップショット
6. 警戒条件1～4
7. STRONG時のみHistorical Analogue
8. Cycle表
9. 地域ランキング1～4
10. リスクと反証材料
11. データ品質・計算モード・主要情報源

---

# Cycle表

列：

* 地域
* Hilbert Phase
* Hilbert Strength
* Cycle Proxy Phase
* Cycle Proxy Strength
* Trend
* 前週変化
* Mode

Exact Hilbertがない場合は、

Hilbert：算出不能

としてよい。

ただし同じ行にCycle Proxyを必ず表示する。

Hilbert欄がN/Aでも表全体をN/Aにしない。

---

# 市場スナップショット

対象：

* S&P 500
* EURO STOXX 50
* TOPIX
* Emerging Markets
* VIX
* 米10年金利
* DXY
* HYG
* Gold
* WTI

可能な範囲で表示：

* 最新値
* 基準日
* 1週間変化
* 1か月変化
* 3か月変化

3か月高値下落率は株式4地域だけでよい。

すべての期間を埋めるために数値を推測しない。

取得不能セルを大量のN/Aで埋めるより、利用可能期間を明示する。

---

# リスクと反証材料

現在のレジーム判断を崩す条件を2～4点示す。

この指示にない固定価格閾値を新しく作らない。

可能なら、

* VIXの定義済み閾値28
* Compression
* 直近レンジブレイク
* HYG方向
* DXY方向
* 金利方向
* 地域トレンド転換

で説明する。

---

# N/Aルール

N/Aは最後の手段。

次の区別をする。

### 不要

上流条件が不成立で計算する必要がない。

例：

ATR：不要（20日レンジ >3％）

### 未確認

追加データが不足しているが、候補状態を確認できる。

例：

Compression Candidate — ATR未確認

### Proxy

厳密計算の代わりに定義済みFallbackを利用した。

### N/A

ExactもFallbackも意味のある判定ができない。

この4種類を混同しない。

---

# Output Quality

* 日本語
* 数値・事実・解釈を分離
* 重要な数値には出典
* ニュースだけで判断しない
* 同じ説明を繰り返さない
* 未確認の精密値を創作しない
* 未定義の数値閾値を創作しない
* ユーザーのポートフォリオを参照しない
* 強気・弱気の両方の証拠を扱う
* ExactができなくてもFallbackでレポートを完成させる
* 高度指標1つの欠損でレポート全体を停止しない

---

# Stop Rules

次を満たしたら終了する。

* 必須10市場を確認
* VIX条件を評価
* 3か月ドローダウンを評価
* Compressionを評価
* Regime Changeを評価
* ExactまたはProxy Cycleを4地域で評価
* ExactまたはFallback Compositeを4地域で算出
* 1～4位ランキングを出力
* 市場レジームを判定
* 反証材料を提示
* データ品質と使用モードを明記

追加調査をしても主要判断が変わらない場合は、検索を終了してレポートを完成させる。

---

## 更新履歴

- v1.0.0 — 2026-08-22 初版公開
