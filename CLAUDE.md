# 99agent-ads-web — 99agent ADS 行銷自動化代操銷售頁

## 正本與來源
- 來源：2026-10-06 從 `stock-page-standalone.zip`（線上 https://stock.99agent.app/ 存下的單檔 HTML）解出，改名為 `index.html`。
- 這份 repo 是**修改用的副本**；線上 stock.99agent.app 不會因為改這裡就變。
- GitHub：`cindyhsu-png/99agent-ads-web`（private）。

## 部署
- 目前**沒有部署**。要上線需另外決定（GitHub Pages／Kolable iframe／換掉 stock.99agent.app 原站）。

## 注意事項
- 單檔約 520KB，原站是 Next.js 匯出：CSS、JS 都內嵌在 `index.html`，改文案直接搜中文字串改。
- 圖片在 `storage.googleapis.com/99agent-public/`、影片在 Mux，都是外部連結，不在 repo 內。
- 頁內有原站的 GTM（`GTM-MZGCRQ2M`）；若另外部署，要決定保留或拔掉，以免灌髒原站數據。
- 頁尾連到原站的 /privacy、/terms、/system-contract。
- 尚未在墨機登記（若要上線再走 §7 第 9 步）。
