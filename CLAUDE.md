# 南法 17 天行程表 HTML 工具

> 給 Claude 看的專案說明。每次新 session 先讀這份，再讀使用者的需求。

---

## 這是什麼

Vais（江沛儀）2026 年 6 月 29 日 — 7 月 15 日南法自駕旅行的單檔 HTML 工具。
兩人同行（Vais + 朋友）。繁體中文介面。

---

## 檔案說明

| 檔案 | 說明 |
|------|------|
| `index.html` | **永遠是最新版**，固定網址用。每次改完 v3 就 cp 過來覆蓋 |
| `南法行程表_v3.html` | **主力工作檔**，在這裡改東西 |
| `南法行程表_v2.html` | 另帳號做的版本，存檔用，不動 |
| `南法行程表.html` | 最原始 v1，不動 |

**更新流程：** 改 `南法行程表_v3.html` → 確認 OK → 複製成 `index.html` → commit → push

---

## 行程概覽

- **6/29（一）** 台北出發
- **6/30（二）** 抵達尼斯 NCE 09:10（Day 2）
- **7/4（六）** 取租車，開往 Fourques（Day 6，第一天自駕）
- **7/9（四）** 移動到 Aix-en-Provence（Day 11）
- **7/14（二）** 還車於馬賽機場，飛回（Day 16）
- **7/15（三）** 抵台（Day 17）

住宿：Nice 4 晚 → Fourques Airbnb 5 晚（有泳池）→ Aix Le Mas des Ecureuils 5 晚

---

## HTML 工具技術結構

### 關鍵常數
```javascript
const SK = 'sf2026v3'          // localStorage key（v3 專用，不與 v1 衝突）
const TIME_OFFSET = 90          // 全行程時間自動 +1.5hr（Vais 8:30 才起床）
```

### 時間偏移邏輯
- 所有活動時間顯示時自動 +90 分鐘
- 航班、機場出發等固定時間用 `fixed: true` 保護，不偏移，顯示 ✈ 圖示
- `shiftTime(t, fixed)` 函數處理偏移

### DRV 自駕摘要卡
`const DRV = {...}` 以 DAYS 陣列 index（0-based）為 key，包含：
- `km`、`time`、`diff`（1-5 難度）、`fuel`（加油建議）、`warn`（警告）

### 六個 Tab
行程 / 攻略 / 探索 / 地圖 / 清單 / 備忘

### 探索（EX）資料結構
```javascript
{type, name, desc, tip, addr, gmap}
```
地圖連結同時有 Google Maps 和 Waze：
- Google Maps：`https://www.google.com/maps/search/?api=1&query=...`
- Waze：`https://waze.com/ul?q=...&navigate=yes`

### Sync 功能
Base64 編碼同步碼，可讓兩人合併打卡進度

---

## 已知決定 & 注意事項

- **Day 6（7/4）** 是第一天自駕，路線 Nice → Grasse香水城（Fragonard 免費導覽）→ Valensole 薰衣草 → Fourques。主題：香氛日 🌸
- **Day 10（7/8 週三）** 早上阿爾勒週三大市集（Boulevard des Lices，09:00-12:30）
- **Day 7（7/5 週日）** 索格島古董市集，建議例外早起，10:00 前到
- **6/30（7/30 週二）** 抵達 Nice 當天，Cours Saleya 市集有開（週二到週日）。放行李後約 12:00 可趕上尾段
- **Verdon Gorge（聖十字湖）** 已從 Day 6 移除，新手第一天不開山路
- localStorage 存的是勾選狀態、自訂活動、備忘，不是行程本身

---

## 常見修改任務

**加一個探索地點：**
在 `const EX = {...}` 對應城市陣列加一個物件即可。

**改某天行程：**
找 `const DAYS = [...]`，找到對應 day 的物件修改 `a` 陣列（活動列表）。

**改攻略文字：**
找 HTML 裡的 `<!-- GUIDE -->` 區塊，直接改 `<p>` 內容。

**更新完記得：**
`cp 南法行程表_v3.html index.html` → commit → push
