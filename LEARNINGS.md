# React 履歷網站學習路線

## 🎯 專案目標

透過建立個人履歷網站，循序漸進學習 React 核心概念與實戰技巧。

---

## 📋 第一階段：基礎元件與靜態內容

### 學習重點
- JSX 語法與元件結構
- Props 資料傳遞
- 陣列渲染（`.map()`）
- 基本 TypeScript 型別定義

---

### 1.1 個人資訊卡片（Header 元件）

**學習目標：**
- 建立第一個 React 元件
- 了解 JSX 語法
- 使用 Props 傳遞資料

**實作內容：**
```tsx
// src/components/Header.tsx
interface HeaderProps {
  name: string
  title: string
  email: string
  github: string
  linkedin: string
}

export function Header({ name, title, email, github, linkedin }: HeaderProps) {
  return (
    <header className="header">
      <h1>{name}</h1>
      <p className="title">{title}</p>
      <div className="contact-links">
        <a href={`mailto:${email}`}>Email</a>
        <a href={github} target="_blank">GitHub</a>
        <a href={linkedin} target="_blank">LinkedIn</a>
      </div>
    </header>
  )
}
```

**在 App.tsx 使用：**
```tsx
import { Header } from './components/Header'

function App() {
  return (
    <div className="app">
      <Header
        name="張三"
        title="前端工程師"
        email="example@email.com"
        github="https://github.com/username"
        linkedin="https://linkedin.com/in/username"
      />
    </div>
  )
}
```

**CSS 樣式範例：**
```css
/* src/components/Header.css */
.header {
  text-align: center;
  padding: 3rem 1rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.header h1 {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.title {
  font-size: 1.25rem;
  opacity: 0.9;
  margin-bottom: 1.5rem;
}

.contact-links {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
}

.contact-links a {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border: 2px solid white;
  border-radius: 4px;
  transition: all 0.3s;
}

.contact-links a:hover {
  background: white;
  color: #667eea;
}
```

**練習任務：**
- [x] 建立 Header 元件檔案
- [x] 定義 TypeScript interface
- [x] 撰寫 CSS 樣式
- [x] 在 App.tsx 中使用元件
- [x] 嘗試修改 Props 值，觀察畫面變化

---

### 1.2 關於我（About 元件）

**學習目標：**
- 練習多行文字內容渲染
- 了解元件的可重用性
- 使用 `children` prop

**實作內容：**
```tsx
// src/components/About.tsx
interface AboutProps {
  title: string
  description: string
}

export function About({ title, description }: AboutProps) {
  return (
    <section className="about">
      <h2>{title}</h2>
      <p>{description}</p>
    </section>
  )
}
```

**進階版（使用 children）：**
```tsx
interface AboutProps {
  title: string
  children: React.ReactNode
}

export function About({ title, children }: AboutProps) {
  return (
    <section className="about">
      <h2>{title}</h2>
      <div className="about-content">{children}</div>
    </section>
  )
}

// 使用方式
<About title="關於我">
  <p>我是一名熱愛程式開發的前端工程師...</p>
  <p>專注於 React 生態系...</p>
</About>
```

**練習任務：**
- [ ] 建立基本版 About 元件
- [ ] 嘗試使用 `children` 版本
- [ ] 撰寫自我介紹內容
- [ ] 加入段落樣式（行高、字距）

---

### 1.3 技能列表（Skills 元件）

**學習目標：**
- 使用陣列 `.map()` 渲染列表
- 了解 `key` 屬性的重要性
- 定義陣列型別

**實作內容：**
```tsx
// src/components/Skills.tsx
interface Skill {
  id: number
  name: string
  level: 'beginner' | 'intermediate' | 'advanced'
}

interface SkillsProps {
  skills: Skill[]
}

export function Skills({ skills }: SkillsProps) {
  return (
    <section className="skills">
      <h2>技能</h2>
      <div className="skills-list">
        {skills.map((skill) => (
          <div key={skill.id} className={`skill-tag skill-${skill.level}`}>
            {skill.name}
          </div>
        ))}
      </div>
    </section>
  )
}
```

**在 App.tsx 準備資料：**
```tsx
const mySkills: Skill[] = [
  { id: 1, name: 'React', level: 'advanced' },
  { id: 2, name: 'TypeScript', level: 'intermediate' },
  { id: 3, name: 'CSS', level: 'advanced' },
  { id: 4, name: 'Node.js', level: 'intermediate' },
  { id: 5, name: 'Git', level: 'advanced' },
]

<Skills skills={mySkills} />
```

**CSS 樣式（不同等級顏色）：**
```css
.skills-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.skill-tag {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
}

.skill-beginner {
  background: #e3f2fd;
  color: #1976d2;
}

.skill-intermediate {
  background: #fff3e0;
  color: #f57c00;
}

.skill-advanced {
  background: #e8f5e9;
  color: #388e3c;
}
```

**練習任務：**
- [ ] 建立 Skills 元件
- [ ] 定義 Skill 型別
- [ ] 使用 `.map()` 渲染列表
- [ ] 測試加入新技能
- [ ] 為不同等級設計不同顏色

**常見錯誤：**
- ❌ 忘記加 `key` 屬性（React 會警告）
- ❌ 使用 `index` 當作 `key`（資料順序改變時會有問題）
- ✅ 使用唯一的 `id` 作為 `key`

---

## 📋 第二階段：互動與狀態管理

### 學習重點
- `useState` Hook
- 事件處理（`onClick`、`onMouseEnter`）
- 條件渲染（`&&`、三元運算子）
- 狀態提升（Lifting State Up）

---

### 2.1 可展開的經歷區塊（Experience 元件）

**學習目標：**
- 使用 `useState` 控制顯示/隱藏
- 處理點擊事件
- 條件渲染內容

**實作內容：**
```tsx
// src/components/Experience.tsx
import { useState } from 'react'

interface ExperienceItem {
  id: number
  company: string
  position: string
  period: string
  description: string
  highlights: string[]
}

interface ExperienceProps {
  experience: ExperienceItem
}

export function Experience({ experience }: ExperienceProps) {
  const [isExpanded, setIsExpanded] = useState(false)

  return (
    <div className="experience-item">
      <div
        className="experience-header"
        onClick={() => setIsExpanded(!isExpanded)}
      >
        <h3>{experience.position}</h3>
        <p className="company">{experience.company}</p>
        <p className="period">{experience.period}</p>
        <button className="toggle-btn">
          {isExpanded ? '▲ 收合' : '▼ 展開'}
        </button>
      </div>

      {isExpanded && (
        <div className="experience-details">
          <p>{experience.description}</p>
          <ul>
            {experience.highlights.map((highlight, index) => (
              <li key={index}>{highlight}</li>
            ))}
          </ul>
        </div>
      )}
    </div>
  )
}
```

**使用範例：**
```tsx
const experiences: ExperienceItem[] = [
  {
    id: 1,
    company: 'ABC 科技公司',
    position: '前端工程師',
    period: '2023/01 - 至今',
    description: '負責開發公司主要產品的前端介面',
    highlights: [
      '使用 React + TypeScript 重構舊系統，效能提升 40%',
      '導入 Vitest 測試框架，覆蓋率達 80%',
      '帶領 3 人團隊完成專案交付'
    ]
  },
  // ...更多經歷
]

<div className="experiences">
  {experiences.map(exp => (
    <Experience key={exp.id} experience={exp} />
  ))}
</div>
```

**進階版（整合動畫）：**
```css
.experience-details {
  animation: slideDown 0.3s ease-out;
  overflow: hidden;
}

@keyframes slideDown {
  from {
    opacity: 0;
    max-height: 0;
  }
  to {
    opacity: 1;
    max-height: 500px;
  }
}
```

**練習任務：**
- [ ] 建立基本的展開/收合功能
- [ ] 加入過場動畫
- [ ] 嘗試改用滑鼠懸停（`onMouseEnter`/`onMouseLeave`）
- [ ] 思考：如果只想同時展開一個項目，該如何實作？（提示：狀態提升）

---

### 2.2 專案作品集（Projects 元件）

**學習目標：**
- 滑鼠懸停效果
- 卡片佈局
- 圖片載入處理

**實作內容：**
```tsx
// src/components/ProjectCard.tsx
import { useState } from 'react'

interface Project {
  id: number
  title: string
  description: string
  image: string
  technologies: string[]
  githubUrl?: string
  liveUrl?: string
}

interface ProjectCardProps {
  project: Project
}

export function ProjectCard({ project }: ProjectCardProps) {
  const [isHovered, setIsHovered] = useState(false)

  return (
    <div
      className="project-card"
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <div className="project-image">
        <img src={project.image} alt={project.title} />
        {isHovered && (
          <div className="project-overlay">
            <h3>{project.title}</h3>
            <p>{project.description}</p>
            <div className="project-links">
              {project.githubUrl && (
                <a href={project.githubUrl} target="_blank">GitHub</a>
              )}
              {project.liveUrl && (
                <a href={project.liveUrl} target="_blank">Live Demo</a>
              )}
            </div>
          </div>
        )}
      </div>

      <div className="project-tech">
        {project.technologies.map((tech, index) => (
          <span key={index} className="tech-tag">{tech}</span>
        ))}
      </div>
    </div>
  )
}
```

**CSS 樣式：**
```css
.project-card {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.3s;
}

.project-card:hover {
  transform: translateY(-8px);
}

.project-image {
  position: relative;
  height: 200px;
  overflow: hidden;
}

.project-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.project-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.85);
  color: white;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  animation: fadeIn 0.3s;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

**練習任務：**
- [ ] 建立專案卡片元件
- [ ] 實作滑鼠懸停效果
- [ ] 使用 Grid 或 Flexbox 佈局多個卡片
- [ ] 處理圖片載入失敗（顯示預設圖片）

---

### 2.3 技能篩選器

**學習目標：**
- 使用多個 `useState`
- 陣列過濾（`.filter()`）
- 按鈕群組互動

**實作內容：**
```tsx
// src/components/SkillsFilter.tsx
import { useState } from 'react'

type SkillCategory = 'all' | 'frontend' | 'backend' | 'tools'

interface Skill {
  id: number
  name: string
  category: SkillCategory
}

const skillsData: Skill[] = [
  { id: 1, name: 'React', category: 'frontend' },
  { id: 2, name: 'TypeScript', category: 'frontend' },
  { id: 3, name: 'Node.js', category: 'backend' },
  { id: 4, name: 'Express', category: 'backend' },
  { id: 5, name: 'Git', category: 'tools' },
  { id: 6, name: 'Docker', category: 'tools' },
]

export function SkillsFilter() {
  const [selectedCategory, setSelectedCategory] = useState<SkillCategory>('all')

  const filteredSkills = selectedCategory === 'all'
    ? skillsData
    : skillsData.filter(skill => skill.category === selectedCategory)

  return (
    <section className="skills-filter">
      <div className="filter-buttons">
        <button
          className={selectedCategory === 'all' ? 'active' : ''}
          onClick={() => setSelectedCategory('all')}
        >
          全部
        </button>
        <button
          className={selectedCategory === 'frontend' ? 'active' : ''}
          onClick={() => setSelectedCategory('frontend')}
        >
          前端
        </button>
        <button
          className={selectedCategory === 'backend' ? 'active' : ''}
          onClick={() => setSelectedCategory('backend')}
        >
          後端
        </button>
        <button
          className={selectedCategory === 'tools' ? 'active' : ''}
          onClick={() => setSelectedCategory('tools')}
        >
          工具
        </button>
      </div>

      <div className="skills-list">
        {filteredSkills.map(skill => (
          <div key={skill.id} className="skill-tag">
            {skill.name}
          </div>
        ))}
      </div>

      <p className="result-count">
        顯示 {filteredSkills.length} 個技能
      </p>
    </section>
  )
}
```

**進階挑戰：**
```tsx
// 加入搜尋功能
const [searchTerm, setSearchTerm] = useState('')

const filteredSkills = skillsData
  .filter(skill => selectedCategory === 'all' || skill.category === selectedCategory)
  .filter(skill => skill.name.toLowerCase().includes(searchTerm.toLowerCase()))

<input
  type="text"
  placeholder="搜尋技能..."
  value={searchTerm}
  onChange={(e) => setSearchTerm(e.target.value)}
/>
```

**練習任務：**
- [ ] 實作基本篩選功能
- [ ] 加入搜尋框
- [ ] 顯示篩選結果數量
- [ ] 嘗試加入「多選」功能（提示：用陣列儲存選中的分類）

---

## 📋 第三階段：表單與資料驗證

### 學習重點
- 受控元件（Controlled Components）
- 表單驗證
- `useContext` 全域狀態管理
- 自定義 Hook

---

### 3.1 聯絡表單（ContactForm 元件）

**學習目標：**
- 處理多個輸入欄位
- 表單驗證
- 錯誤訊息顯示
- 表單送出處理

**實作內容：**
```tsx
// src/components/ContactForm.tsx
import { useState } from 'react'

interface FormData {
  name: string
  email: string
  message: string
}

interface FormErrors {
  name?: string
  email?: string
  message?: string
}

export function ContactForm() {
  const [formData, setFormData] = useState<FormData>({
    name: '',
    email: '',
    message: ''
  })

  const [errors, setErrors] = useState<FormErrors>({})
  const [isSubmitting, setIsSubmitting] = useState(false)
  const [submitSuccess, setSubmitSuccess] = useState(false)

  // 驗證函式
  const validate = (): boolean => {
    const newErrors: FormErrors = {}

    if (!formData.name.trim()) {
      newErrors.name = '請輸入姓名'
    }

    if (!formData.email.trim()) {
      newErrors.email = '請輸入 Email'
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) {
      newErrors.email = 'Email 格式不正確'
    }

    if (!formData.message.trim()) {
      newErrors.message = '請輸入訊息'
    } else if (formData.message.length < 10) {
      newErrors.message = '訊息至少需要 10 個字元'
    }

    setErrors(newErrors)
    return Object.keys(newErrors).length === 0
  }

  // 處理輸入變更
  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target
    setFormData(prev => ({
      ...prev,
      [name]: value
    }))
    // 清除該欄位的錯誤訊息
    if (errors[name as keyof FormErrors]) {
      setErrors(prev => ({
        ...prev,
        [name]: undefined
      }))
    }
  }

  // 送出表單
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()

    if (!validate()) {
      return
    }

    setIsSubmitting(true)

    // 模擬 API 請求
    try {
      await new Promise(resolve => setTimeout(resolve, 1500))
      console.log('表單資料：', formData)
      setSubmitSuccess(true)
      setFormData({ name: '', email: '', message: '' })
    } catch (error) {
      console.error('送出失敗', error)
    } finally {
      setIsSubmitting(false)
    }
  }

  return (
    <section className="contact-form">
      <h2>聯絡我</h2>

      {submitSuccess && (
        <div className="success-message">
          感謝您的訊息！我會盡快回覆您。
        </div>
      )}

      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label htmlFor="name">姓名 *</label>
          <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChange}
            className={errors.name ? 'error' : ''}
          />
          {errors.name && <span className="error-message">{errors.name}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="email">Email *</label>
          <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            className={errors.email ? 'error' : ''}
          />
          {errors.email && <span className="error-message">{errors.email}</span>}
        </div>

        <div className="form-group">
          <label htmlFor="message">訊息 *</label>
          <textarea
            id="message"
            name="message"
            rows={5}
            value={formData.message}
            onChange={handleChange}
            className={errors.message ? 'error' : ''}
          />
          {errors.message && <span className="error-message">{errors.message}</span>}
        </div>

        <button type="submit" disabled={isSubmitting}>
          {isSubmitting ? '送出中...' : '送出'}
        </button>
      </form>
    </section>
  )
}
```

**CSS 樣式：**
```css
.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.form-group input,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #e0e0e0;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #667eea;
}

.form-group input.error,
.form-group textarea.error {
  border-color: #f44336;
}

.error-message {
  display: block;
  color: #f44336;
  font-size: 0.875rem;
  margin-top: 0.25rem;
}

.success-message {
  background: #4caf50;
  color: white;
  padding: 1rem;
  border-radius: 4px;
  margin-bottom: 1rem;
}

button[type="submit"] {
  background: #667eea;
  color: white;
  padding: 0.75rem 2rem;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  cursor: pointer;
  transition: background 0.3s;
}

button[type="submit"]:hover:not(:disabled) {
  background: #5568d3;
}

button[type="submit"]:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
```

**練習任務：**
- [ ] 實作基本表單
- [ ] 加入即時驗證（邊輸入邊驗證）
- [ ] 嘗試使用 `react-hook-form` 函式庫
- [ ] 加入檔案上傳功能（履歷 PDF）

---

### 3.2 深色模式切換（Theme Context）

**學習目標：**
- 使用 `useContext` 管理全域狀態
- 建立自定義 Hook（`useTheme`）
- `localStorage` 儲存使用者偏好

**實作內容：**
```tsx
// src/contexts/ThemeContext.tsx
import { createContext, useContext, useState, useEffect, ReactNode } from 'react'

type Theme = 'light' | 'dark'

interface ThemeContextType {
  theme: Theme
  toggleTheme: () => void
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined)

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(() => {
    // 從 localStorage 讀取或使用系統偏好
    const saved = localStorage.getItem('theme') as Theme
    if (saved) return saved

    return window.matchMedia('(prefers-color-scheme: dark)').matches
      ? 'dark'
      : 'light'
  })

  useEffect(() => {
    // 儲存到 localStorage
    localStorage.setItem('theme', theme)
    // 更新 HTML class
    document.documentElement.className = theme
  }, [theme])

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

// 自定義 Hook
export function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) {
    throw new Error('useTheme 必須在 ThemeProvider 內使用')
  }
  return context
}
```

**在 main.tsx 使用：**
```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { ThemeProvider } from './contexts/ThemeContext'
import App from './App'
import './index.css'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </StrictMode>,
)
```

**主題切換按鈕：**
```tsx
// src/components/ThemeToggle.tsx
import { useTheme } from '../contexts/ThemeContext'

export function ThemeToggle() {
  const { theme, toggleTheme } = useTheme()

  return (
    <button
      className="theme-toggle"
      onClick={toggleTheme}
      aria-label="切換主題"
    >
      {theme === 'light' ? '🌙 深色模式' : '☀️ 淺色模式'}
    </button>
  )
}
```

**CSS 變數設定：**
```css
/* src/index.css */
:root {
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #212121;
  --text-secondary: #757575;
  --border-color: #e0e0e0;
}

.dark {
  --bg-primary: #1a1a1a;
  --bg-secondary: #2d2d2d;
  --text-primary: #ffffff;
  --text-secondary: #b0b0b0;
  --border-color: #404040;
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  transition: background-color 0.3s, color 0.3s;
}
```

**練習任務：**
- [ ] 實作 ThemeContext
- [ ] 建立主題切換按鈕
- [ ] 為所有元件套用 CSS 變數
- [ ] 測試重新整理頁面後主題是否保留
- [ ] 進階：支援「跟隨系統」選項

---

## 📋 第四階段：進階功能

### 學習重點
- `useEffect` Hook
- API 串接（`fetch`）
- 非同步資料處理
- 載入狀態與錯誤處理
- CSS 動畫

---

### 4.1 動態載入 GitHub 專案

**學習目標：**
- 使用 `useEffect` 發送 API 請求
- 處理載入、成功、錯誤三種狀態
- 串接真實 API

**實作內容：**
```tsx
// src/components/GitHubProjects.tsx
import { useState, useEffect } from 'react'

interface GitHubRepo {
  id: number
  name: string
  description: string
  html_url: string
  stargazers_count: number
  language: string
  updated_at: string
}

export function GitHubProjects({ username }: { username: string }) {
  const [repos, setRepos] = useState<GitHubRepo[]>([])
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<string | null>(null)

  useEffect(() => {
    const fetchRepos = async () => {
      try {
        setLoading(true)
        setError(null)

        const response = await fetch(
          `https://api.github.com/users/${username}/repos?sort=updated&per_page=6`
        )

        if (!response.ok) {
          throw new Error('無法取得專案資料')
        }

        const data = await response.json()
        setRepos(data)
      } catch (err) {
        setError(err instanceof Error ? err.message : '發生錯誤')
      } finally {
        setLoading(false)
      }
    }

    fetchRepos()
  }, [username]) // 當 username 改變時重新請求

  if (loading) {
    return (
      <div className="loading">
        <div className="spinner"></div>
        <p>載入中...</p>
      </div>
    )
  }

  if (error) {
    return (
      <div className="error">
        <p>❌ {error}</p>
        <button onClick={() => window.location.reload()}>重新載入</button>
      </div>
    )
  }

  return (
    <section className="github-projects">
      <h2>GitHub 專案</h2>
      <div className="repos-grid">
        {repos.map(repo => (
          <a
            key={repo.id}
            href={repo.html_url}
            target="_blank"
            className="repo-card"
          >
            <h3>{repo.name}</h3>
            <p>{repo.description || '無描述'}</p>
            <div className="repo-meta">
              {repo.language && (
                <span className="language">{repo.language}</span>
              )}
              <span className="stars">⭐ {repo.stargazers_count}</span>
              <span className="updated">
                更新於 {new Date(repo.updated_at).toLocaleDateString('zh-TW')}
              </span>
            </div>
          </a>
        ))}
      </div>
    </section>
  )
}
```

**Loading Spinner CSS：**
```css
.loading {
  text-align: center;
  padding: 3rem;
}

.spinner {
  width: 50px;
  height: 50px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #667eea;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
```

**進階：快取機制**
```tsx
// 簡單的記憶體快取
const cache = new Map<string, { data: GitHubRepo[], timestamp: number }>()
const CACHE_DURATION = 5 * 60 * 1000 // 5 分鐘

useEffect(() => {
  const fetchRepos = async () => {
    const cached = cache.get(username)
    const now = Date.now()

    // 如果快取存在且未過期
    if (cached && (now - cached.timestamp) < CACHE_DURATION) {
      setRepos(cached.data)
      setLoading(false)
      return
    }

    // ...原本的 fetch 邏輯
    // 成功後儲存快取
    cache.set(username, { data, timestamp: now })
  }

  fetchRepos()
}, [username])
```

**練習任務：**
- [ ] 實作基本的 GitHub API 串接
- [ ] 處理載入與錯誤狀態
- [ ] 加入快取機制
- [ ] 嘗試使用 `useSWR` 或 `React Query` 函式庫

---

### 4.2 滾動淡入動畫（Intersection Observer）

**學習目標：**
- 使用 Intersection Observer API
- 自定義 Hook（`useInView`）
- 效能優化

**實作內容：**
```tsx
// src/hooks/useInView.ts
import { useEffect, useRef, useState } from 'react'

interface UseInViewOptions {
  threshold?: number
  triggerOnce?: boolean
}

export function useInView({ threshold = 0.1, triggerOnce = true }: UseInViewOptions = {}) {
  const [isInView, setIsInView] = useState(false)
  const ref = useRef<HTMLDivElement>(null)

  useEffect(() => {
    const element = ref.current
    if (!element) return

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setIsInView(true)
          if (triggerOnce) {
            observer.unobserve(element)
          }
        } else if (!triggerOnce) {
          setIsInView(false)
        }
      },
      { threshold }
    )

    observer.observe(element)

    return () => {
      observer.disconnect()
    }
  }, [threshold, triggerOnce])

  return { ref, isInView }
}
```

**使用範例：**
```tsx
// src/components/FadeInSection.tsx
import { ReactNode } from 'react'
import { useInView } from '../hooks/useInView'
import './FadeInSection.css'

interface FadeInSectionProps {
  children: ReactNode
  delay?: number
}

export function FadeInSection({ children, delay = 0 }: FadeInSectionProps) {
  const { ref, isInView } = useInView({ threshold: 0.2 })

  return (
    <div
      ref={ref}
      className={`fade-in-section ${isInView ? 'visible' : ''}`}
      style={{ transitionDelay: `${delay}ms` }}
    >
      {children}
    </div>
  )
}
```

**CSS 動畫：**
```css
/* src/components/FadeInSection.css */
.fade-in-section {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.fade-in-section.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**在元件中使用：**
```tsx
<FadeInSection>
  <About title="關於我" description="..." />
</FadeInSection>

<FadeInSection delay={100}>
  <Skills skills={mySkills} />
</FadeInSection>

<FadeInSection delay={200}>
  <Experience experience={experiences[0]} />
</FadeInSection>
```

**練習任務：**
- [ ] 建立 `useInView` Hook
- [ ] 為各個區塊加入淡入效果
- [ ] 嘗試不同的動畫效果（滑入、縮放）
- [ ] 加入延遲時間讓動畫依序出現

---

### 4.3 響應式設計

**學習目標：**
- 使用 CSS Media Queries
- Flexbox 與 Grid 佈局
- 移動優先設計（Mobile First）

**實作內容：**
```css
/* src/App.css */

/* 手機版（預設）- Mobile First */
.app {
  max-width: 100%;
  padding: 1rem;
}

.header {
  padding: 2rem 1rem;
}

.header h1 {
  font-size: 1.75rem;
}

.contact-links {
  flex-direction: column;
  gap: 0.75rem;
}

.skills-list,
.repos-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

/* 平板版（768px 以上）*/
@media (min-width: 768px) {
  .app {
    padding: 1.5rem;
  }

  .header h1 {
    font-size: 2.25rem;
  }

  .contact-links {
    flex-direction: row;
    gap: 1.5rem;
  }

  .skills-list,
  .repos-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* 桌機版（1024px 以上）*/
@media (min-width: 1024px) {
  .app {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }

  .header h1 {
    font-size: 2.5rem;
  }

  .repos-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .two-column-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
  }
}

/* 大螢幕（1440px 以上）*/
@media (min-width: 1440px) {
  .app {
    max-width: 1400px;
  }
}
```

**自定義 Hook 偵測螢幕大小：**
```tsx
// src/hooks/useMediaQuery.ts
import { useState, useEffect } from 'react'

export function useMediaQuery(query: string): boolean {
  const [matches, setMatches] = useState(() => {
    return window.matchMedia(query).matches
  })

  useEffect(() => {
    const mediaQuery = window.matchMedia(query)

    const handleChange = (e: MediaQueryListEvent) => {
      setMatches(e.matches)
    }

    mediaQuery.addEventListener('change', handleChange)
    return () => mediaQuery.removeEventListener('change', handleChange)
  }, [query])

  return matches
}

// 使用範例
function MyComponent() {
  const isMobile = useMediaQuery('(max-width: 767px)')
  const isTablet = useMediaQuery('(min-width: 768px) and (max-width: 1023px)')
  const isDesktop = useMediaQuery('(min-width: 1024px)')

  return (
    <div>
      {isMobile && <MobileMenu />}
      {isDesktop && <DesktopNav />}
    </div>
  )
}
```

**練習任務：**
- [ ] 為所有元件加入響應式設計
- [ ] 測試不同螢幕尺寸（Chrome DevTools）
- [ ] 實作 `useMediaQuery` Hook
- [ ] 確保觸控裝置的按鈕尺寸夠大（最小 44x44px）

---

## 📋 第五階段：測試與部署

### 學習重點
- 單元測試（Vitest + React Testing Library）
- 元件測試
- 建置與部署流程

---

### 5.1 單元測試

**學習目標：**
- 為元件撰寫測試
- 測試使用者互動
- 測試非同步行為

**實作內容：**
```tsx
// src/components/Skills.test.tsx
import { render, screen } from '@testing-library/react'
import { describe, it, expect } from 'vitest'
import { Skills } from './Skills'

describe('Skills 元件', () => {
  const mockSkills = [
    { id: 1, name: 'React', level: 'advanced' as const },
    { id: 2, name: 'TypeScript', level: 'intermediate' as const },
  ]

  it('應該渲染所有技能', () => {
    render(<Skills skills={mockSkills} />)

    expect(screen.getByText('React')).toBeInTheDocument()
    expect(screen.getByText('TypeScript')).toBeInTheDocument()
  })

  it('應該顯示正確的技能數量', () => {
    render(<Skills skills={mockSkills} />)

    const skillTags = screen.getAllByRole('generic', {
      name: /skill-tag/
    })
    expect(skillTags).toHaveLength(2)
  })

  it('應該根據等級套用正確的 CSS class', () => {
    const { container } = render(<Skills skills={mockSkills} />)

    const advancedSkill = container.querySelector('.skill-advanced')
    const intermediateSkill = container.querySelector('.skill-intermediate')

    expect(advancedSkill).toBeInTheDocument()
    expect(intermediateSkill).toBeInTheDocument()
  })
})
```

**測試互動行為：**
```tsx
// src/components/Experience.test.tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect } from 'vitest'
import { Experience } from './Experience'

describe('Experience 元件', () => {
  const mockExperience = {
    id: 1,
    company: 'ABC 科技',
    position: '前端工程師',
    period: '2023/01 - 至今',
    description: '負責前端開發',
    highlights: ['使用 React', '導入測試']
  }

  it('預設應該收合詳細資訊', () => {
    render(<Experience experience={mockExperience} />)

    expect(screen.getByText('前端工程師')).toBeInTheDocument()
    expect(screen.queryByText('負責前端開發')).not.toBeInTheDocument()
  })

  it('點擊後應該展開詳細資訊', async () => {
    const user = userEvent.setup()
    render(<Experience experience={mockExperience} />)

    const toggleButton = screen.getByText('▼ 展開')
    await user.click(toggleButton)

    expect(screen.getByText('負責前端開發')).toBeInTheDocument()
    expect(screen.getByText('使用 React')).toBeInTheDocument()
  })

  it('再次點擊應該收合', async () => {
    const user = userEvent.setup()
    render(<Experience experience={mockExperience} />)

    const toggleButton = screen.getByText('▼ 展開')
    await user.click(toggleButton)
    await user.click(screen.getByText('▲ 收合'))

    expect(screen.queryByText('負責前端開發')).not.toBeInTheDocument()
  })
})
```

**測試表單驗證：**
```tsx
// src/components/ContactForm.test.tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect } from 'vitest'
import { ContactForm } from './ContactForm'

describe('ContactForm 元件', () => {
  it('空白送出應顯示錯誤訊息', async () => {
    const user = userEvent.setup()
    render(<ContactForm />)

    const submitButton = screen.getByRole('button', { name: '送出' })
    await user.click(submitButton)

    expect(screen.getByText('請輸入姓名')).toBeInTheDocument()
    expect(screen.getByText('請輸入 Email')).toBeInTheDocument()
    expect(screen.getByText('請輸入訊息')).toBeInTheDocument()
  })

  it('Email 格式不正確應顯示錯誤', async () => {
    const user = userEvent.setup()
    render(<ContactForm />)

    const emailInput = screen.getByLabelText(/Email/)
    await user.type(emailInput, 'invalid-email')

    const submitButton = screen.getByRole('button', { name: '送出' })
    await user.click(submitButton)

    expect(screen.getByText('Email 格式不正確')).toBeInTheDocument()
  })

  it('正確填寫應成功送出', async () => {
    const user = userEvent.setup()
    render(<ContactForm />)

    await user.type(screen.getByLabelText(/姓名/), '測試使用者')
    await user.type(screen.getByLabelText(/Email/), 'test@example.com')
    await user.type(screen.getByLabelText(/訊息/), '這是測試訊息內容')

    await user.click(screen.getByRole('button', { name: '送出' }))

    // 等待成功訊息出現
    expect(await screen.findByText(/感謝您的訊息/)).toBeInTheDocument()
  })
})
```

**測試非同步 API：**
```tsx
// src/components/GitHubProjects.test.tsx
import { render, screen, waitFor } from '@testing-library/react'
import { describe, it, expect, vi } from 'vitest'
import { GitHubProjects } from './GitHubProjects'

// Mock fetch
global.fetch = vi.fn()

describe('GitHubProjects 元件', () => {
  it('應該顯示載入狀態', () => {
    ;(fetch as any).mockImplementation(() => new Promise(() => {}))

    render(<GitHubProjects username="testuser" />)
    expect(screen.getByText('載入中...')).toBeInTheDocument()
  })

  it('成功取得資料後應顯示專案', async () => {
    const mockRepos = [
      {
        id: 1,
        name: 'test-repo',
        description: '測試專案',
        html_url: 'https://github.com/test/repo',
        stargazers_count: 10,
        language: 'TypeScript',
        updated_at: '2024-01-01'
      }
    ]

    ;(fetch as any).mockResolvedValueOnce({
      ok: true,
      json: async () => mockRepos
    })

    render(<GitHubProjects username="testuser" />)

    await waitFor(() => {
      expect(screen.getByText('test-repo')).toBeInTheDocument()
      expect(screen.getByText('測試專案')).toBeInTheDocument()
    })
  })

  it('API 失敗應顯示錯誤訊息', async () => {
    ;(fetch as any).mockRejectedValueOnce(new Error('API Error'))

    render(<GitHubProjects username="testuser" />)

    await waitFor(() => {
      expect(screen.getByText(/發生錯誤/)).toBeInTheDocument()
    })
  })
})
```

**執行測試：**
```bash
# 單次執行
npm run test

# Watch 模式（檔案變更自動重跑）
npm run test -- --watch

# 產生覆蓋率報告
npm run test:coverage
```

**練習任務：**
- [ ] 為每個元件撰寫至少 3 個測試
- [ ] 測試覆蓋率達到 70% 以上
- [ ] 測試所有使用者互動
- [ ] 測試錯誤處理邏輯

---

### 5.2 部署到 GitHub Pages

**學習目標：**
- 建置生產版本
- 設定部署流程
- 使用 GitHub Actions 自動部署

**步驟一：設定 Vite 基礎路徑**
```ts
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/ReactPractice/', // 替換成你的 repo 名稱
})
```

**步驟二：安裝 gh-pages**
```bash
npm install -D gh-pages
```

**步驟三：新增部署指令**
```json
// package.json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "deploy": "npm run build && gh-pages -d dist"
  }
}
```

**步驟四：手動部署**
```bash
npm run deploy
```

**步驟五：設定 GitHub Actions 自動部署**
```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm run test

      - name: Build
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v3
```

**練習任務：**
- [ ] 本地測試建置流程（`npm run build`）
- [ ] 預覽建置結果（`npm run preview`）
- [ ] 部署到 GitHub Pages
- [ ] 設定自動部署流程
- [ ] 測試部署後的網站是否正常運作

---

### 5.3 部署到 Vercel（替代方案）

**優點：**
- 零設定，自動偵測 Vite
- 每次 commit 自動部署
- 提供預覽網址
- 免費 HTTPS

**步驟：**
1. 前往 [vercel.com](https://vercel.com)
2. 使用 GitHub 登入
3. Import 你的 Repository
4. 點擊 Deploy

**設定環境變數（如果需要）：**
```bash
# Vercel Dashboard → Settings → Environment Variables
VITE_API_URL=https://api.example.com
VITE_GITHUB_TOKEN=your_token_here
```

---

## 🎯 完整專案檢查清單

### 功能完整性
- [ ] Header（個人資訊）
- [ ] About（關於我）
- [ ] Skills（技能列表 + 篩選）
- [ ] Experience（工作經歷）
- [ ] Projects（專案作品集）
- [ ] GitHub Projects（串接 API）
- [ ] Contact Form（聯絡表單）
- [ ] Theme Toggle（深色模式）
- [ ] Scroll Animations（滾動動畫）

### 程式碼品質
- [ ] 所有元件都有 TypeScript 型別定義
- [ ] ESLint 無錯誤
- [ ] 測試覆蓋率 > 70%
- [ ] 無 console.log（生產環境）
- [ ] 遵循 Git 提交訊息規範

### 使用者體驗
- [ ] 響應式設計（手機/平板/桌機）
- [ ] 載入狀態處理
- [ ] 錯誤訊息友善
- [ ] 動畫流暢（60fps）
- [ ] 無障礙設計（ARIA 標籤）

### 效能優化
- [ ] 圖片壓縮與延遲載入
- [ ] Code Splitting（按需載入）
- [ ] CSS 最佳化
- [ ] Lighthouse 分數 > 90

### 部署
- [ ] 成功部署到線上
- [ ] HTTPS 憑證正常
- [ ] 所有功能正常運作
- [ ] 分享連結給朋友測試

---

## 📚 延伸學習資源

### 官方文件
- [React 官方文件](https://react.dev)
- [TypeScript 官方文件](https://www.typescriptlang.org)
- [Vite 官方文件](https://vitejs.dev)

### 測試
- [Vitest 文件](https://vitest.dev)
- [React Testing Library 文件](https://testing-library.com/react)

### 進階主題
- React Router（多頁面路由）
- Zustand / Redux（狀態管理）
- React Query（資料獲取）
- Framer Motion（進階動畫）
- Tailwind CSS（工具類 CSS）

---

## 🤔 常見問題

### Q: 我應該按照順序完成每個階段嗎？
A: 建議按順序進行，因為後面的階段會用到前面學到的概念。但如果你已經熟悉某些主題，可以跳過。

### Q: 每個階段大概需要多久？
A: 視個人學習速度而定，建議每個階段花 1-3 天時間，包含練習與除錯。

### Q: 遇到錯誤該怎麼辦？
A:
1. 閱讀錯誤訊息（React 錯誤訊息很詳細）
2. 檢查瀏覽器 Console
3. 使用 React DevTools 除錯
4. 搜尋 Stack Overflow

### Q: 需要學會所有 CSS 才能開始嗎？
A: 不需要。建議先學會 Flexbox 和基本的 Box Model，其他可以邊做邊學。

### Q: 應該使用 CSS 框架（如 Tailwind）嗎？
A: 第一次建議先用原生 CSS，了解底層原理後再學框架會更容易。

---

## 🎉 完成後的下一步

恭喜完成履歷網站！接下來你可以：

1. **加入更多功能**
   - 部落格文章（Markdown 渲染）
   - 多語系支援（i18n）
   - 訪客計數器
   - 留言板

2. **優化使用者體驗**
   - 加入 Loading Skeleton
   - 圖片懶加載
   - PWA（離線瀏覽）

3. **學習新技術**
   - Next.js（SSR/SSG）
   - React Native（手機 App）
   - GraphQL

4. **分享你的作品**
   - 寫部落格記錄學習過程
   - 在 GitHub 加上完整 README
   - 投稿到技術社群

---

**祝學習順利！有任何問題都可以隨時提問。** 🚀
