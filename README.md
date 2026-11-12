# Smart Hydration Monitoring Agent

基於重量感測、機器學習與情境感知 AI Agent 的智慧飲水監測系統
*From Passive Water Tracking to Proactive, Context-Aware Hydration Assistance.*

- 課程：物聯網數據分析與應用 (6679)，國立中興大學
- 組員：鄭安、林煜宸、宋武晉
- Repository：https://github.com/AngJeng/AIOT_final_project
- 狀態：**Proposal 階段（Planned）**——尚無實作與實驗結果；系統只做行為提醒，不做醫療診斷。

## 這個 repo 怎麼讀

| 想知道 | 看哪裡 |
|---|---|
| 摘要 A–F、5 核心／3 取捨／10 細節 | [`docs/01-abstract-A-F.md`](docs/01-abstract-A-F.md) |
| 300 字摘要、IEEE 風格英文摘要 | [`docs/02-abstracts.md`](docs/02-abstracts.md) |
| Introduction（L／RQ／I／C 可追溯） | [`docs/03-introduction.md`](docs/03-introduction.md) |
| Related Work、Article Summary Table、Research Gap | [`docs/04-related-work.md`](docs/04-related-work.md) |
| 架構、資料、演算法、實作對照 | [`DESIGN.md`](DESIGN.md) |
| 系統「必須」做到什麼（行為契約） | [`openspec/changes/smart-hydration-monitoring-agent/specs/`](openspec/changes/smart-hydration-monitoring-agent/specs) |
| 為什麼做、設計取捨、任務清單 | `proposal.md`、`design.md`、`tasks.md`（同上資料夾） |
| 參考文獻 | [`docs/references.md`](docs/references.md) |

## 規格驅動開發（OpenSpec 2.0.0）

```bash
npm install -g @fission-ai/openspec
openspec validate smart-hydration-monitoring-agent --strict   # 驗證規格
openspec show smart-hydration-monitoring-agent                # 檢視 change
openspec list                                                 # 進行中的 change
# 實作並通過審查後：
openspec archive smart-hydration-monitoring-agent             # 把 delta 併入 openspec/specs/
```

流程：**proposal → specs（SHALL + Scenario）→ design → tasks → apply → archive**。行為變更先改 spec，再改 DESIGN.md 與程式。

七個 capability：`drinking-event-detection`、`drinking-behavior-prediction`、`reminder-decision-agent`、`line-feedback-loop`、`hydration-dashboard`、`safety-and-privacy-boundaries`、`strategy-comparison-experiment`。

## 技術棧（規劃）

Raspberry Pi 4（Existing）+ Load Cell + HX711；Python、FastAPI、SQLite；scikit-learn / XGBoost；LLM API + Tool Calling；LINE Messaging API；Next.js + Chart.js。

## 誠信聲明

不使用或虛構公開資料集；未實驗的數值一律標示 Planned / Expected / Illustrative Example；LLM 僅生成通知文字，不負責計算或安全判斷。
