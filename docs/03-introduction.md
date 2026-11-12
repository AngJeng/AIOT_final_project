# 03 Introduction（OpenSpec 重構版）

> 重構原則：每個論述都有編號並可追到 spec。**L＝限制（Limitation）、RQ＝研究問題、I＝創新點、C＝貢獻**。引用編號見 `references.md`。
> 內容與已繳交版本一致，差別在結構：把「問題 → 限制 → 創新 → 貢獻」變成可追溯鏈，而不是一段敘述。

**Why is this project worth doing?** 因為「提醒喝水」已是公共衛生議題，但現有做法要嘛固定時間、要嘛只記錄不決策，且「提醒是否適當」尚無對照證據。

## A. Attention Getter

印尼東加里曼丹成人約 41% 的總液體攝取（TFI）不足，高於全國的 28%，遵循建議攝取量的最大障礙是忘記喝水 [4]。臺灣國健署 2026 年公布的新版每日飲食指南草案首度將水分納入正式飲食章節，並說明實際需求須依體型、活動量及氣候調整 [12, 13]。¹

¹ 各媒體報導的具體建議水量不一致，本專案只引用「首度納入」與「需求依個人與情境調整」兩項，數值待核對官方公告。

## B. Background

- **應用領域與使用者**：一般健康成人（學生、辦公室工作者）；需求為「自動記錄飲水」與「適時被提醒」。定位為行為支持工具，**非醫療裝置**。
- **技術背景**：飲水監測可分穿戴式、嵌入感測的表面、視覺／環境式、智慧容器四類，多技術融合最有希望 [1]；容器附著或杯墊式重量感測可直接取得杯中水量變化，避免「手臂動作與飲水相似」的誤報 [2]；JITAI 主張依當下狀態與情境決定是否、何時、如何介入，而非固定間隔 [8]。

## C. Problem

**如何以低成本的重量感測自動取得可用的飲水紀錄，並結合生活情境與回饋，在適當時機提醒，而不造成過多干擾？**

| ID | 研究問題 | 對應 capability | 對應實驗 |
|---|---|---|---|
| RQ1 | 低成本重量感測能否自動估計飲水事件與飲水量？ | `drinking-event-detection` | 實驗 1 |
| RQ2 | 歷史紀錄與活動情境能否建立個人化飲水行為預測？（探索性） | `drinking-behavior-prediction` | 實驗 2 |
| RQ3 | ML 預測 + 情境 + 回饋的 Agent，是否比固定提醒更適當？ | `reminder-decision-agent`、`line-feedback-loop`、`strategy-comparison-experiment` | 實驗 3 |

## D. Existing Limitations

| ID | 限制（證據） | 造成的需求 |
|---|---|---|
| **L1** | 提醒缺乏個人化與策略比較：開啟提醒組 TFI 充足率較高（90.2% vs. 63.0%, p = 0.003），但只比較提醒開／關 [4] | 比較多種提醒策略（R7） |
| **L2** | 感測式系統的事件有效性與情境缺口：1,747 筆原始紀錄僅約 51.7%（904 筆）為有效事件；無法分辨重量減少是否真為飲用、目標只依靜態個資、無活動或環境情境 [6]；IMU 飲水量估計實際資料 MAPE 31.8% [2]；端點學習體積誤差 0–28%、受試者少 [3] | ML 事件辨識與情境輸入（R1、R2） |
| **L3** | 提醒決策的適當性未被驗證：JITAI 多為 if-then 規則、難維護 [9, 7]；LLM 研究或僅在概念情境評文字 [5]，或僅證明可自動運作、未證明改善行為 [7] | 規則護欄 + 與固定提醒對照（R3、R7） |
| **L4** | 攝取量不等於水合狀態：單一指標難偵測高齡者脫水 [1]；提醒提高攝取充足率，但水合狀態無顯著差異（87.8% vs. 73.9%, p = 0.103）[4] | 不診斷、不宣稱水合（R6） |

## E. Innovation

四件事放進同一閉環：

| ID | 創新 | 針對 | Spec |
|---|---|---|---|
| **I1** | 以重量時序做 **ML 事件辨識**，而非只用重量差門檻 | L2 | `drinking-event-detection` |
| **I2** | 以歷史與情境特徵做**短期飲水行為預測** | L1、L2 | `drinking-behavior-prediction` |
| **I3** | **AI Agent** 經 Tool Calling 整合預測、情境、偏好與冷卻，在**規則護欄內**決策 | L3 | `reminder-decision-agent` |
| **I4** | LINE 提醒與使用者回覆形成**回饋閉環** | L1、L3 | `line-feedback-loop` |

與既有工作的差異：相較 [6] 增加 ML 事件辨識、情境輸入與主動提醒決策；相較 [7] 具備真實飲水感測資料與 LINE 回饋，且嚴格區分「ML 預測」「確定性計算」「LLM 文字生成」三者責任。

## F. Contributions（預期，尚待驗證）

| ID | 預期貢獻 | 來源 |
|---|---|---|
| C1 | 低成本、可重現的智慧飲水監測原型（Raspberry Pi 4 + Load Cell + HX711） | I1 |
| C2 | 重量時序飲水事件資料集（含人工確認 Ground Truth，隱私許可下整理） | I1 |
| C3 | Threshold-based 與 ML-based 事件辨識的比較 | I1 |
| C4 | 情境感知 Agent 在個人化飲水提醒中的價值（固定間隔 vs. ML-based vs. ML + Agent） | I2–I4 |
| C5 | 具 Web Dashboard、LINE 通知與使用者回饋的 AIoT Demo | 全部 |

## 可追溯鏈（Traceability）

```
L2 ──► RQ1 ──► I1 ──► drinking-event-detection ──► 實驗 1 ──► C1, C2, C3
L1, L2 ► RQ2 ► I2 ──► drinking-behavior-prediction ► 實驗 2
L1, L3 ► RQ3 ► I3, I4 ► reminder-decision-agent + line-feedback-loop ► 實驗 3 ► C4
L4 ──► safety-and-privacy-boundaries（限制，不是貢獻）
```

## 文章架構

`docs/04-related-work.md`（相關研究與缺口）→ `DESIGN.md`（架構、資料、演算法）→ `openspec/`（行為契約與任務）。
