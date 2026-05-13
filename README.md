# 菊島漫遊 · Penghu 4D3N

> 2026 年 6 月 26-29 日 · 兩人 · 4 天 3 夜澎湖之旅 · 手機 App 風格旅遊指揮中心

一份個人化的澎湖旅遊行動指南 — 從機票、住宿、行程、租車、浮潛、美食到緊急聯絡，全部整合在單一頁面，仿 iOS App 操作介面，行動裝置體驗最佳。

🌐 **Live demo**：https://penghu-trip.web.app （Firebase Hosting · 支援 Google 登入跨裝置同步）

---

## ✨ 5 大功能分頁

| 分頁 | 功能說明 |
| --- | --- |
| **儀表板** | 出發倒數、當前 / 下一個活動、**馬公即時天氣 + 4 天預報**、訂購摘要 |
| **行程** | 4 天時間軸；每筆活動可打卡 + **上傳多張照片** + **寫下心得** |
| **攻略** | 已訂機加酒卡 / 4 家電動機車比價 / 浮潛行程 / 必吃美食 TOP 6 |
| **記帳** | TWD 預算追蹤，純本地儲存不上雲 |
| **資訊** | 完整訂購明細、Google 地圖連結、6 組緊急聯絡電話 |

### 🌤 天氣

- 透過 [Open-Meteo](https://open-meteo.com/) API (免費、無需 API Key) 取得馬公座標 (23.57°N, 119.59°E) 的天氣
- 顯示當下溫度 / 體感狀態 / 風速 / 濕度
- 旅程 4 天 (6/26-6/29) 預報會在接近行程日期 (約出發前 7 天) 自動顯示
- 每日只 fetch 一次，結果快取於 `localStorage`

### 📷 景點回憶 (打卡 + 照片 + 心得)

點開行程任一項目展開：
- **打卡** — 標記完成，項目會變綠色
- **照片** — 多張上傳，自動壓縮至 800px JPEG，每張可預覽 / 下載原圖 / 刪除
- **心得** — 寫下文字筆記，可隨時編輯或刪除

### ☁️ 雲端同步（Firebase）

啟用 Google 登入後，**所有資料即時同步到所有裝置**：

- 你和伴侶用各自的 Google 帳號登入
- 任一裝置上傳照片 / 寫心得 / 打卡 / 新增記帳 → 另一裝置立刻看到
- 照片放 Firebase Storage，文字放 Firestore
- 沒網路時操作會自動排隊，恢復連線後同步
- 限制：只有白名單內的 email 才能讀寫（程式碼 + Firestore 安全規則雙重檢查）

> 若想跑沒帳號的本地版（純 `localStorage`），請 checkout 至 `85d525d` commit。

---

## 🎟 我的訂購 (立榮假期 · 易遊網)

| 項目 | 內容 |
| --- | --- |
| 去程 | B7-8617 · 松山 TSA → 澎湖 MZG · 06/26 13:55 → 14:45 |
| 回程 | B7-8610 · 澎湖 MZG → 松山 TSA · 06/29 12:15 → 13:05 |
| 住宿 | 澎湖左轉休閒民宿 (★★★★) · 馬公市西文里新生路 380 號 |
| 房型 | 雙人房一大床 × 1 · 3 晚 · 不含早餐 |
| 總金額 | **TWD 14,219** |

⚠️ 促銷票限當日當班次有效，不可更換航班。

---

## 🗺 4 天 3 夜行程概要

- **Day 1 · 6/26 (五) 抵達** — 觀音亭夕陽 · 中央老街 · 小紅莓海鮮石頭火鍋
- **Day 2 · 6/27 (六) 湖西** — 山水沙灘浮潛 · 奎壁山摩西分海 · 海上皇宮海洋牧場
- **Day 3 · 6/28 (日) 西嶼** — 跨海大橋 · 二崁古厝 · 鯨魚洞 · 漁翁島燈塔 · 福朋喜來登
- **Day 4 · 6/29 (一) 返程** — 篤行十村 · 名產採買 · 12:15 返航松山

詳細時間軸請見「行程」分頁。

---

## 🚀 使用方式

無需建置流程，所有資源透過 CDN 載入。

### 線上 (Firebase Hosting)

正式網址：**https://penghu-trip.web.app**

> 為何不用 GitHub Pages？因為 Google 登入需要網站網域跟 Firebase Auth 處理器
> 同網域，跨網域時瀏覽器會擋第三方 cookie 導致登入後 `currentUser` 是 null。
> Firebase Hosting 的 `*.web.app` 跟 `*.firebaseapp.com` 是同源信任，無此問題。

### 本地

```bash
# macOS 直接開（無雲端同步，僅瀏覽 UI）
open index.html

# 或本地預覽（含 Firebase 連線）
firebase emulators:start --only hosting
# 預設開在 http://localhost:5000
```

### 部署

```bash
firebase deploy --only hosting
```

> 行程打卡、記帳資料以 `localStorage` 儲存於瀏覽器，**不會上傳到任何伺服器**。

---

## 🛠 技術棧

- 純 HTML / CSS / JavaScript，零建置流程
- [Tailwind CSS](https://tailwindcss.com/) (CDN)
- [Font Awesome 6](https://fontawesome.com/) — icon
- Google Fonts — Outfit
- `localStorage` — 行程打卡、記帳紀錄

---

## 📁 專案結構

```
Penghu_trip/
├── index.html       # 主程式：手機 App 指揮中心
├── firebase.json    # Firebase Hosting 設定
├── .firebaserc      # Firebase 專案綁定
├── README.md
└── .gitignore
```

---

🤖 由 [Claude Code](https://claude.com/claude-code) 協助生成
