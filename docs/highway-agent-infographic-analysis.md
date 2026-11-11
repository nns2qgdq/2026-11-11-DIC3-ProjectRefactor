# 可解釋的國道壅塞預測 Agent｜高密度 NUMBERING 資訊圖表內容

> 依據《國道預測 Agent.pdf》三頁分析整理；原文件的符合度判定及初步實驗結果未經獨立查證。

## 1. 專案概覽

| 編號 | 項目 | 內容 |
|---|---|---|
| 1.1 | 研究目標 | 將未來 30–60 分鐘車速預測轉化為可解釋的出發資訊 |
| 1.2 | 資料 | 高公局 M05A 門架資料，國道 5 號主線 10 區間 |
| 1.3 | 方法 | 預測模型作為工具；程式計算數值與規則，LLM 解析意圖及生成說明 |
| 1.4 | 資料切分 | 2024 年訓練、2025 年測試（含 8 個連假）、2026 年樣本外 |
| 1.5 | 評估 | MAE、壅塞偵測率；持續預測、歷史平均、GBM |
| 1.6 | 初步結果 | MAE 低於持續預測，但連假改善較小；預警能力沒有明顯優勢 |

## 2. A–Z 報告架構（20 頁）

| 編號 | 章節 | 重點 |
|---|---|---|
| 2.1 | Abstract（S2） | A–F：動機、挑戰、解法、方法、評估、結果 |
| 2.2 | Introduction（S3–6） | G–L：背景、問題、文獻、判斷、貢獻、組織 |
| 2.3 | Related Work（S7–11） | N–O：研究缺口、R1–R15 八欄文獻摘要表、D1–D6 官方證據 |
| 2.4 | Proposed Scheme（S12–18） | PA–PJ：目標、五模組、流程圖、演算法、公式、對話 |
| 2.5 | Simulation & Conclusion（S19–20） | Q–V：評估；W–Z：貢獻、限制、結論 |

## 3. 研究缺口：Existing → Limitations → Gap → Project

**3.1 Existing**：傳統統計／ML（HA、ARIMA、SVR）、深度時空模型（Chen 2021、SLIT 2024、Informer 2025）、LLM／Agent（ST-LLM、xTP-LLM、TrafficGPT、TP-GPT、IDM-GPT）。

**3.2 Limitations**：許多研究僅輸出數值；LLM 交通應用偏查詢與模擬，數值推理與真實路側預測依據不足。

**3.3 Gap**：欠缺可重現的真實資料評估、可追溯的出發建議與數值防幻覺機制。

**3.4 Project**：M05A 預測工具＋官方交通管制知識庫＋生成結果數字核對。

## 4. 系統架構與流程

### 4.1 五大模組

1. **互動呈現**：輸入問題、顯示建議。
2. **Agent**：意圖解析、資料整合、自然語言回覆。
3. **知識庫**：官方管制措施、規則與情境。
4. **預測引擎**：車速預測、壅塞判斷。
5. **資料擷取與清理**：M05A 門架車速、缺值／無車處理。

### 4.2 三階段流程

1. **輸入守門**：意圖判斷，不合適則拒答或追問。
2. **預測／知識檢索**：取得車速預測與官方規則。
3. **組成與數字核對**：檢查輸出數字；失敗則退回固定模板。

### 4.3 關鍵定義

- 壅塞：`v < 40 km/h`。
- 無資料：`速度 = 0 且交通量 = 0`，不直接視為壅塞。
- 歷史特徵：12 步視窗。
- 主要評估：MAE、壅塞偵測率。

### 4.4 Algorithm 1 摘要（非原文逐字轉錄）

```text
01  intent = parse(user_query)
02  if not valid(intent): return ask_followup()
03  data = load_M05A()
04  X = preprocess(data)
05  v = predict_GBM(X)
06  congest = (v < 40)
07  policy = retrieve_KB(intent)
08  advice = generate(intent, v, policy)
09  if not check_numbers(advice): return template_reply()
10  return advice
```

## 5. 期末作業規範符合度（依來源報告判定）

| 編號 | 指標 | 來源評估 |
|---|---|---|
| 5.1 | Domain AI Agent：AI＋Domain＋Data | 完全符合 |
| 5.2 | Abstract A–F | 完全符合 |
| 5.3 | Introduction 與客觀動機 | 完全符合 |
| 5.4 | Related Work／八欄文獻摘要 | 完全符合 |
| 5.5 | Deep Research：Q1 五思維、Q2 三爭論、Q3 十問題 | 高度符合 |
| 5.6 | 編號系統架構圖 | 完全符合 |
| 5.7 | Flowchart／帶行號 Pseudocode | 完全符合 |
| 5.8 | GitHub／README／DESIGN／可重現性 | 完全符合 |

## 6. 綜合評估與亮點

**6.1 標籤對齊**：20 頁簡報依 A–Z SOP 呈現研究邏輯。

**6.2 防幻覺機制**：LLM 不自行計算；後端核對數值，失敗改用固定模板。

**6.3 科學誠實**：揭露連假改善有限、跨年度不穩定及預警未優於持續預測的限制。

**6.4 定位**：提供有依據、可解釋的行前資訊，不宣稱取代官方 1968 預測。

## 7. 來源與限制

- 來源：使用者提供《國道預測 Agent.pdf》（3 頁）。
- 原專題儲存庫（來源文件提及）：https://github.com/nns2qgdq/T1-highway-congestion-agent
- 來源稱 DESIGN.md 包含 57 項單元測試及 sha256 評估案例凍結，本次未獨立驗證。
- 本檔為 A4 橫式高密度資訊圖表的**文字底稿**，不是原先生成的 PNG 圖檔。
