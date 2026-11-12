# Tasks

對應六週時程（Planned；Week 1 取決於 Load Cell 與 HX711 到貨）。必要項目優先：感測 → 事件辨識 → Agent 提醒 → LINE 回饋的完整鏈路。

## 1. Planning artifacts（Week 0）

- [x] 1.1 安裝 OpenSpec 並 `openspec init`；驗證：`openspec --version` 與 `openspec/config.yaml` 存在
- [x] 1.2 完成 proposal、7 個 spec delta、design；驗證：`openspec validate smart-hydration-monitoring-agent --strict` 通過
- [x] 1.3 建立 GitHub Repository 並推送；驗證：連結填入報告與 README（README 已更新；書面報告的連結待自行更新）

## 2. Hardware & data collection（Week 1–2；drinking-event-detection）

- [ ] 2.1 組裝 Load Cell + HX711 + Raspberry Pi 並校正；驗證：以已知砝碼記錄誤差表，並確認讀數穩定
- [ ] 2.2 實作 Data Collector（Python）與 SQLite schema；驗證：連續記錄 10 分鐘，資料表筆數與取樣率相符
- [ ] 2.3 實作片段切分與門檻法 Baseline；驗證：單元測試涵蓋「拿起放回」「僅雜訊」兩個情境
- [ ] 2.4 建立人工 Ground Truth 記錄流程；驗證：標註資料與預測欄位分開儲存
- [ ] 2.5 撰寫資料集說明（欄位、來源、隱私、限制）；驗證：`docs/dataset.md` 列出所有欄位且標明模擬資料

## 3. ML models（Week 3；drinking-event-detection、drinking-behavior-prediction）

- [ ] 3.1 實作特徵工程與 Task A（Random Forest / XGBoost）；驗證：輸出 Precision、Recall、F1、Confusion Matrix、volume MAE，並與 Baseline 同表
- [ ] 3.2 實作 Task B 與兩個 Baseline（時間切分）；驗證：測試集時間皆晚於訓練集的檢查通過
- [ ] 3.3 實作模型不可用時的降級規則；驗證：關閉模型檔後預測呼叫回傳 fallback 並在紀錄中標示

## 4. Backend & Dashboard（Week 4；hydration-dashboard）

- [ ] 4.1 實作 FastAPI 端點（事件、情境、偏好、決策紀錄）；驗證：API 測試通過
- [ ] 4.2 實作 Dashboard 四頁；驗證：手動走過 Overview、情境登記、Analytics、Agent Insights，且無診斷用語
- [ ] 4.3 實作刪除資料與暫停；驗證：刪除後資料庫查詢為空、暫停後 Agent 回傳 SKIP

## 5. Agent & LINE（Week 5；reminder-decision-agent、line-feedback-loop、safety-and-privacy-boundaries）

- [ ] 5.1 實作 8 個 Agent Tools；驗證：每個 Tool 有單元測試，確定性 Tool 輸出可重現
- [ ] 5.2 實作護欄與 SEND/SKIP 決策；驗證：冷卻、安靜時段、每日上限、目標已達、暫停各一個測試
- [ ] 5.3 實作 LLM 文字生成、詞彙檢查與模板備援；驗證：模擬 LLM 失敗與含禁用詞輸出，皆改用模板
- [ ] 5.4 實作 LLM 輸入去識別化；驗證：攔截送出內容，不含姓名、LINE ID、原始重量序列
- [ ] 5.5 串接 LINE 推播與三種回覆、逾時處理；驗證：實機收到訊息，三種回覆與逾時皆寫入資料庫
- [ ] 5.6 實作規則式提醒參數更新；驗證：連續「暫時不需要」後該時段被抑制

## 6. Experiments & handover（Week 6；strategy-comparison-experiment）

- [ ] 6.1 實作三種策略切換與順序紀錄；驗證：每筆提醒皆含策略、使用者、時期、順序
- [ ] 6.2 執行實驗 1–3 並產生報告表；驗證：每個指標附樣本數與限制，無顯著性宣稱
- [ ] 6.3 更新 `DESIGN.md` 的 Implementation Mapping 與 Results；驗證：新開發者僅憑 README 與 DESIGN.md 能啟動系統
- [ ] 6.4 準備 Final PPT（>20 頁）、Demo 影片（>10 分鐘）；驗證：頁數與時長達標

## Workflow follow-up

- 實作完成並通過審查後，執行 `openspec archive smart-hydration-monitoring-agent` 把 spec 併入 `openspec/specs/`。
- 封存後以 `openspec list --specs` 驗證 7 個 capability 皆存在。
