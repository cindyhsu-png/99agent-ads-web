# 99agent-ads-web — 99agent ADS 行銷自動化代操銷售頁

## 正本與來源
- 來源：2026-10-06 從 `stock-page-standalone.zip`（線上 https://stock.99agent.app/ 存下的單檔 HTML）解出，改名為 `index.html`。
- 這份 repo 是**修改用的副本**；線上 stock.99agent.app 不會因為改這裡就變。
- GitHub：`cindyhsu-png/99agent-ads-web`（**public**，2026-10-06 為開 Pages 從 private 改公開；免費方案 private 不能開 Pages）。

## 部署
- GitHub Pages：https://cindyhsu-png.github.io/99agent-ads-web/ （推 main 自動更新，約 1 分鐘）。
- 原站 stock.99agent.app 不受影響。

## 注意事項
- 單檔約 520KB，原站是 Next.js 匯出：CSS、JS 都內嵌在 `index.html`，改文案直接搜中文字串改。
- 圖片在 `storage.googleapis.com/99agent-public/`、影片在 Mux，都是外部連結，不在 repo 內。
- 頁內有原站的 GTM（`GTM-MZGCRQ2M`）；若另外部署，要決定保留或拔掉，以免灌髒原站數據。
- 頁尾連到原站的 /privacy、/terms、/system-contract。
- 尚未在墨機登記（若要上線再走 §7 第 9 步）。

## 嵌入 Kolable

這頁會被 iframe 嵌進 `dnschool.kolable.app/aimarketing`（取代原本的 `gotlead-order-web`）。
`index.html` 結尾有一段**嵌入支援**，單獨開啟時完全不作用（偵測 `window.self !== window.top`）。

它做三件事：

1. **回報內容高度**給父頁（`adsweb:height`），父頁把 iframe 撐到等高 → 只剩外層一條卷軸
2. **解開跟著視窗走的高度**：原頁 `<html class="h-full">` + `<body class="min-h-full">` 會讓
   body 高度等於視窗高度；iframe 被撐高後量到的內容高就只漲不縮（棘輪），必須在嵌入時解開。
   `min-h-screen`、`min-h-[calc(100vh-56px)]` 同理，改釘死成父頁回報的真實視窗高。
3. **修正固定定位**：`position:fixed` 在被撐高的 iframe 裡會黏在整份內容頂端而不是使用者眼前。
   導覽列改 `static`；`.standalone-modal`／`.standalone-lightbox`（含 **NT$100 預約彈窗**）
   改絕對定位，開在父頁回報的捲動位置；跟著滑鼠的預覽卡直接關掉。

### 父頁（Kolable 嵌入區塊）要放的程式碼

```html
<section style="width:100%">
  <iframe id="ads-frame"
    src="https://cindyhsu-png.github.io/99agent-ads-web/"
    title="99agent ADS"
    referrerpolicy="strict-origin-when-cross-origin"
    allow="fullscreen; clipboard-write"
    style="width:100%;height:720px;border:0;display:block"></iframe>
</section>
<script>
(function(){
  var ORIGIN='https://cindyhsu-png.github.io';
  function frame(){ return document.getElementById('ads-frame') }
  window.addEventListener('message',function(e){
    if(e.origin!==ORIGIN) return;
    var d=e.data||{};
    if(d.type==='adsweb:height'&&d.height){
      var f=frame(); if(f) f.style.height=d.height+'px';
      tellViewport();
    }
  });
  function tellViewport(){
    var f=frame(); if(!f||!f.contentWindow) return;
    var r=f.getBoundingClientRect();
    f.contentWindow.postMessage({type:'adsweb:viewport',
      top:Math.max(0,-r.top), height:window.innerHeight}, ORIGIN);
  }
  window.addEventListener('scroll',tellViewport,{passive:true});
  window.addEventListener('resize',tellViewport);
  setTimeout(tellViewport,300); setTimeout(tellViewport,1200);
})();
</script>
```

> 🔴 外層**不要**包 `height:100dvh` + `overflow:hidden` 的容器，那會造成雙卷軸。
> `loading="lazy"` 也不要加，會延後高度回報。
> Kolable 的「嵌入」元件用 `createContextualFragment` 渲染，會執行 `<script>`。

⚠️ **捲動位置要回報**，否則 NT$100 預約彈窗會開在內容頂端（客人點了像沒反應）。
