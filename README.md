# React Practice

這是一個使用 Vite + React + TypeScript 建立的學習專案。

## 建立專案

```bash
npm create vite@latest . -- --template react-ts
```

## 安裝依賴

```bash
npm install
```

## 啟動開發伺服器

```bash
npm run dev
```

## 專案結構說明

### 入口檔案（運行流程）

```
index.html (HTML 入口)
    ↓
src/main.tsx (React 入口)
    ↓
src/App.tsx (主元件)
```

1. **index.html** - HTML 入口檔案
   - 包含 `<div id="root"></div>` 作為 React 掛載點
   - 引入 `src/main.tsx` 作為 JavaScript 模組

2. **src/main.tsx** - JavaScript 入口檔案
   - 使用 `createRoot` 將 React 渲染到 `#root` 元素
   - 引入 App 元件

3. **src/App.tsx** - 主要元件
   - React 應用的根元件
   - 範例包含計數器功能（使用 `useState` Hook）

### 設定檔案

**建置工具：**
- **vite.config.ts** - Vite 設定檔（配置開發伺服器、建置選項）

**TypeScript：**
- **tsconfig.json** - TypeScript 主設定檔（參照子設定檔）
- **tsconfig.app.json** - 應用程式碼的 TypeScript 設定
- **tsconfig.node.json** - Node 工具的 TypeScript 設定

**程式碼品質：**
- **eslint.config.js** - ESLint 設定檔（檢查程式碼品質和風格）

### 套件管理

- **package.json** - 專案清單檔
  - `npm run dev` - 啟動開發伺服器
  - `npm run build` - 建置生產版本
  - `npm run lint` - 執行程式碼檢查
  - `npm run preview` - 預覽建置結果

- **package-lock.json** - 鎖定依賴版本

### 樣式與資源

- **src/index.css** - 全域樣式
- **src/App.css** - App 元件的樣式
- **src/assets/** - 圖片、字型等靜態資源
- **public/** - 公開資源目錄（檔案會直接複製到建置輸出）

## 開發流程

**開發時：**
```
Vite 開發伺服器 → 讀取 vite.config.ts → 啟動 HMR → 即時編譯 → 瀏覽器顯示
```

**建置時：**
```
TypeScript 編譯 (tsc -b) → Vite 打包優化 → 產生 dist/ 目錄
```

## 學習重點

1. **單頁應用 (SPA)** - 只有一個 HTML 檔案，內容由 React 動態渲染
2. **元件化開發** - 將 UI 拆分成可重用的元件
3. **熱更新 (HMR - Hot Module Replacement)** - 修改程式碼立即看到結果，無需手動重新整理，並保留應用程式狀態
4. **TypeScript** - 提供型別檢查，減少執行時期錯誤
