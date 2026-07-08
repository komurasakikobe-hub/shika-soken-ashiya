# 芦屋歯科総研 プロジェクト指示

大阪歯科総研の横展開（神戸・京都に続く）。**立ち上げ手順の正本は `~/Desktop/クロード/横展開マニュアル.md`**。
デザイン・記事・スコアリングの共通ルールは大阪版 `CLAUDE.md`／`articles/ARTICLE_MANUAL.md` に準拠する。

## サイト概要
- 対象：芦屋市の歯科医院ポータル。エリアは駅・地区ベース3分類（区なし・小規模市）
- 設定の正本：`site_config.json`＋`assets/site-config.js`。都市固有の値は必ずここに集約
- 記事生成側の設定：`AI評判設計システム/client_config_ashiya.json`（使用時に client_config.json へコピー）
- ドメイン・GA4測定IDは未定（公開前にユーザーが決定）

## 費用ルール（無料優先の原則）
- 医院リスト収集：Google Maps API（月次無料枠内で運用）／AI分析：gpt-4o-mini／ジオコーディング：Nominatim（無料）
- 費用が発生しそうな操作は、まず無料の方法を調べ、無料で不可なら見積り提示→承認後に実行

## 立ち上げ進捗（2026-07-08 開始）
- [x] 京都版（修正済みコード）から複製・git init・ashiya_stations.py・グリッド・AREA_KEYWORDS・ブランド置換
- [ ] データ収集（clinic_collector.py 実行中/完了確認）
- [ ] 収集後の監査（7点チェック）→ slug生成 → ジオコーディング → 最寄駅計算
- [ ] サイト生成（build_clinics → build_features → build_index → build_sitemap）
- [ ] index.html の統計数字を実数に差し替え（site_config.json の stats と一致させる）
- [ ] サムネイル画像（ChatGPT無課金ルート）
- [ ] GitHub Privateリポジトリ・Cloudflare Pages・ドメイン・GA4・Search Console（ユーザー操作）
- [ ] 毎日投稿 launchd（時刻は他都市とずらして 11:30）

## 注意（大阪版で踏んだ地雷 — 横展開マニュアル §3 参照）
- slug衝突／ジオコーディング座標集約／統計数字の不一致／計測タグ入れ忘れ／医療広告ガイドライン
