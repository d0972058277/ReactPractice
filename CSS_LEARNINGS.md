# CSS 完整學習路線

> 這份指南會教你 CSS 的核心概念，並搭配履歷網站專案實作。每個章節都有「為什麼需要」、「怎麼運作」、「實際應用」三個部分。

---

## 🎯 學習目標

- 理解 CSS 的運作原理（不只是複製貼上）
- 學會使用瀏覽器開發者工具除錯
- 能夠自己設計並實作網頁佈局
- 掌握響應式設計的核心概念

---

## 📋 第一階段：CSS 基礎概念

### 1.1 CSS 是什麼？如何載入？

**為什麼需要 CSS？**
- HTML 只負責「內容結構」（這是標題、這是段落）
- CSS 負責「視覺呈現」（顏色、大小、位置）
- 分離內容與樣式，讓程式碼更好維護

**三種載入方式：**

```html
<!-- 方式一：外部 CSS 檔案（最推薦）-->
<link rel="stylesheet" href="App.css">

<!-- 方式二：內部 <style> 標籤 -->
<style>
  h1 { color: blue; }
</style>

<!-- 方式三：行內樣式（不推薦，難以維護）-->
<h1 style="color: blue;">標題</h1>
```

**在 React 中如何使用？**

```tsx
// src/components/Header.tsx
import './Header.css'  // ← 引入 CSS 檔案

export function Header() {
  return <h1 className="header-title">標題</h1>
}
```

```css
/* src/components/Header.css */
.header-title {
  color: blue;
}
```

**💡 重點：**
- React 使用 `className` 而非 `class`（因為 `class` 是 JavaScript 保留字）
- 每個元件的 CSS 檔案名稱通常跟元件同名（如 `Header.tsx` → `Header.css`）

---

### 1.2 選擇器（Selector）- 如何指定要改變樣式的元素？

CSS 的基本語法：

```css
選擇器 {
  屬性: 值;
}
```

**常用選擇器：**

```css
/* 1. 元素選擇器 - 選取所有 <h1> */
h1 {
  color: red;
}

/* 2. Class 選擇器 - 選取 class="title" 的元素 */
.title {
  color: blue;
}

/* 3. ID 選擇器 - 選取 id="main-title" 的元素 */
#main-title {
  color: green;
}

/* 4. 組合選擇器 - 選取 <header> 裡面的 <h1> */
header h1 {
  color: purple;
}

/* 5. 直接子元素 - 選取 <header> 的「直接」子元素 <h1> */
header > h1 {
  color: orange;
}

/* 6. 多個 class - 同時有 .card 和 .featured */
.card.featured {
  border: 2px solid gold;
}

/* 7. 群組選擇器 - 同時套用到 h1, h2, h3 */
h1, h2, h3 {
  font-family: Arial;
}
```

**實際範例：**

```tsx
// React 元件
<div className="container">
  <header className="header">
    <h1 className="header-title">我的履歷</h1>
    <p className="header-subtitle">前端工程師</p>
  </header>
</div>
```

```css
/* CSS 樣式 */

/* 選取 .header 內的所有文字 */
.header {
  text-align: center;
}

/* 只選取 .header 內的 h1 */
.header h1 {
  font-size: 2rem;
  color: #333;
}

/* 只選取有 header-subtitle 這個 class 的元素 */
.header-subtitle {
  color: #666;
}
```

**優先權（Specificity）：**

當多個規則套用到同一個元素時，誰會贏？

```css
/* 優先權由高到低： */
#main-title { color: red; }      /* 1. ID 選擇器 (權重 100) */
.title { color: blue; }           /* 2. Class 選擇器 (權重 10) */
h1 { color: green; }              /* 3. 元素選擇器 (權重 1) */

/* 特殊：!important 會強制覆蓋（不推薦，除非必要）*/
h1 { color: yellow !important; }
```

**🎯 練習任務：**
1. 打開 Chrome DevTools（F12）
2. 點選「Elements」頁籤
3. 找到任一個元素，右側會顯示套用的所有 CSS 規則
4. 試著取消勾選某個屬性，看看畫面變化

---

### 1.3 Box Model - CSS 的核心概念

**所有 HTML 元素都是一個「盒子」，由內到外有四層：**

```
┌──────────────────────────────────┐
│         Margin (外邊距)           │  ← 元素與外部的距離
│  ┌────────────────────────────┐  │
│  │   Border (邊框)             │  │
│  │  ┌──────────────────────┐  │  │
│  │  │  Padding (內邊距)     │  │  │  ← 內容與邊框的距離
│  │  │  ┌────────────────┐  │  │  │
│  │  │  │   Content      │  │  │  │  ← 實際內容（文字、圖片）
│  │  │  │  (width/height)│  │  │  │
│  │  │  └────────────────┘  │  │  │
│  │  └──────────────────────┘  │  │
│  └────────────────────────────┘  │
└──────────────────────────────────┘
```

**實際應用：**

```css
.card {
  /* 內容區域大小 */
  width: 300px;
  height: 200px;

  /* 內邊距（內容與邊框的距離）*/
  padding: 20px;           /* 四邊都是 20px */
  padding: 10px 20px;      /* 上下 10px，左右 20px */
  padding: 10px 20px 30px 40px;  /* 上 右 下 左（順時針）*/

  /* 邊框 */
  border: 2px solid #333;  /* 寬度 樣式 顏色 */
  border-radius: 8px;      /* 圓角 */

  /* 外邊距（與其他元素的距離）*/
  margin: 20px;            /* 四邊都是 20px */
  margin-top: 10px;        /* 只設定上邊距 */
  margin: 0 auto;          /* 上下 0，左右自動（水平置中）*/
}
```

**box-sizing 屬性（重要！）：**

```css
/* 預設值：content-box */
.box1 {
  width: 300px;
  padding: 20px;
  border: 2px solid black;
  /* 實際寬度 = 300 + 20*2 + 2*2 = 344px（不直覺）*/
}

/* 推薦：border-box */
.box2 {
  box-sizing: border-box;
  width: 300px;
  padding: 20px;
  border: 2px solid black;
  /* 實際寬度 = 300px（padding 和 border 會往內縮）*/
}
```

**全域設定（推薦在所有專案使用）：**

```css
/* src/index.css */
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

**🎯 練習任務：**

建立一個卡片元件，觀察 Box Model：

```tsx
// src/components/Card.tsx
export function Card() {
  return (
    <div className="card">
      <h3>我的卡片</h3>
      <p>這是內容區域</p>
    </div>
  )
}
```

```css
/* src/components/Card.css */
.card {
  width: 300px;
  padding: 20px;
  margin: 20px;
  border: 2px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}
```

在 Chrome DevTools 中：
1. 選取 `.card` 元素
2. 右側會顯示 Box Model 圖示
3. 滑鼠移到不同區域，頁面會高亮顯示對應的 margin、border、padding

---

### 1.4 顏色與單位

**顏色的表示方式：**

```css
.element {
  /* 1. 顏色名稱（不推薦，選擇有限）*/
  color: red;
  background: lightblue;

  /* 2. Hex 十六進位（最常用）*/
  color: #ff0000;        /* 紅色 */
  color: #f00;           /* 簡寫（每兩位相同可縮寫）*/
  color: #ff0000ff;      /* 帶透明度（最後兩位是 alpha）*/

  /* 3. RGB */
  color: rgb(255, 0, 0);           /* 紅色 */
  color: rgba(255, 0, 0, 0.5);     /* 50% 透明的紅色 */

  /* 4. HSL（色相、飽和度、亮度）- 更直覺 */
  color: hsl(0, 100%, 50%);        /* 紅色 */
  color: hsla(0, 100%, 50%, 0.5);  /* 50% 透明的紅色 */
}
```

**💡 推薦工具：**
- [Coolors](https://coolors.co) - 配色產生器
- Chrome DevTools 的顏色選擇器（點擊顏色方塊）

**單位的種類：**

```css
.element {
  /* === 絕對單位 === */
  width: 200px;        /* 像素（最常用）*/
  font-size: 16pt;     /* 點（印刷用）*/

  /* === 相對單位（響應式設計必學）=== */

  /* 相對於父元素 */
  width: 50%;          /* 父元素寬度的 50% */

  /* 相對於根元素（<html>）的字體大小 */
  font-size: 1rem;     /* 如果 html { font-size: 16px }，則 1rem = 16px */
  padding: 2rem;       /* 32px */

  /* 相對於父元素的字體大小 */
  font-size: 1.5em;    /* 父元素字體的 1.5 倍 */

  /* 相對於視窗（Viewport）大小 */
  width: 100vw;        /* 視窗寬度的 100% */
  height: 100vh;       /* 視窗高度的 100% */
  font-size: 5vw;      /* 視窗寬度的 5% */
}
```

**實際應用：**

```css
/* 設定根元素字體大小 */
html {
  font-size: 16px;  /* 1rem = 16px */
}

/* 使用 rem 讓整體可縮放 */
.header {
  padding: 2rem;      /* 32px */
  font-size: 1.5rem;  /* 24px */
}

.button {
  padding: 0.75rem 1.5rem;  /* 12px 24px */
  font-size: 1rem;          /* 16px */
}

/* 使用者放大頁面時，所有元素會等比例縮放 */
```

**🎯 練習任務：**

```css
/* 試試看這些效果的差異 */

/* 固定寬度 */
.box1 {
  width: 300px;  /* 永遠是 300px */
}

/* 百分比寬度 */
.box2 {
  width: 50%;    /* 父元素寬度的一半 */
}

/* 視窗相對寬度 */
.box3 {
  width: 50vw;   /* 視窗寬度的一半 */
}
```

試著調整瀏覽器視窗大小，觀察三個盒子的變化！

---

## 📋 第二階段：文字與排版

### 2.1 字體屬性

```css
.text {
  /* 字體家族（會依序嘗試，找不到就用下一個）*/
  font-family: Arial, Helvetica, sans-serif;

  /* 字體大小 */
  font-size: 16px;
  font-size: 1rem;  /* 推薦使用 rem */

  /* 粗細 */
  font-weight: 400;    /* normal */
  font-weight: 700;    /* bold */
  font-weight: bold;   /* 等同 700 */

  /* 樣式 */
  font-style: normal;
  font-style: italic;  /* 斜體 */

  /* 行高（影響行與行之間的距離）*/
  line-height: 1.5;    /* 字體大小的 1.5 倍（推薦無單位）*/
  line-height: 24px;

  /* 字距 */
  letter-spacing: 0.5px;  /* 字元間距 */
  word-spacing: 2px;      /* 單字間距 */

  /* 文字裝飾 */
  text-decoration: none;       /* 移除底線 */
  text-decoration: underline;  /* 底線 */
  text-decoration: line-through;  /* 刪除線 */

  /* 文字轉換 */
  text-transform: uppercase;    /* 全大寫 */
  text-transform: lowercase;    /* 全小寫 */
  text-transform: capitalize;   /* 首字母大寫 */

  /* 對齊 */
  text-align: left;      /* 靠左（預設）*/
  text-align: center;    /* 置中 */
  text-align: right;     /* 靠右 */
  text-align: justify;   /* 左右對齊 */
}
```

**使用 Google Fonts：**

```html
<!-- index.html -->
<head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
</head>
```

```css
/* src/index.css */
body {
  font-family: 'Noto Sans TC', sans-serif;
}
```

**實際應用：**

```css
/* 標題樣式 */
h1 {
  font-size: 2.5rem;      /* 40px */
  font-weight: 700;       /* 粗體 */
  line-height: 1.2;       /* 較緊密 */
  letter-spacing: -0.5px; /* 略微縮減間距 */
  color: #1a1a1a;
}

/* 內文樣式 */
p {
  font-size: 1rem;        /* 16px */
  font-weight: 400;       /* 正常 */
  line-height: 1.6;       /* 增加可讀性 */
  color: #333;
}

/* 連結樣式 */
a {
  color: #667eea;
  text-decoration: none;  /* 移除預設底線 */
}

a:hover {
  text-decoration: underline;  /* 滑鼠移上時顯示底線 */
}
```

---

### 2.2 文字溢出處理

```css
/* 單行文字溢出顯示省略號 */
.single-line {
  width: 200px;
  white-space: nowrap;       /* 不換行 */
  overflow: hidden;          /* 隱藏溢出內容 */
  text-overflow: ellipsis;   /* 顯示省略號 */
}

/* 多行文字溢出（需要特定語法）*/
.multi-line {
  display: -webkit-box;
  -webkit-line-clamp: 3;           /* 顯示 3 行 */
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

**🎯 練習任務：**

建立個人資訊卡片，應用文字樣式：

```css
.profile-card {
  max-width: 400px;
  padding: 2rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.profile-card h2 {
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: #1a1a1a;
}

.profile-card .title {
  font-size: 1.125rem;
  color: #667eea;
  margin-bottom: 1rem;
}

.profile-card p {
  font-size: 1rem;
  line-height: 1.6;
  color: #666;
}
```

---

## 📋 第三階段：佈局（Layout）- 最重要的部分

### 3.1 Display 屬性 - 元素的顯示方式

```css
.element {
  /* === 塊級元素（Block）=== */
  display: block;
  /* 特性：
     - 獨佔一行
     - 可設定 width/height
     - 預設寬度是父元素的 100%
     - 例如：<div>, <p>, <h1>
  */

  /* === 行內元素（Inline）=== */
  display: inline;
  /* 特性：
     - 不會換行，可以並排
     - 無法設定 width/height
     - 只佔內容的寬度
     - 例如：<span>, <a>, <strong>
  */

  /* === 行內塊級元素（Inline-Block）=== */
  display: inline-block;
  /* 特性：
     - 可以並排
     - 可設定 width/height
     - 常用於按鈕、標籤
  */

  /* === 隱藏元素 === */
  display: none;
  /* 完全移除，不佔空間 */
}

.element2 {
  visibility: hidden;
  /* 隱藏但仍佔空間 */
}
```

**實際範例：**

```html
<!-- 預設 <div> 是 block -->
<div>我是塊級元素</div>
<div>我會換行</div>

<!-- 預設 <span> 是 inline -->
<span>我是行內元素</span>
<span>我不會換行</span>
```

```css
/* 讓 <a> 變成按鈕樣式 */
.button {
  display: inline-block;  /* 可設定寬高，但可並排 */
  padding: 0.75rem 1.5rem;
  background: #667eea;
  color: white;
  text-decoration: none;
  border-radius: 4px;
}
```

---

### 3.2 Flexbox - 現代佈局的核心

**為什麼需要 Flexbox？**
- 輕鬆實現水平/垂直置中
- 自動分配空間
- 響應式佈局更簡單

**基本概念：**

```
┌─────────────────────────────────────────┐
│  Flex Container (容器)                   │
│  ┌────────┐  ┌────────┐  ┌────────┐     │
│  │ Flex   │  │ Flex   │  │ Flex   │     │
│  │ Item 1 │  │ Item 2 │  │ Item 3 │     │
│  └────────┘  └────────┘  └────────┘     │
└─────────────────────────────────────────┘
       ↑                                    ↑
    主軸（Main Axis）                    交叉軸（Cross Axis）
```

**容器屬性（設定在父元素）：**

```css
.container {
  display: flex;  /* 啟用 Flexbox */

  /* === 主軸方向 === */
  flex-direction: row;         /* 水平排列（預設）*/
  flex-direction: column;      /* 垂直排列 */
  flex-direction: row-reverse; /* 水平反向 */

  /* === 主軸對齊 === */
  justify-content: flex-start;    /* 靠左（預設）*/
  justify-content: center;        /* 置中 */
  justify-content: flex-end;      /* 靠右 */
  justify-content: space-between; /* 平均分配，兩端對齊 */
  justify-content: space-around;  /* 平均分配，兩端留空 */
  justify-content: space-evenly;  /* 完全平均分配 */

  /* === 交叉軸對齊 === */
  align-items: stretch;     /* 拉伸填滿（預設）*/
  align-items: flex-start;  /* 靠上 */
  align-items: center;      /* 垂直置中 */
  align-items: flex-end;    /* 靠下 */

  /* === 換行 === */
  flex-wrap: nowrap;  /* 不換行（預設）*/
  flex-wrap: wrap;    /* 自動換行 */

  /* === 間距 === */
  gap: 1rem;          /* 子元素之間的間距 */
  gap: 1rem 2rem;     /* 行間距 列間距 */
}
```

**子元素屬性：**

```css
.item {
  /* 彈性伸縮 */
  flex: 1;        /* 簡寫：flex-grow: 1; flex-shrink: 1; flex-basis: 0; */
  flex-grow: 1;   /* 分配剩餘空間的比例 */
  flex-shrink: 1; /* 空間不足時的收縮比例 */
  flex-basis: 200px;  /* 初始大小 */

  /* 單獨對齊 */
  align-self: center;  /* 覆蓋父元素的 align-items */
}
```

**實際應用範例：**

```tsx
// 1. 水平置中
<div className="header">
  <h1>我的履歷</h1>
  <nav>...</nav>
</div>
```

```css
.header {
  display: flex;
  justify-content: space-between;  /* 標題靠左，導航靠右 */
  align-items: center;             /* 垂直置中 */
  padding: 1rem 2rem;
}
```

```tsx
// 2. 卡片排列
<div className="card-list">
  <div className="card">Card 1</div>
  <div className="card">Card 2</div>
  <div className="card">Card 3</div>
</div>
```

```css
.card-list {
  display: flex;
  gap: 1.5rem;      /* 卡片之間的間距 */
  flex-wrap: wrap;  /* 螢幕太小時自動換行 */
}

.card {
  flex: 1 1 300px;  /* 最小寬度 300px，剩餘空間平均分配 */
}
```

```tsx
// 3. 完美置中（水平 + 垂直）
<div className="center-container">
  <div className="content">我會完美置中</div>
</div>
```

```css
.center-container {
  display: flex;
  justify-content: center;  /* 水平置中 */
  align-items: center;      /* 垂直置中 */
  height: 100vh;            /* 填滿視窗高度 */
}
```

**🎯 Flexbox 互動遊戲：**
- [Flexbox Froggy](https://flexboxfroggy.com/#zh-tw) - 超推薦！用遊戲學 Flexbox

---

### 3.3 Grid - 二維佈局系統

**Flexbox vs Grid：**
- Flexbox：一維佈局（一行或一列）
- Grid：二維佈局（同時控制行和列）

**基本語法：**

```css
.container {
  display: grid;

  /* === 定義列（Columns）=== */
  grid-template-columns: 200px 200px 200px;  /* 3 列，各 200px */
  grid-template-columns: 1fr 1fr 1fr;        /* 3 列，平均分配 */
  grid-template-columns: 1fr 2fr 1fr;        /* 中間列是兩倍寬 */
  grid-template-columns: repeat(3, 1fr);     /* 簡寫：3 列平均分配 */
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));  /* 響應式！*/

  /* === 定義行（Rows）=== */
  grid-template-rows: 100px 200px;

  /* === 間距 === */
  gap: 1rem;          /* 行列間距 */
  gap: 1rem 2rem;     /* 行間距 列間距 */
  row-gap: 1rem;      /* 只設定行間距 */
  column-gap: 2rem;   /* 只設定列間距 */
}
```

**實際應用：**

```tsx
// 專案作品集
<div className="projects-grid">
  <div className="project">Project 1</div>
  <div className="project">Project 2</div>
  <div className="project">Project 3</div>
  <div className="project">Project 4</div>
</div>
```

```css
/* 固定 3 欄 */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}

/* 響應式：自動調整欄數 */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}
/*
  原理：
  - auto-fit：自動計算可以放幾欄
  - minmax(300px, 1fr)：最小 300px，最大平均分配
  - 螢幕寬時：顯示多欄
  - 螢幕窄時：自動減少欄數
*/
```

**進階：Grid 區域命名**

```css
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
  grid-template-columns: 200px 1fr 1fr;
  gap: 1rem;
}

.header  { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main    { grid-area: main; }
.footer  { grid-area: footer; }
```

**🎯 Grid 互動遊戲：**
- [Grid Garden](https://cssgridgarden.com/#zh-tw) - 用遊戲學 Grid

---

### 3.4 定位（Position）

```css
.element {
  /* === Static（預設）=== */
  position: static;
  /* 正常文件流，無法使用 top/left 等屬性 */

  /* === Relative（相對定位）=== */
  position: relative;
  top: 10px;     /* 相對於原本位置向下移 10px */
  left: 20px;    /* 相對於原本位置向右移 20px */
  /* 原本的空間仍保留 */

  /* === Absolute（絕對定位）=== */
  position: absolute;
  top: 0;
  right: 0;
  /* 相對於「最近的有定位的父元素」（非 static）*/
  /* 脫離正常文件流，不佔空間 */

  /* === Fixed（固定定位）=== */
  position: fixed;
  top: 0;
  left: 0;
  /* 相對於視窗，捲動時不會移動 */
  /* 常用於固定導航列 */

  /* === Sticky（黏性定位）=== */
  position: sticky;
  top: 0;
  /* 捲動到指定位置後會「黏」住 */
  /* 常用於表格標題 */
}
```

**實際應用：**

```tsx
// 1. 固定導航列
<nav className="navbar">...</nav>
```

```css
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  z-index: 1000;  /* 確保在最上層 */
}

/* 注意：因為導航列脫離文件流，下方內容會被遮住 */
body {
  padding-top: 60px;  /* 導航列的高度 */
}
```

```tsx
// 2. 卡片角標
<div className="card">
  <span className="badge">新</span>
  <h3>標題</h3>
</div>
```

```css
.card {
  position: relative;  /* 建立定位基準 */
  padding: 1rem;
  border: 1px solid #ddd;
}

.badge {
  position: absolute;
  top: -10px;
  right: -10px;
  background: red;
  color: white;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
}
```

**z-index（層級）：**

```css
.element {
  position: relative;  /* 必須有定位才能使用 z-index */
  z-index: 10;         /* 數字越大越上層 */
}
```

---

## 📋 第四階段：響應式設計

### 4.1 Media Queries - 媒體查詢

**語法：**

```css
/* 基本語法 */
@media (條件) {
  /* 符合條件時套用的樣式 */
}

/* 常用條件 */
@media (max-width: 768px) {
  /* 螢幕寬度 ≤ 768px 時套用（手機）*/
}

@media (min-width: 1024px) {
  /* 螢幕寬度 ≥ 1024px 時套用（桌機）*/
}

@media (min-width: 768px) and (max-width: 1023px) {
  /* 768px ~ 1023px 之間（平板）*/
}
```

**常用斷點（Breakpoints）：**

```css
/* 手機優先（Mobile First）- 推薦 */

/* 手機（預設）*/
.container {
  padding: 1rem;
}

/* 平板（≥ 768px）*/
@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}

/* 桌機（≥ 1024px）*/
@media (min-width: 1024px) {
  .container {
    padding: 3rem;
    max-width: 1200px;
    margin: 0 auto;
  }
}
```

**實際應用：**

```css
/* 導航列：手機版垂直，桌機版水平 */
.nav {
  display: flex;
  flex-direction: column;  /* 手機：垂直 */
  gap: 0.5rem;
}

@media (min-width: 768px) {
  .nav {
    flex-direction: row;  /* 平板以上：水平 */
    gap: 2rem;
  }
}

/* Grid：響應式欄數 */
.grid {
  display: grid;
  grid-template-columns: 1fr;  /* 手機：1 欄 */
  gap: 1rem;
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);  /* 平板：2 欄 */
  }
}

@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);  /* 桌機：3 欄 */
  }
}
```

---

### 4.2 響應式單位

```css
/* === 視窗相對單位（Viewport Units）=== */
.hero {
  width: 100vw;   /* 視窗寬度的 100% */
  height: 100vh;  /* 視窗高度的 100% */
  font-size: 5vw; /* 視窗寬度的 5%（螢幕越大字越大）*/
}

/* === clamp() 函式 - 響應式字體 === */
.title {
  /* 語法：clamp(最小值, 理想值, 最大值) */
  font-size: clamp(1.5rem, 5vw, 3rem);
  /*
    - 最小 1.5rem（24px）
    - 理想值是視窗寬度的 5%
    - 最大 3rem（48px）
  */
}

/* === min() / max() === */
.container {
  width: min(90%, 1200px);  /* 取較小值：90% 或 1200px */
  /* 小螢幕：90%，大螢幕：最多 1200px */
}
```

---

## 📋 第五階段：進階效果

### 5.1 過渡與動畫

**Transition（過渡）：**

```css
.button {
  background: #667eea;
  color: white;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;

  /* 過渡效果 */
  transition: background 0.3s ease;
  /* 語法：transition: 屬性 時間 緩動函式; */
}

.button:hover {
  background: #5568d3;
}

/* 多個屬性 */
.card {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
  /* all = 所有可動畫的屬性 */
}

.card:hover {
  transform: translateY(-8px);  /* 往上移 8px */
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
}
```

**緩動函式（Easing）：**

```css
.element {
  transition: all 0.3s ease;        /* 慢→快→慢 */
  transition: all 0.3s ease-in;     /* 慢→快 */
  transition: all 0.3s ease-out;    /* 快→慢 */
  transition: all 0.3s ease-in-out; /* 慢→快→慢（較明顯）*/
  transition: all 0.3s linear;      /* 等速 */
  transition: all 0.3s cubic-bezier(0.68, -0.55, 0.27, 1.55);  /* 自訂 */
}
```

**Animation（動畫）：**

```css
/* 1. 定義動畫 */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 或使用百分比 */
@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

/* 2. 套用動畫 */
.fade-in {
  animation: fadeIn 0.6s ease-out;
  /* 語法：animation: 名稱 時間 緩動函式; */
}

.pulsing {
  animation: pulse 2s ease-in-out infinite;
  /* infinite = 無限循環 */
}
```

**實用範例：**

```css
/* Loading Spinner */
@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #667eea;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

/* 滑入動畫 */
@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-100px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.slide-in {
  animation: slideIn 0.5s ease-out;
}
```

---

### 5.2 Transform（變形）

```css
.element {
  /* 平移 */
  transform: translateX(50px);      /* 右移 50px */
  transform: translateY(-20px);     /* 上移 20px */
  transform: translate(50px, -20px); /* X, Y */

  /* 縮放 */
  transform: scale(1.2);        /* 放大 1.2 倍 */
  transform: scale(0.8);        /* 縮小到 0.8 倍 */
  transform: scale(1.5, 0.8);   /* X 軸 1.5 倍，Y 軸 0.8 倍 */

  /* 旋轉 */
  transform: rotate(45deg);     /* 順時針旋轉 45 度 */
  transform: rotate(-90deg);    /* 逆時針旋轉 90 度 */

  /* 傾斜 */
  transform: skew(10deg, 5deg); /* X 軸 10 度，Y 軸 5 度 */

  /* 組合（從右到左執行）*/
  transform: translateX(50px) rotate(45deg) scale(1.2);

  /* 變形原點 */
  transform-origin: center;      /* 預設：中心點 */
  transform-origin: top left;    /* 左上角 */
  transform-origin: 50% 50%;     /* 自訂位置 */
}
```

**實際應用：**

```css
/* 卡片 hover 效果 */
.card {
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-10px) scale(1.02);
}

/* 按鈕點擊效果 */
.button:active {
  transform: scale(0.95);
}

/* 圖片放大 */
.image-container {
  overflow: hidden;  /* 隱藏溢出部分 */
}

.image-container img {
  transition: transform 0.3s ease;
}

.image-container:hover img {
  transform: scale(1.1);
}
```

---

### 5.3 陰影與漸層

**Box Shadow（盒子陰影）：**

```css
.element {
  /* 語法：box-shadow: X偏移 Y偏移 模糊半徑 擴散半徑 顏色; */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

  /* 多層陰影（用逗號分隔）*/
  box-shadow:
    0 2px 4px rgba(0, 0, 0, 0.1),
    0 8px 16px rgba(0, 0, 0, 0.1);

  /* 內陰影 */
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

**實用陰影範例：**

```css
/* 卡片陰影 */
.card {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.card:hover {
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

/* 按鈕陰影 */
.button {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.button:active {
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

**Gradient（漸層）：**

```css
.element {
  /* 線性漸層 */
  background: linear-gradient(to right, #667eea, #764ba2);
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

  /* 徑向漸層 */
  background: radial-gradient(circle, #667eea, #764ba2);

  /* 多色漸層 */
  background: linear-gradient(
    to right,
    #ff0000 0%,
    #ffff00 25%,
    #00ff00 50%,
    #0000ff 100%
  );

  /* 漸層疊加 */
  background:
    linear-gradient(135deg, rgba(102, 126, 234, 0.8), rgba(118, 75, 162, 0.8)),
    url('background.jpg');
  background-size: cover;
}
```

**實際應用：**

```css
/* Header 背景漸層 */
.header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 3rem 1rem;
}

/* 文字漸層 */
.gradient-text {
  background: linear-gradient(135deg, #667eea, #764ba2);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* 按鈕漸層 */
.button {
  background: linear-gradient(135deg, #667eea, #764ba2);
  border: none;
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
}
```

---

## 📋 第六階段：實戰技巧

### 6.1 CSS Variables（CSS 變數）

**為什麼需要？**
- 集中管理顏色、字體大小等
- 支援深色模式切換
- 提升程式碼可維護性

**語法：**

```css
/* 1. 定義變數（通常在 :root）*/
:root {
  /* 顏色 */
  --primary-color: #667eea;
  --secondary-color: #764ba2;
  --text-color: #333;
  --bg-color: #ffffff;

  /* 字體 */
  --font-size-base: 16px;
  --font-size-lg: 1.25rem;
  --font-size-xl: 1.75rem;

  /* 間距 */
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 2rem;

  /* 陰影 */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* 2. 使用變數 */
.button {
  background: var(--primary-color);
  padding: var(--spacing-md) var(--spacing-lg);
  box-shadow: var(--shadow-sm);
  font-size: var(--font-size-base);
}

/* 3. 變數可以覆蓋 */
.card {
  --primary-color: #ff6b6b;  /* 只在 .card 內生效 */
  background: var(--primary-color);
}
```

**深色模式範例：**

```css
/* src/index.css */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #212121;
  --text-secondary: #757575;
  --border-color: #e0e0e0;
}

/* 深色模式變數 */
.dark {
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --text-primary: #ffffff;
  --text-secondary: #b0b0b0;
  --border-color: #404040;
}

/* 使用變數 */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  transition: background-color 0.3s, color 0.3s;
}

.card {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
}
```

---

### 6.2 偽類與偽元素

**偽類（Pseudo-classes）：**

```css
/* 連結狀態 */
a:link    { color: blue; }      /* 未訪問 */
a:visited { color: purple; }    /* 已訪問 */
a:hover   { color: red; }       /* 滑鼠移上 */
a:active  { color: green; }     /* 點擊中 */
a:focus   { outline: 2px solid blue; }  /* 鍵盤焦點 */

/* 表單狀態 */
input:focus { border-color: blue; }
input:disabled { opacity: 0.5; }
input:checked + label { font-weight: bold; }

/* 結構性偽類 */
li:first-child { font-weight: bold; }
li:last-child { margin-bottom: 0; }
li:nth-child(odd) { background: #f9f9f9; }  /* 奇數行 */
li:nth-child(even) { background: white; }   /* 偶數行 */
li:nth-child(3) { color: red; }             /* 第 3 個 */

/* 其他 */
:not(.active) { opacity: 0.5; }   /* 排除 .active */
:empty { display: none; }         /* 空元素 */
```

**偽元素（Pseudo-elements）：**

```css
/* 前後插入內容 */
.quote::before {
  content: '"';
  font-size: 2rem;
  color: #999;
}

.quote::after {
  content: '"';
}

/* 首字放大 */
p::first-letter {
  font-size: 2rem;
  font-weight: bold;
  float: left;
  margin-right: 0.5rem;
}

/* 首行樣式 */
p::first-line {
  font-weight: bold;
  color: #667eea;
}

/* 使用者選取的文字 */
::selection {
  background: #667eea;
  color: white;
}
```

**實際應用：**

```css
/* 必填欄位標記 */
.required::after {
  content: ' *';
  color: red;
}

/* 外部連結圖示 */
a[href^="http"]::after {
  content: ' 🔗';
}

/* 清除浮動 */
.clearfix::after {
  content: '';
  display: table;
  clear: both;
}
```

---

### 6.3 除錯技巧

**1. 使用 Chrome DevTools：**

```
1. 右鍵元素 → 檢查（Inspect）
2. Elements 頁籤：查看 HTML 結構
3. Styles 面板：
   - 查看套用的所有 CSS 規則
   - 被劃掉的規則 = 被覆蓋
   - 勾選/取消勾選屬性測試效果
   - 直接修改數值即時預覽
4. Computed 頁籤：查看最終計算結果
5. Layout 頁籤：視覺化 Box Model
```

**2. 邊框除錯法：**

```css
/* 快速找出元素範圍 */
* {
  outline: 1px solid red;
}

/* 或針對特定元素 */
.container * {
  outline: 1px solid blue;
}
```

**3. 常見問題與解法：**

```css
/* 問題：margin 沒效果？ */
.inline-element {
  display: inline;
  margin-top: 20px;  /* ❌ inline 元素無法設定垂直 margin */
}

/* 解法： */
.inline-element {
  display: inline-block;  /* ✅ 改成 inline-block */
  margin-top: 20px;
}

/* 問題：垂直置中失敗？ */
.container {
  text-align: center;  /* ❌ 這只能水平置中文字 */
}

/* 解法：使用 Flexbox */
.container {
  display: flex;
  justify-content: center;  /* 水平置中 */
  align-items: center;      /* 垂直置中 */
}

/* 問題：寬度設定無效？ */
.element {
  display: inline;  /* ❌ inline 無法設定寬高 */
  width: 200px;
}

/* 解法： */
.element {
  display: block;  /* 或 inline-block */
  width: 200px;
}
```

---

## 🎯 完整專案實戰

### 履歷網站 - 完整 CSS 架構

```css
/* ===== 1. 全域設定 ===== */
/* src/index.css */

/* CSS Reset */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* CSS 變數 */
:root {
  /* 顏色 */
  --primary: #667eea;
  --secondary: #764ba2;
  --text-dark: #1a1a1a;
  --text-light: #666;
  --bg-white: #ffffff;
  --bg-gray: #f5f5f5;
  --border: #e0e0e0;

  /* 字體 */
  --font-base: 16px;
  --font-sm: 0.875rem;
  --font-lg: 1.25rem;
  --font-xl: 1.75rem;
  --font-2xl: 2.5rem;

  /* 間距 */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;
  --space-xl: 3rem;

  /* 陰影 */
  --shadow-sm: 0 2px 4px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 4px 8px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 8px 24px rgba(0, 0, 0, 0.15);

  /* 圓角 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 16px;
}

/* 基礎樣式 */
body {
  font-family: 'Noto Sans TC', -apple-system, sans-serif;
  font-size: var(--font-base);
  line-height: 1.6;
  color: var(--text-dark);
  background: var(--bg-gray);
}

/* 標題 */
h1, h2, h3, h4, h5, h6 {
  line-height: 1.2;
  margin-bottom: var(--space-md);
}

h1 { font-size: var(--font-2xl); }
h2 { font-size: var(--font-xl); }
h3 { font-size: var(--font-lg); }

/* 連結 */
a {
  color: var(--primary);
  text-decoration: none;
  transition: color 0.3s ease;
}

a:hover {
  color: var(--secondary);
}

/* 圖片 */
img {
  max-width: 100%;
  height: auto;
  display: block;
}

/* ===== 2. 元件樣式 ===== */

/* Header */
.header {
  background: linear-gradient(135deg, var(--primary), var(--secondary));
  color: white;
  text-align: center;
  padding: var(--space-xl) var(--space-md);
}

.header h1 {
  font-size: var(--font-2xl);
  margin-bottom: var(--space-sm);
}

.header-subtitle {
  font-size: var(--font-lg);
  opacity: 0.9;
  margin-bottom: var(--space-lg);
}

.contact-links {
  display: flex;
  gap: var(--space-md);
  justify-content: center;
  flex-wrap: wrap;
}

.contact-links a {
  color: white;
  padding: var(--space-sm) var(--space-md);
  border: 2px solid white;
  border-radius: var(--radius-sm);
  transition: all 0.3s ease;
}

.contact-links a:hover {
  background: white;
  color: var(--primary);
}

/* Card */
.card {
  background: var(--bg-white);
  padding: var(--space-lg);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-sm);
  transition: all 0.3s ease;
}

.card:hover {
  box-shadow: var(--shadow-lg);
  transform: translateY(-4px);
}

/* Grid Layout */
.grid {
  display: grid;
  gap: var(--space-lg);
  padding: var(--space-lg);
}

@media (min-width: 768px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
    max-width: 1200px;
    margin: 0 auto;
  }
}

/* Button */
.button {
  display: inline-block;
  padding: var(--space-sm) var(--space-lg);
  background: var(--primary);
  color: white;
  border: none;
  border-radius: var(--radius-sm);
  font-size: var(--font-base);
  cursor: pointer;
  transition: all 0.3s ease;
}

.button:hover {
  background: var(--secondary);
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}

.button:active {
  transform: translateY(0);
}

/* Utility Classes */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-md);
}

.text-center { text-align: center; }
.mt-1 { margin-top: var(--space-sm); }
.mt-2 { margin-top: var(--space-md); }
.mt-3 { margin-top: var(--space-lg); }
```

---

## 📚 學習資源推薦

### 互動遊戲
- [Flexbox Froggy](https://flexboxfroggy.com/#zh-tw) - Flexbox 遊戲
- [Grid Garden](https://cssgridgarden.com/#zh-tw) - Grid 遊戲
- [CSS Diner](https://flukeout.github.io/) - 選擇器遊戲

### 線上文件
- [MDN CSS 文件](https://developer.mozilla.org/zh-TW/docs/Web/CSS) - 最權威的參考文件
- [CSS-Tricks](https://css-tricks.com/) - 實用技巧與範例
- [Can I Use](https://caniuse.com/) - 檢查瀏覽器支援度

### 靈感與工具
- [CodePen](https://codepen.io/) - 查看別人的作品
- [Dribbble](https://dribbble.com/) - 設計靈感
- [Coolors](https://coolors.co/) - 配色工具
- [Google Fonts](https://fonts.google.com/) - 免費字體

---

## 🎯 學習檢查清單

### 基礎（必學）
- [ ] 理解 Box Model
- [ ] 掌握選擇器與優先權
- [ ] 熟悉顏色與單位
- [ ] 會使用 Chrome DevTools 除錯

### 佈局（核心）
- [ ] 熟練 Flexbox
- [ ] 理解 Grid 基礎
- [ ] 了解 Position 定位
- [ ] 能夠實作響應式佈局

### 進階
- [ ] 會使用 Transition 和 Animation
- [ ] 理解 Transform
- [ ] 掌握 CSS Variables
- [ ] 熟悉偽類與偽元素

### 實戰
- [ ] 完成履歷網站 CSS
- [ ] 實作深色模式
- [ ] 所有元件都有 hover 效果
- [ ] 通過響應式測試

---

## 💡 學習建議

1. **邊做邊學** - 不要只看，要動手寫
2. **使用 DevTools** - 每天打開來玩玩看
3. **從模仿開始** - 找喜歡的網站，試著做出類似效果
4. **不用背** - 忘記語法很正常，Google 就好
5. **多看別人的程式碼** - CodePen、GitHub 都是寶庫

祝學習順利！🚀
