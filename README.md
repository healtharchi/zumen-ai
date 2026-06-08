# スマート見積＋図面AI（実験版） / zumen-ai

HEALTH ARCHI の実験ツール。**既存「スマート見積システム」をまるごと土台にし、「規模」タブに「図面を放り込むと外周長・床面積などの入力欄へAIが自動入力する」機能を実験的に追加**したもの。図面AI積算が実用に耐えるかを精度検証するための試作。

本番の「スマート見積システム」とは別リポジトリ・別プロジェクト（実験的位置づけ）。要件定義は HEALTH ARCHI フォルダ `3_プロジェクト/進行中/P030_図面AI積算統合/` に集約。

## 公開URL
https://healtharchi.github.io/zumen-ai/ （GitHub Pages・公開後）

## 仕組み（精度検証フェーズ＝極力シンプル構成）
- **単一HTML（既存スマート見積=React/Babel/Tailwind CDN をコピー）+ localStorage**。サーバー・DB・ログインなし
- 「規模」タブ先頭に `DrawingAIPanel` を追加。Claude API を**ブラウザから直接**呼ぶ（`anthropic-dangerous-direct-browser-access`）
- APIキーは利用者がブラウザに保存（localStorage `zumen_ai_apikey`）。**コードには一切埋め込まない**＝Public公開可
- 図面 → Claude vision + Structured Outputs(JSON Schema) で `{variable, value, unit, source(measured/estimated), confidence, basis}` を取得（要件定義 §4.3 準拠）
- 信頼度60%以上の実測値だけを、既存の `setScale()` で `manualPerimeter / manualWallArea / manualRoofArea / buildingAreaInput / floor1Area / floor2Area` へ自動投入 → 既存の計算エンジンが見積を自動再計算（§3 の関数は無改修）
- 読めない項目は空欄のまま＝従来の係数推定にフォールバック（§4.4 準拠）

> 本番版（HEALTH ARCHIのキーをサーバーに隠し、お客様はログイン＋サブスクで利用／認証・Supabase・非同期ジョブ）は、精度が実用に耐えると確認後に別途構築する方針。

## 使い方
[使い方ガイド](./guide.html) を参照。

## 開発・運営
HEALTH ARCHI（https://healtharchi.com）
