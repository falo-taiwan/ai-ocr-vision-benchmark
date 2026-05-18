# 九宮格 AI Vision Benchmark

**v0.06 Student Vibe Coding Edition**
作者：Falo X Force Cheng　日期：2026/5/18

---

## 這個專案是什麼

這是一個給 AI 教學使用的純前端範例。它**不是**遊戲，也不是普通九宮格 demo，
而是一個「**可控式 Vision / OCR Benchmark 小型實驗場**」。

它用一個單一 HTML 檔，示範 AI / OCR / Vision / Computer Use 應用落地時，
必須一起設計的 11 件事：

1. 可控資料生成
2. Ground Truth
3. 畫面干擾
4. 畫面截取 / ROI
5. 非 AI pixel 分析
6. 多演算法比較
7. 綜合加權決策
8. 掃描紀錄
9. JSON / CSV 匯出
10. AI 回饋校正
11. 教學文件化

---

## Student Vibe Coding Edition 是什麼

這個版本不是把工具重寫一次，而是把它「**整理成可以丟給 AI 看的形狀**」。

學生通常不是工程背景，不一定會 clone repo、不一定能讀懂完整前端架構。
v0.06 的目標是讓學生可以：

- 直接打開 `index.html` 就能執行（不需要任何安裝）
- 把整份 HTML 丟給 Gemini / Claude / ChatGPT 閱讀
- 請 AI 解釋程式架構與資料流
- 請 AI 模仿某一段功能、新增一個小功能
- 透過這份範例，理解 AI 應用「落地」是怎麼一回事 — 而不是只看 UI

---

## 怎麼開始

### 給學生（最快路徑）

1. 用瀏覽器打開 `index.html`
2. 按「**產生一次**」→ 看九宮格出現
3. 按「**掃描一次**」→ 看下方表格寫進第一筆紀錄
4. 切換演算法、加入干擾、再掃描幾次
5. 切到「**分析圖表**」看哪種演算法在哪種干擾下會壞掉
6. 按「**匯出 JSON**」存下整份紀錄
7. 把 `index.html`（或匯出的 JSON）整份貼給 Gemini，請它解釋給你聽

### 給老師

整個專案就是一個 `index.html`，沒有 `npm install`、沒有後端、沒有 API key、
沒有 CDN 依賴。可以直接：

- 用瀏覽器打開
- 推到 GitHub Pages
- 用 USB 拷給學生
- 丟進 Google Drive、Notion、任何地方

---

## 設計重點（給之後想改的人看）

### 單一演算法 = 綜合加權的特例

| 演算法 | 對應權重 |
|---|---|
| 中央區域平均 | `centerAverage = 100 / 0 / 0` |
| 整格平均 RGB | `fullAverage = 0 / 100 / 0` |
| 主要顏色投票 | `paletteVote = 0 / 0 / 100` |
| 綜合加權 | 預設 `33 / 33 / 34`，可調整 |

這個設計讓學生理解：「單一演算法不是另一個系統，而是綜合決策框架的特例。」

### Ground Truth 不是天生存在

干擾分成三類：

- 純干擾（雜訊、霧、遮罩）→ 答案還是「底圖原本選的顏色」
- 壓力測試（多數遮罩 / 少數遮罩）→ 答案還是底圖（測演算法穩定度）
- 雙色比例 → 答案改為「面積占比較多的顏色」

> Ground Truth 不是天生存在，而是由任務定義決定。

### 為什麼每個操作都要有熱鍵

| 鍵 | 作用 |
|---|---|
| `S` | Start（啟動連續流程） |
| `E` | End（停止連續流程） |
| `G` | Generate（產生一次） |
| `C` | Capture（掃描一次） |
| `T` | Theme（切換版面配色） |
| `+ / -` | 整體 UI 縮放 |
| `A` | 切到分析圖表 |
| `H` 或 `?` | 切到專案說明 |

熱鍵存在的原因不是炫技 — 是因為 Computer Use / AI Agent 要自動化測試這個工具時，
靠鍵盤指令比靠座標點擊穩定太多。

---

## 程式結構

整份 `index.html` 大約 2900 行，全部用 `/* === SECTION:XXX === */` 標記分區：

| Section | 內容 |
|---|---|
| `STATE` | 全域變數、DOM 參照、調色盤、儲存 key |
| `INTERFERENCE` | 干擾預設總表（base / noise / fog / veil / patch...） |
| `STORAGE` | localStorage 存取、清除、版本欄位 |
| `TRUTH` | Ground Truth 產生 |
| `RENDER` | Canvas 繪製、ROI 虛線、干擾覆蓋層 |
| `DETECT` | pixel → 顏色（非 AI 顏色辨識） |
| `ANALYZE` | 單格分析 + 綜合加權 |
| `SCAN` | 掃描整個九宮格、寫入紀錄 |
| `LOGS` | 三種紀錄表格渲染 |
| `CHARTS` | 分析圖表 |
| `UI` | 演算法切換、主題、UI 縮放 |
| `HOTKEYS` | 鍵盤熱鍵 |
| `TIMERS` | 自動產生 / 自動掃描 |
| `EXPORT` | CSV / JSON 匯出 |
| `INIT` | 啟動流程 |

要找某一區，直接在編輯器或 AI 對話裡搜尋 `SECTION:` 就會跳到目錄。

每個 SECTION 開頭都有一段中文註解，告訴你：
- 這一區在做什麼
- 為什麼這樣設計
- 想改什麼要動哪裡
- 哪些地方不要亂改

---

## 給 AI 工具的閱讀順序建議

1. 讀 HTML 開頭那一大段 `AI Reading Guide` 註解（在 `<script>` 之後）
2. 跳到 `SECTION:STATE`，看資料長什麼樣子
3. 跳到 `SECTION:TRUTH`，看答案是怎麼被定義出來的
4. 跳到 `SECTION:DETECT`，看 pixel 怎麼變成顏色
5. 跳到 `SECTION:ANALYZE`，看四種演算法為什麼不同
6. 最後看 `SECTION:WEIGHTED`（在 ANALYZE 內），理解「單一演算法 = 綜合的特例」

---

## 給 AI 的常用 Prompt 範例

```
請用國中生也聽得懂的話，解釋這份 index.html 的資料流。
請列出每個 SECTION（/* === SECTION:XXX === */ 那些）各自在做什麼。
請幫我新增一種叫 rainbowStripe 的干擾，並說明你改了哪一段、為什麼。
請幫我把 palette 換成柔和粉色系，但保持 9 種顏色都區分得開。
請幫我新增一個分析圖表，顯示每個格子位置（row,col）的平均準確率。
請幫我把 Mode 2 數字九宮格做一個最小可運作的雛形。
我把匯出的 JSON 貼給你，請告訴我這次測試中哪個演算法最常把藍色判成紫色。
```

更詳細的 prompt 與練習，請看 [`student-vibe-coding-guide.md`](student-vibe-coding-guide.md)。

---

## 版本

`v0.06 Student Vibe Coding Edition` — Falo X Force Cheng 2026/5/18

從 v0.05 改了什麼：
- 在 `<script>` 開頭加入 AI Reading Guide 註解區塊（給 AI 工具的閱讀地圖）
- 全份 script 加上 `/* === SECTION:XXX === */` 分區標記，每區附中文教學註解
- 在「專案說明」頁面新增 Student Vibe Coding 區塊：8 步驟、Prompt 範例、練習任務
- 新增 `README.md` 與 `student-vibe-coding-guide.md`
- 標題、meta 描述、頁面版本字串、JSON 匯出的 version 欄位全部更新為 v0.06
- **沒有**改動演算法邏輯、資料結構、log 欄位、CSS。學生看到的功能完全一樣

