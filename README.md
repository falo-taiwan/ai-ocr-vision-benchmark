# 九宮格 AI Vision Benchmark

這個專案是一個純 HTML / JavaScript 的可控式 Vision / OCR 教學實驗場。

它不是單純的九宮格小工具，而是用最小畫面示範 AI 應用落地的完整流程：

```text
可控資料生成 -> 畫面干擾 -> 捕捉 ROI -> 多種辨識方法 -> 綜合加權決策 -> 雙紀錄匯出 -> AI 回饋校正
```

## GitHub Pages 入口

- `index.html`
  - GitHub Pages 建議入口。
  - 目前版本：`v0.02`。
  - 已整合主操作頁、橫向工作列、紀錄分頁、專案說明 view、干擾設計說明、AI 協作教學、SEO/GEO meta 與 `Falo X Force Cheng 2026/5/18` 標記。

## 目前檔案

- `mode1-color-grid-benchmark.html`
  - 開發過程中的主操作頁版本。
  - 支援 Mode 1 顏色九宮格、干擾生成、演算法切換、綜合加權、CSV 匯出。

- `docs/interference-ai-collaboration.md`
  - 干擾設計與 AI 協作方式說明。

- `docs/interference-ai-collaboration.html`
  - 可直接瀏覽的教學版說明頁。

正式發布時可以優先使用 `index.html` 作為單檔展示頁。

## Mode 1 核心功能

- 每 N 秒產生九宮格顏色。
- 每 N 秒掃描九宮格。
- 捕捉區域預設為每格中央 80%，以虛線標示。
- 可切換辨識方法：
  - 中央區域平均
  - 整格平均 RGB
  - 主要顏色投票
  - 綜合加權
- 綜合加權預設比例：
  - 中央區域平均：33%
  - 整格平均 RGB：33%
  - 主要顏色投票：34%
- 可匯出：
  - `capture_log`
  - `weighted_decision_log`

## 干擾設計重點

目前干擾分成兩種任務：

1. 底圖還原壓力測試
   - A：遮罩占 ROI 多數，但正確答案仍是底圖。
   - B：遮罩占 ROI 少數，正確答案仍是底圖。

2. 雙色比例測試
   - 每格隨機產生兩種顏色，比例 20% 到 80%。
   - 正確答案是面積占比較多的顏色。

這兩種任務故意分開，因為 AI 落地時「正確答案」不是自然存在，而是由任務定義決定。

## 教學定位

這個 demo 可以用來說明：

- 模擬資料用 AI
- OCR / Vision / Computer Use 的畫面辨識
- 多演算法決策
- 紀錄與 audit trail
- AI 回饋校正

它對應到更大的 AI 生態概念：AI 不只是一個聊天模型，而是一整套資料、模型、工作流、部署、監控與回饋校正系統。
