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

## 測試

本專案使用 **Vitest + React Testing Library** 進行測試。

### 安裝測試套件

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

### 測試指令

- `npm run test` - 執行測試（watch 模式）
- `npm run test:ui` - 開啟測試 UI 介面
- `npm run test:coverage` - 產生測試覆蓋率報告

### 測試檔案結構（Colocation）

測試檔案與元件放在同一目錄：

```
src/
├── components/
│   ├── Button.tsx
│   ├── Button.test.tsx       ← 測試檔案
│   ├── Card.tsx
│   └── Card.test.tsx          ← 測試檔案
├── App.tsx
├── App.test.tsx               ← 測試檔案
└── test/
    └── setup.ts               ← 測試環境設定
```

**優點：**
- 修改元件時，測試就在旁邊，不用切換目錄
- 刪除元件時不會忘記刪除測試
- 容易看出哪個元件有測試、哪個沒有

**注意：** 測試檔案不會被打包進 production（Vite 建置時會自動排除）

### 測試設定檔

- **vitest.config.ts** - Vitest 測試框架設定檔
- **src/test/setup.ts** - 測試環境設定（如 @testing-library/jest-dom）

## 學習重點

1. **單頁應用 (SPA)** - 只有一個 HTML 檔案，內容由 React 動態渲染
2. **元件化開發** - 將 UI 拆分成可重用的元件
3. **熱更新 (HMR - Hot Module Replacement)** - 修改程式碼立即看到結果，無需手動重新整理，並保留應用程式狀態
4. **TypeScript** - 提供型別檢查，減少執行時期錯誤
5. **單向資料流 (Unidirectional Data Flow)** - React 的核心設計理念，資料只能由上而下傳遞

### 單向資料流詳解

在 React 中，資料只能**由上而下**（從父元件到子元件）傳遞，形成單一方向的流動。

**資料流動方向：**
```
父元件 (Parent Component)
    ↓ props
子元件 (Child Component)
    ↓ props
孫元件 (Grandchild Component)
```

**核心規則：**
1. **資料往下流** - 透過 `props` 傳遞給子元件
2. **子元件不能修改 props** - props 是唯讀的
3. **事件往上傳** - 透過回調函式（callback）通知父元件
4. **父元件控制狀態** - 由父元件決定是否更新狀態

**範例：**
```tsx
// 父元件持有狀態
function App() {
  const [count, setCount] = useState(0)  // ← 資料源頭

  return (
    <div>
      {/* 透過 props 向下傳遞 */}
      <Counter count={count} onIncrement={() => setCount(count + 1)} />
    </div>
  )
}

// 子元件接收 props（唯讀）
function Counter({ count, onIncrement }) {
  // ❌ 不能直接修改 count
  // ✅ 只能透過父元件提供的函式來「請求」更新
  return <button onClick={onIncrement}>{count}</button>
}
```

**優點：**
- ✅ **可預測性** - 資料流動方向明確，容易追蹤資料變化
- ✅ **除錯容易** - 只需檢查父元件就知道資料來源
- ✅ **程式碼維護** - 避免多個元件同時修改同一資料造成混亂
- ✅ **效能最佳化** - React 可以準確判斷哪些元件需要重新渲染
