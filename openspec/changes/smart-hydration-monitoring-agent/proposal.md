# Proposal

## Why

飲水提醒多依固定時間或靜態個資；重量式智慧水杯雖可自動記錄，卻少整合運動、飲食情境與提醒回饋，而 LLM 於 JITAI 的研究也指出「能自動執行」不等於「提醒適當」。本專題要建立一個低成本、可重現的 AIoT 原型，並用實驗回答「ML 事件辨識 + 情境感知 Agent 是否比固定提醒更適當」。證據與來源見 `docs/01-abstract-A-F.md` 與 `docs/03-introduction.md`。

## What Changes

這是 greenfield 專案，目前沒有既有程式碼；本 change 定義第一版系統的行為契約。

- 新增以重量時序辨識飲水事件（Drinking / Refilling / CupMovement / Other）並估計飲水量的能力（RQ1）。
- 新增短期飲水行為預測：未來 60 分鐘內發生飲水事件的機率（RQ2，探索性，視資料量）。
- 新增在規則護欄內決定「是否、何時、如何」提醒的 AI Agent，LLM 僅生成通知文字（RQ3）。
- 新增 LINE 推播與使用者回覆的回饋閉環。
- 新增 Web Dashboard，含飲水概覽、運動／飲食情境登記、分析與 Agent 決策紀錄。
- 新增安全與隱私邊界：不做醫療診斷、資料最小化、使用者可暫停。
- 新增三種提醒策略（固定間隔、ML-based、ML + Agent）的比較實驗能力。

**Out of scope**：醫療診斷或脫水判定、公開資料集訓練、行動 App、相機影像辨識、多使用者雲端部署、健康成效（水合或臨床結果）宣稱。

## Capabilities

### New Capabilities
- `drinking-event-detection`: 由重量時序切分片段、分類事件類型並估計飲水量，含門檻法 Baseline。
- `drinking-behavior-prediction`: 以歷史與情境特徵預測未來 60 分鐘內的飲水事件機率，含 Baseline 與資料量不足時的降級。
- `reminder-decision-agent`: 以 Tool Calling 整合資料，在規則護欄內決定 SEND／SKIP，並記錄每次決策。
- `line-feedback-loop`: 以 LINE 發送提醒、接收三種回覆並更新提醒參數。
- `hydration-dashboard`: 提供四個頁面的資料呈現與情境登記。
- `safety-and-privacy-boundaries`: 醫療聲明、通知用語限制、LLM 資料去識別化、暫停與刪除。
- `strategy-comparison-experiment`: 三種提醒策略的比較實驗設計與記錄。

### Modified Capabilities
<!-- 無：本專案目前沒有既有 spec。 -->

## Impact

- 新增系統元件：Data Collector（Raspberry Pi）、FastAPI Backend、SQLite、ML 模組、Agent、LINE Bot、Next.js Dashboard。
- 外部相依：LLM API、LINE Messaging API（Webhook 需公開 HTTPS；推播額度須查證官方現行方案）。
- 硬體：Load Cell、HX711、杯墊結構尚未到貨，成本未確認；到貨前以標示為「模擬資料」的資料驗證軟體流程。
- 文件：`DESIGN.md` 自本 change 起持續維護。
