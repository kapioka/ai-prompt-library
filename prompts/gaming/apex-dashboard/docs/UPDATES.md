# 公式アップデート情報の取得（Xを使わない方法） UPDATES

Apex Legends の公式アップデート（パッチノート、細かな修正、イベント情報）は、主に X（旧Twitter）の公式アカウントで告知される。
しかし X はログインなしでは安定して取得できないため、本プロジェクトでは **X に依存しない代替情報先** から公式情報を取得する。

## 主な代替情報先：Steam News API（キー不要）

Respawn 公式スタッフの投稿は Steam のコミュニティ告知にもそのまま配信されている。公式の Web API で JSON 取得できる。

```
GET https://api.steampowered.com/ISteamNews/GetNewsForApp/v2/?appid=1172470&count=8&maxlength=300&format=json
```

- `appid=1172470` は Apex Legends の Steam アプリID
- レスポンス：`appnews.newsitems[]`
  - `title`：見出し（例：「Apex Legends: Latest Update 11/16/2026」）
  - `url`：記事URL
  - `contents`：本文冒頭（`maxlength` で文字数を制限できる）
  - `date`：投稿日時（Unix秒・UTC）
  - `feedlabel` / `feedname`：配信元（Community Announcements）
  - `tags`：`patchnotes` を含む投稿はパッチノート系として区別できる
  - `author`：投稿者（Respawn スタッフは `rspn_` で始まることが多い）
- 取得できる内容の例：最新アップデートの修正内容、アンチチート関連の報告、イベント告知、AMA告知など、Xで告知される細かな修正情報とほぼ同等の公式情報

## 日本語の公式情報：EA Japan ニュースページ

Steam News の投稿は日本語指定をしても英語のまま（実測）。日本語の公式記事は EA Japan のサイトに出る。

- **EA Japan ニュースページ**（ https://www.ea.com/ja/games/apex-legends/apex-legends/news ）
  - シーズンのパッチノート、デザイナーノート、イベント記事が日本語で掲載される
  - ページHTML内の埋め込みJSON（`__NEXT_DATA__` のニュースデータ）に記事タイトルとスラッグがあり、`https://www.ea.com/ja/games/apex-legends/apex-legends/news/<slug>` が記事URLになる
  - ただし「Latest Update」のような細かな修正告知は掲載されない。日本語で全部は揃わない
- 表示の推奨構成は2本立て：上段に EA Japan の日本語記事（最大3件）、下段に Steam の英語記事（最大3件）
- ページ構成の変更に弱いので、取得失敗時は前回分を保持し、Steam側だけで表示を落とさないこと

## 補助の情報先

- **EA公式サイト（英語）のニュース／パッチノートページ**（ https://www.ea.com/games/apex-legends/news ）
  - シーズン開始時や中間パッチの正式なパッチノート全文はこちらが正本
- **apexlegendsstatus.com**：サーバー状態や既知の障害情報の確認用

## 実装上の注意

- ニュース取得はローテーション取得と同じ定期ジョブ（15分間隔）で一緒に行い、結果を保存して表示は保存済みデータを即返す（表示を外部取得で待たせない）
- 表示は最新5件程度に絞り、タイトル＋日付＋短い冒頭＋リンクだけにする（全文転載はしない。著作権と可読性の両面から）
- `patchnotes` タグ付きは「アプデ」と目立たせると、細かな修正の見落としが減る
- 取得に失敗した場合は前回分を表示し続ける

## なぜ X を使わないか

- 未ログインではタイムラインの安定取得ができない（ログイン必須化が進んでいる）
- 誰でも再実装・公開できるプロジェクトにするには、キーもログインも要らない公開APIの方が適している
- Steam News は EA/Respawn の公式投稿そのものであり、情報の正確さは X の告知と同等
