# 02 摘要：300 字版與 IEEE 風格英文版

> 兩份摘要都只描述**預期**，不含實驗結果。引用編號見 `references.md`；摘要依慣例不放引用編號。

## 300 字中文摘要（中文字元 289 字，含標點與英文約 454 字元）

飲水常因「忘記喝水」而不足：印尼東加里曼丹約 41% 成年人總液體攝取不足，忘記喝水為最大障礙。然而既有提醒多依固定時間或靜態個資，智慧水杯少整合運動、飲食情境與提醒回饋，LLM 適時介入研究也指出系統能執行不代表決策適當。本專題提出 Smart Hydration Monitoring Agent，結合重量感測、機器學習與情境感知 AI Agent：Raspberry Pi 4、荷重元與 HX711 蒐集杯重時序，機器學習辨識飲水、加水、移動杯子等事件並預測短期飲水行為；Agent 以 Tool Calling 整合紀錄、預測、使用者情境與回饋，在冷卻、安靜時段、每日上限等規則護欄內決定是否提醒，LLM 僅生成文字，並經 LINE Bot 推播、收集回覆。我們規劃以自建資料集比較門檻法與 ML 的事件辨識，並比較固定間隔、ML-based 與 ML + Agent 三種策略的接受率與無效提醒比例。目前尚未實驗；預期產出低成本原型、時序資料集與提醒策略的初步比較，系統僅做行為提醒，不做醫療診斷。

## IEEE Transactions / Systems Journal 風格英文摘要

**Title—** Smart Hydration Monitoring Agent: A Context-Aware AI Agent with Load-Cell Sensing and Machine Learning for Proactive Drinking Reminders

**Abstract—** Inadequate fluid intake is frequently attributed to simply forgetting to drink, yet most hydration reminders still follow fixed schedules or static user profiles. Sensor-based smart bottles can record intake automatically but rarely exploit exercise or meal context or reminder feedback, and recent studies of large language model (LLM)-based just-in-time adaptive interventions (JITAIs) show that an agent can run end to end without establishing that its reminders are appropriate. This paper proposes the Smart Hydration Monitoring Agent, a low-cost AIoT system that couples load-cell sensing, machine learning, and a context-aware AI agent. A Raspberry Pi 4 with an HX711 amplifier samples cup-weight time series; a classifier separates drinking from refilling, cup movement, and other events, and a secondary model estimates the probability of a drinking event within the next 60 minutes. The agent uses tool calling to combine intake history, model outputs, user-logged exercise and meal context, preferences, and feedback, and decides within deterministic guardrails (cooldown, quiet hours, daily cap, pause) whether to send a reminder through a LINE bot; the LLM only drafts message text and performs no calculation or safety decision. We plan to evaluate event recognition against a threshold baseline and to compare fixed-interval, ML-based, and ML-plus-agent reminder strategies using F1-score, reminder acceptance rate, and the ratio of ineffective reminders. No experiments have been conducted yet; the system provides behavioral reminders only and makes no medical or dehydration claims.

**Index Terms—** AIoT, domain-specific AI agents, hydration monitoring, load cell, just-in-time adaptive intervention, machine learning, tool calling, context awareness, LINE bot.

寫作慣例：單一段落、不放引用編號、縮寫首次出現即展開（LLM、JITAI）、第三人稱、以「提出—方法—規劃評估—限制」收尾。字數約 232 詞。
