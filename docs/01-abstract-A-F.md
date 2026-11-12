# 01 摘要 A–F 與「5 核心、3 取捨、10 細節」

> 來源：已繳交的 Proposal Document。數據引用編號見 `references.md`。尚未實驗，F 為 Expected Outcomes。

## A–F 摘要

**A. Attention Getter（動機）**
印尼東加里曼丹約 41% 成年人的總液體攝取（TFI）不足，高於全國的 28%，且「忘記喝水」被認為是最主要障礙 [4]；臺灣國健署 2026 年公布的新版每日飲食指南草案首度將水分攝取納入正式章節 [12]。提醒大家多喝水已是公共衛生議題，但如何讓提醒貼近個人生活仍未解決。

**B. But（挑戰與 Research Gap）**
(L1) 既有提醒多依固定時間或靜態個資，且只比較提醒「開／關」[4]；(L2) 重量式智慧水杯雖能自動記錄，但 1,747 筆原始紀錄僅約 51.7%（904 筆）為有效飲水事件，且不含運動、飲食情境與提醒回饋 [6]；(L3) LLM 於 JITAI 的研究顯示系統能執行不代表提醒決策適當 [5, 7]；(L4) 攝取量不等於水合狀態 [1, 4]。缺口：真實重量感測 + ML 事件辨識 + 情境感知 Agent 決策 + 回饋閉環，且未與固定提醒對照。

**C. Cure（解法）**
為解決上述問題，本專題提出 **Smart Hydration Monitoring Agent**，一個結合重量感測、機器學習與情境感知 AI Agent 的 Domain AI Agent，而非單純提醒 App 或 Chatbot。

**D. Development（方法）**
Raspberry Pi 4 + Load Cell + HX711 蒐集杯重時序；Task A（Random Forest／XGBoost）辨識 Drinking / Refilling / CupMovement / Other，Task B 預測未來 60 分鐘飲水機率；Agent 以 Tool Calling 整合紀錄、預測、使用者登記的情境與回饋，在規則護欄（冷卻、安靜時段、每日上限、目標已達、暫停）內決定是否提醒；LLM 只生成通知文字；經 LINE Bot 推播並收集回覆；Web Dashboard 負責視覺化與情境登記。

**E. Experiments（評估，Planned）**
自建資料集，人工確認 Ground Truth，時間切分。實驗 1：Threshold vs. ML 事件辨識（Precision、Recall、F1、Confusion Matrix、volume MAE）；實驗 2：行為預測 vs. Fixed Interval／Historical Average（探索性）；實驗 3：固定間隔／ML-based／ML + Agent 三種提醒策略（接受率、無效提醒比例、滿意度、延遲、紀錄完整度）。受測者限三位組員與少數同學，僅做描述性比較。

**F. Findings（Expected Outcomes，尚無實驗結果）**
預期產出：低成本可重現的原型、重量時序飲水事件資料集、Threshold 與 ML 的比較，以及情境感知 Agent 對提醒適當性的初步評估。系統只提供行為提醒，不做醫療診斷或脫水判定。

## 5 核心、3 取捨、10 細節

> 對應報告的 Deep Research（Q1 核心思維、Q2 專業爭論、Q3 深度理解問題）。

### 5 個核心（Q1 核心思維模式）

| # | 核心 | 對設計的影響 | Spec |
|---|---|---|---|
| 1 | 攝取量 ≠ 水合狀態 | 只報告杯中飲水與行為，不推論脫水 | `safety-and-privacy-boundaries` |
| 2 | 需求因人與情境而異 | 目標由使用者設定；加入運動／飲食／咖啡因特徵 | `hydration-dashboard`、`drinking-behavior-prediction` |
| 3 | 先切事件、再估量 | Task A 先分類，僅 Drinking 計量 | `drinking-event-detection` |
| 4 | 提醒要在對的時機且不造成負擔 | 冷卻、每日上限、安靜時段、可暫停 | `reminder-decision-agent` |
| 5 | 人定義界線、決策可稽核、資料最小化 | 護欄不交給 LLM；每次決策寫紀錄；LLM 輸入去識別化 | `reminder-decision-agent`、`safety-and-privacy-boundaries` |

### 3 個取捨（Q2 專業爭論）

| # | 取捨 | 我們的選擇 | Spec |
|---|---|---|---|
| T1 | 固定每日目標 ↔ 依個人與情境調整 | 使用者可調目標 + 情境特徵；不宣稱最佳量 | `hydration-dashboard` |
| T2 | 提醒提高攝取 ↔ 是否改善水合與健康 | 評估聚焦提醒行為指標，不宣稱健康成效 | `strategy-comparison-experiment` |
| T3 | LLM 決策 ↔ 規則／ML 決策 | 混合式：ML 預測、規則護欄、LLM 僅文字 | `reminder-decision-agent` |

### 10 個細節（Q3 深度理解問題）

> 每題先列完整問題，再寫目前的設計回答與對應 spec。回答只描述規劃中的設計，不含實驗結果；門檻數值尚未校正，一律為可設定參數。

| # | 類型 | 主要對應 |
|---|---|---|
| 1 | Why | `drinking-event-detection` |
| 2 | What-if | `drinking-event-detection` |
| 3 | Trade-off | `drinking-event-detection`、`drinking-behavior-prediction` |
| 4 | Evidence | `strategy-comparison-experiment` |
| 5 | Evidence | `hydration-dashboard` |
| 6 | Comparison | `strategy-comparison-experiment` |
| 7 | Design Decision | `reminder-decision-agent` |
| 8 | Real-world | `reminder-decision-agent`、`line-feedback-loop` |
| 9 | Failure / Safety | `safety-and-privacy-boundaries` |
| 10 | Privacy | `safety-and-privacy-boundaries` |

**Q1（Why）杯重從 550 g 降到 420 g，為什麼不能直接認定使用者喝了 130 mL？**

重量減少可能來自飲用、倒掉、杯子移位或感測雜訊，而且 130 g ≈ 130 mL 還假設液體密度約 1 g/mL（Illustrative Example）。因此系統先切出「拿起—放回」片段，再分類為 Drinking / Refilling / CupMovement / Other，只有 Drinking 才以 max(0, ΔW)/ρ 計量；被判為 Other 的減重（例如倒掉）不計入當日攝取。→ `drinking-event-detection`

**Q2（What-if）如果使用者直接對瓶口喝、換了杯子，或喝完沒有放回杯墊，系統會怎麼失效？**

這三種情況下杯墊量不到重量變化，或片段無法回到穩定值，事件會漏記，或被歸為 Other。系統不嘗試「猜」這些量，而是在 Dashboard 標示資料缺口，並在限制中明示「杯子未放回或直接對瓶口飲用無法量測」；水密度與「會放回杯墊」都列為未驗證假設。→ `drinking-event-detection`、`DESIGN.md` §12

**Q3（Trade-off）資料量很少時，門檻法與 Random Forest／XGBoost 該如何取捨？為什麼評估要用時間切分？**

門檻法透明、不需訓練資料，但難以區分加水、移動等相似情況；ML 能用斜率、穩定度、片段長度等特徵區分，卻需要人工確認的標註。因此門檻法保留為 Baseline，兩者用同一批標註片段比較 Precision、Recall、F1、Confusion Matrix 與 volume MAE。時間切分（訓練資料一律早於測試資料）是為了避免用「未來」資料預測「過去」造成的洩漏；Task B 在資料不足時改用 Historical Average Rule，且不宣稱已建立可靠的個人化預測。→ `drinking-event-detection`、`drinking-behavior-prediction`

**Q4（Evidence）使用者在提醒後喝了水，怎麼知道是提醒造成的，而不是他本來就會喝？**

單一事件無法證明因果。設計上以策略 A（固定間隔）作為對照，並記錄每則提醒的策略、使用者、時期、順序與回覆；指標採接受率與無效提醒比例等行為指標，且只做描述性比較，不宣稱顯著性或因果效果。→ `strategy-comparison-experiment`

**Q5（Evidence）為什麼不直接用「每天喝 2 L」這種單一規則當目標？**

EFSA 的參考值（女 2.0 L、男 2.5 L）包含食物水分，且只適用溫和環境與中等活動量 [10]；國健署新版指南草案也強調需求依體型、活動量與氣候調整 [12]。因此系統不內建單一標準水量，目標由使用者自行設定，運動與飲食只作為情境特徵。→ `hydration-dashboard`

**Q6（Comparison）三種提醒策略要如何公平比較？順序效應與新奇效應怎麼處理？**

同一批使用者輪流使用 A／B／C 三種策略，系統記錄每段時期的策略與先後順序，以便檢視順序與新奇效應。受測者限三位組員與少數同學，結果附樣本數與限制，僅做描述性比較。實際人數與每個策略的天數列為 Open Question，取決於可招募人數。→ `strategy-comparison-experiment`、`design.md` Open Questions

**Q7（Design Decision）為什麼飲水量計算、事件辨識與提醒判斷不交給 LLM？**

LLM 可能產生不一致或虛構的數值，而且研究顯示「能自動執行」不等於「決策適當」[7]。因此責任切分為：確定性程式負責計算、ML 負責預測、規則護欄負責判斷（LLM 不可覆寫），LLM 只生成通知文字；生成失敗或未通過詞彙檢查時改用模板。每次決策的輸入、護欄結果與輸出都寫入紀錄，可以稽核與重現。→ `reminder-decision-agent`

**Q8（Real-world）使用者連續開會兩小時、根本不能喝水，Agent 還該提醒嗎？**

使用者可設定安靜時段或直接暫停，護欄判斷失敗時 Agent 一律回傳 SKIP 並記錄原因。若仍收到提醒，使用者可回覆「稍後提醒」延長冷卻，連續回覆「暫時不需要」則會依規則抑制該時段的提醒。目前不讀取行事曆，會議時段需由使用者自行設定。→ `reminder-decision-agent`、`line-feedback-loop`

**Q9（Failure / Safety）醫囑限水的人（例如心臟衰竭病人）使用時，如何避免系統不當鼓勵多喝水？**

系統定位為行為提醒，不做診斷、不判斷脫水，也不提供醫療級補水建議。介面提示醫囑限水者應先諮詢醫療專業人員，且可隨時暫停提醒；通知文字經詞彙檢查，不通過即改用模板。運動情境也遵循 ACSM「不宜飲用超過排汗流失量」的原則，不鼓勵過量 [11]。→ `safety-and-privacy-boundaries`

**Q10（Privacy）哪些資料會送到外部 LLM？使用者要如何暫停或刪除資料？**

只傳送生成通知文字所需的最少欄位，送出前去識別化，不含姓名、LINE ID 或原始重量序列。資料存在本地 SQLite，使用者可暫停提醒（暫停期間 Agent 一律 SKIP），也可刪除自己的紀錄；系統不使用相機。→ `safety-and-privacy-boundaries`
