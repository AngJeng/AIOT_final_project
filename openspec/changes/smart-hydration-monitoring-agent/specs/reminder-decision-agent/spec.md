# Spec Delta

## Purpose

以 Tool Calling 整合飲水紀錄、預測、情境與偏好，在確定性規則護欄內決定是否、何時、如何提醒，並讓每次決策可稽核。

## ADDED Requirements

### Requirement: Use deterministic tools for data and rules
The agent SHALL obtain intake history, daily intake, preferences, activity context, and cooldown status through tool calls, and SHALL obtain the drinking probability through the prediction tool.

#### Scenario: Scheduled trigger
- **WHEN** the scheduler, a new drinking event, or a user query triggers the agent
- **THEN** the agent calls the history, daily-intake, preference, context, prediction, and cooldown tools before deciding

### Requirement: Enforce guardrails before any reminder
The agent SHALL return SKIP without sending when any guardrail fails: cooldown active, quiet hours, daily cap reached, daily goal reached, or reminders paused. The language model SHALL NOT override a guardrail.

#### Scenario: Quiet hours
- **WHEN** the current time falls within the user's quiet hours
- **THEN** the agent returns SKIP and records the reason

#### Scenario: Goal already reached
- **WHEN** the daily intake is at or above the user's goal
- **THEN** the agent returns SKIP and records the reason

### Requirement: Decide need for reminder from intake, gap, and probability
The agent SHALL send a reminder only if intake is behind the expected progress and either the time since the last drink exceeds a context-dependent threshold or the predicted probability is below a threshold.

#### Scenario: Behind schedule with long gap
- **WHEN** guardrails pass, intake is behind expected progress, and the gap exceeds the threshold
- **THEN** the agent returns SEND with a reminder type

#### Scenario: On track
- **WHEN** guardrails pass but intake is not behind expected progress
- **THEN** the agent returns SKIP and records "no reminder needed"

### Requirement: Restrict the language model to message text
The agent SHALL use the language model only to draft notification text, SHALL fall back to a template when generation fails or the text fails the vocabulary check, and SHALL NOT use the model for volume, prediction, or guardrail decisions.

#### Scenario: Model failure
- **WHEN** the language model call errors or times out
- **THEN** a template message is sent instead and the log records the fallback

### Requirement: Log every decision
The agent SHALL write a record for every invocation containing trigger, inputs, probability and its source, guardrail results, action, reminder type, and reply.

#### Scenario: Skipped decision is logged
- **WHEN** the agent returns SKIP
- **THEN** a log record with the skip reason exists for that invocation
