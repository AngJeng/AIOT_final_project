# 04 Related Work（OpenSpec 重構版）

> 重構原則：約 **1/2 AI Technology + 1/2 Domain Know-how**；每個小節結尾都有「→ 設計含意 → Spec」，讓文獻直接驅動需求。引用編號見 `references.md`。
> 注意：[8] Nahum-Shani 2018 與 [9] Fiedler 2024 的書目資訊與重點取自 [7] 的引用列表，**尚未逐篇讀全文**。

## Part I — AI Technology

### R-AI1 飲水事件偵測與飲水量估計

| 工作 | 做法 | 結果 | 對我們的意義 |
|---|---|---|---|
| Cohen 2021 [1] | 綜述：穿戴式／嵌入表面／視覺環境式／智慧容器 | 多技術融合最有希望 | 重量式屬容器／表面類；可結合情境 |
| Griffith 2019 [2] | 兩階段：sip 偵測 → 量估計（容器 IMU） | 實際資料 MAPE 31.8% | 採「先切事件、再估量」 |
| Roy 2022 [3] | 端點 TinyML 偵測 sip 與加水後首次 sip，自我校正 | sip 偵測約 94–95%；體積誤差 0–28% | 加水（Refilling）需獨立處理；注意隱私 |
| Miftahuddin 2025 [6] | HX711 + 5 kg 荷重元；規則篩有效事件 | 1,747 筆 → 904 筆有效（約 51.7%）；校正最大誤差 0.4 g | 規則篩選是我們的 Baseline；缺 ML 與情境 |

**→ 設計含意**：重量式系統多靠規則篩事件，尚未見以 ML 區分飲水／加水／移動杯子並與門檻法比較的研究。
**→ Spec**：`drinking-event-detection`（含門檻法 Baseline 與 Ground Truth 分離）。

### R-AI2 提醒決策：規則、ML 與 LLM Agent

| 工作 | 做法 | 結果 | 對我們的意義 |
|---|---|---|---|
| Nahum-Shani 2018 [8] | JITAI 框架 | 依當下狀態與情境決定是否、何時、如何介入 | 決策三問題即 Agent 輸出 |
| Fiedler 2024 [9]（經 [7] 引用） | 45 項 JITAI 系統性回顧 | 決策規則以 if-then 為主 | 規則透明但難維護 |
| Haag 2025 [5] | GPT-4 依 persona 與情境產生 JITAI 提案 | 450 筆提案評分高於人類版本 | 僅概念情境、評的是文字，非真實成效 |
| Rezaee 2026 [7] | LLM Agent 每日審視 30 天資料、改提醒、留決策紀錄 | 8 次自動執行成功；每日上限 3 則；**未顯示改善** | 成功執行 ≠ 決策適當；需與固定提醒對照 |

**→ 設計含意**：採混合式——ML 預測、規則護欄、LLM 只生成文字；每次決策留紀錄；以固定間隔作對照。
**→ Spec**：`reminder-decision-agent`、`strategy-comparison-experiment`。

## Part II — Domain Know-how（飲水領域）

### R-D1 需求參考值及其適用條件

EFSA 成人總水分適足量：女 2.0 L/日、男 2.5 L/日，**含食物水分**，僅適用溫和環境與中等活動量（PAL 1.6）[10]。臺灣國健署 2026 新版指南首度納入水分並強調需依個人調整 [12]。
**→ 設計含意**：不內建單一「標準水量」；目標由使用者設定，系統只提供可調整的參考起點。
**→ Spec**：`hydration-dashboard`（目標設定）。

### R-D2 運動補水

ACSM：運動補水目標是避免缺水造成體重損失超過 2% 並避免電解質過度變化；排汗率個體差異大，建議客製化；不宜飲用超過排汗流失量的水 [11]。
**→ 設計含意**：運動類型、時長、強度為情境特徵；通知不鼓勵過量。
**→ Spec**：`drinking-behavior-prediction`（情境特徵）、`safety-and-privacy-boundaries`。

### R-D3 攝取量、水合狀態與過度補水

高齡者因共病與器官功能下降更易過度補水，且心臟衰竭等病人需追蹤攝取與排出 [1]；提醒提高攝取充足率，但水合狀態無顯著差異 [4]。
**→ 設計含意**：只做行為提醒與紀錄，不推論脫水，對有醫囑限水者提示先諮詢專業人員。
**→ Spec**：`safety-and-privacy-boundaries`。

## Article Summary Table

| Ref. | Year | Problem | AI Method | Domain Knowledge | Dataset | Findings | Limitation |
|---|---|---|---|---|---|---|---|
| [1] Cohen | 2021 | 高齡者液體攝取監測 | 綜述，依感測方式分類 | 脫水與過度補水風險 | —（綜述） | 多技術融合最有希望 | 未提出新系統；對象為高齡者；不含 LLM 決策 |
| [2] Griffith | 2019 | 容器附著 IMU 追蹤飲水 | 兩階段：sip 偵測 + 量估計 | 飲水動作運動學 | 辦公環境實際蒐集 + 實驗資料 | 實際資料 MAPE 31.8% | 誤差仍高；無提醒與情境 |
| [3] Roy | 2022 | 低功耗、不上傳原始資料 | 端點 TinyML | 飲水手勢、加水循環 | 4 位受試者；訓練 1,918 sip + 1,918 非 sip | sip 94–95%；體積誤差 0–28% | 單軸 IMU、受試者少；漏偵測累積誤差 |
| [4] Sari | 2024 | TFI 不足，忘記喝水 | —（App 定時提醒） | TFI 充足率、水合狀態 | 106 人分組、87 人完成；4 週 | TFI 充足率 90.2% vs. 63.0%；水合無顯著差異 | 準實驗、僅健康男性、僅 4 週；只比較開／關 |
| [5] Haag | 2025 | LLM 觸發個人化 JITAI | GPT-4 | 心臟復健身體活動 | 3 persona × 5 情境；450 提案 | 評分高於人類版本 | 概念情境；評文字非真實成效 |
| [6] Miftahuddin | 2025 | 共用情境個人化飲水監測 | —（規則篩選） | Watson、Holliday–Segar 估目標 | 3 人、約 4 個月；1,747 → 904 筆 | 最大校正誤差 0.4 g；約 51.7% 有效 | 無法確認真實飲用；目標只用靜態個資；樣本 3 人 |
| [7] Rezaee | 2026 | JITAI 規則難維護 | LLM Agent（skill 檔） | 睡眠與生活行為 | 單一使用者；8 次執行 | 完整循環可自動運作 | 成功執行不等於適當；單一使用者 |

## 歸納：Existing Research → Limitations → Research Gap → Our Project

```
Existing Research            Limitations                    Research Gap                          Our Project
重量／IMU 飲水量估計    →    事件有效性與誤差 (L2)     →   缺少「真實重量感測 + ML 事件辨識   →  Smart Hydration Monitoring Agent
規則篩選有效事件             無情境與回饋 (L1, L2)            + 情境感知 Agent 決策 + 回饋閉環」       ML 辨識與預測、護欄內 Agent 決策、
提醒開／關對照               決策適當性未驗證 (L3)            且未與固定提醒對照                        LINE 回饋；三種提醒策略比較
LLM 於 JITAI 概念與原型      攝取 ≠ 水合 (L4)
```

## Gap → Spec 對照

| Gap | Spec |
|---|---|
| 缺 ML 事件辨識與門檻法比較 | `drinking-event-detection` |
| 缺情境輸入的行為預測 | `drinking-behavior-prediction` |
| 決策適當性無對照 | `reminder-decision-agent`、`strategy-comparison-experiment` |
| 缺回饋閉環 | `line-feedback-loop` |
| 攝取 ≠ 水合；LLM 隱私 | `safety-and-privacy-boundaries` |
