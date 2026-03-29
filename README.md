# 🔬 回單鑑真系統 · Receipt Forensics

> AI 驅動的回單真偽分析系統，基於 Claude AI 進行六項鑑真稽核

## ✨ 功能特點

- **六項鑑真分析**
  1. 色塊 / 拼貼 / 重製痕跡偵測
  2. 文字數字排版間距對齊檢查
  3. 回單字體像素比對
  4. 回單字體一致性偵測
  5. 圖片屬性黑名單掃描（PHOTOSHOP / Adobe / Meitu / XINGTU / PICSART 等）
  6. 日期時間吻合度驗證

- **格栅輔助功能** — 一鍵開啟方格輔助線，方便肉眼對比金額是否在同一水平線
- **風險評分** — 0~100 分的鑑真指數，直觀判斷回單真偽
- **拖曳上傳** — 支援拖曳或點擊上傳，支援 JPG/PNG/WEBP/BMP/HEIC
- **圖片元數據** — 顯示檔案大小、解析度、格式、修改時間等資訊

---

## 🚀 部署至 Vercel（GitHub 一鍵部署）

### 方法一：透過 Vercel CLI（推薦）

```bash
# 1. 安裝 Vercel CLI
npm i -g vercel

# 2. 登入 Vercel
vercel login

# 3. 在專案資料夾部署
vercel --prod
```

### 方法二：GitHub + Vercel Dashboard

1. **上傳至 GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Receipt Forensics"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/receipt-forensics.git
   git push -u origin main
   ```

2. **連接 Vercel**
   - 前往 [vercel.com](https://vercel.com)
   - 點擊 "New Project"
   - 選擇你的 GitHub repo
   - 點擊 "Deploy" — 完成！

---

## ⚙️ API Key 設定

本系統使用 Anthropic Claude API 進行圖像分析。

### 取得 API Key
1. 前往 [console.anthropic.com](https://console.anthropic.com)
2. 建立帳號並取得 API Key

### 注意事項
> ⚠️ **重要安全提示**  
> 本專案為純前端靜態部署，API Key 直接在客戶端使用。  
> 建議僅限內部或私人環境使用。  
> 若需要公開部署，請改用 Vercel Serverless Function 保護 API Key。

### 設定 API Key（純前端版本）
在 `index.html` 中，找到 API 呼叫區塊，直接替換為你的 Key（僅限內部使用）：
```javascript
headers: { 
  'Content-Type': 'application/json',
  'x-api-key': 'YOUR_ANTHROPIC_API_KEY',
  'anthropic-version': '2023-06-01',
  'anthropic-dangerous-direct-browser-access': 'true'
}
```

---

## 🔒 進階：使用 Vercel Serverless Function 保護 API Key

建立 `api/analyze.js`：

```javascript
export default async function handler(req, res) {
  if (req.method !== 'POST') return res.status(405).end();
  
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01',
    },
    body: JSON.stringify(req.body)
  });
  
  const data = await response.json();
  res.json(data);
}
```

在 Vercel Dashboard → Settings → Environment Variables 設定：
```
ANTHROPIC_API_KEY = sk-ant-xxxxx
```

---

## 📁 檔案結構

```
receipt-forensics/
├── index.html       # 主要應用程式
├── vercel.json      # Vercel 部署設定
└── README.md        # 說明文件
```

---

## 🏗️ 技術架構

- **Frontend**: 純 HTML5 + CSS3 + Vanilla JS（零依賴）
- **AI Engine**: Anthropic Claude claude-sonnet-4-20250514（Vision）
- **Hosting**: Vercel（Static Deployment）
- **版本控制**: GitHub

---

## ⚠️ 免責聲明

本系統僅供輔助分析使用，結果僅供參考，不構成法律依據。最終判斷應由專業人員綜合評估。
