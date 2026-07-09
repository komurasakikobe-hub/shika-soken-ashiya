# 芦屋歯科総研 プロジェクト指示

大阪歯科総研の横展開（神戸・京都に続く）。**立ち上げ手順の正本は `~/Desktop/クロード/横展開マニュアル.md`**。
デザイン・記事・スコアリングの共通ルールは大阪版 `CLAUDE.md`／`articles/ARTICLE_MANUAL.md` に準拠する。

## サイト概要
- 対象：芦屋市の歯科医院ポータル。エリアは駅・地区ベース3分類（区なし・小規模市）
- 設定の正本：`site_config.json`＋`assets/site-config.js`。都市固有の値は必ずここに集約
- 記事生成側の設定：`AI評判設計システム/client_config_ashiya.json`（daily_post.sh の CLIENT_CONFIG 環境変数で直接指定する。旧コピー差し替え運用は廃止・2026-07-10）
- ドメイン・GA4測定IDは未定（公開前にユーザーが決定）

## 費用ルール（無料優先の原則）
- 医院リスト収集：Google Maps API（月次無料枠内で運用）／AI分析：gpt-4o-mini／ジオコーディング：Nominatim（無料）
- 費用が発生しそうな操作は、まず無料の方法を調べ、無料で不可なら見積り提示→承認後に実行

## 立ち上げ進捗（2026-07-08 開始・完了：同日中に一巡目完了）
- [x] 京都版（修正済みコード）から複製・git init・ashiya_stations.py・グリッド・AREA_KEYWORDS・ブランド置換
- [x] データ収集（clinic_collector.py 完了：210院収集、Phase1+2）
- [x] 収集後の監査（品質フラグ：芦屋市外138院・サロン1院除外 → 掲載71院）→ slug生成（衝突0件）→ ジオコーディング（Places APIの座標をそのまま使用・Nominatim不要だった）→ 最寄駅計算（71院すべて付与）
- [x] サイト生成（build_clinics → build_features → build_index → build_sitemap）
- [x] index.html の統計数字を実数に差し替え済み（210院分析・71院掲載・5,282件口コミ・55院AI分析済み）
- [ ] サムネイル画像（ChatGPT無課金ルート）
- [ ] GitHub Privateリポジトリ・Cloudflare Pages・ドメイン・GA4・Search Console（ユーザー操作）
- [ ] 毎日投稿 launchd（時刻は他都市とずらして 11:30）

## 注意（大阪版で踏んだ地雷 — 横展開マニュアル §3 参照）
- slug衝突／ジオコーディング座標集約／統計数字の不一致／計測タグ入れ忘れ／医療広告ガイドライン

## この都市の立ち上げで新たに発見・修正したバグ（全都市に反映済み）
1. **Place Details APIの無効フィールド名**（`national_phone_number`）で全院の詳細取得（公式サイト・口コミ）が失敗していた → 除去して修正（5都市とも再収集）
2. **build_features.pyが必要とするequipment_stars/patient_fit/specialty_tagsが、clinic_collector.py v4.0のAI分析プロンプトから欠落**しており、特徴ページが必ず0件になるバグ → プロンプト・entry構築に追加。既存データは `../大阪歯科総研/backfill_schema_v2.py` で欠落分のみ追加分析して補完（gpt-4o-mini・安価）
3. **build_clinics.py/build_features.pyに「大阪市内 約2,050院」「shikasoken.com」がハードコード**されており、複製した全都市で誤った院数・ドメインが表示される状態だった → site_config.jsonのCITY_SHORT/N_PUBLISHED/domainを動的に埋め込むよう修正（大阪本体・5都市すべてに反映）
