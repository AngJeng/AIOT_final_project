# Spec Delta

## Purpose

透過 LINE 把提醒送給使用者並收回覆，使回覆成為 Agent 後續提醒參數與評估指標的資料來源。

## ADDED Requirements

### Requirement: Send reminders through LINE
The system SHALL send an approved reminder message to the user's LINE account and record the send time and reminder type.

#### Scenario: Reminder sent
- **WHEN** the agent returns SEND
- **THEN** a LINE message is delivered and a reminder record is stored

#### Scenario: Delivery failure
- **WHEN** the LINE API returns an error
- **THEN** the failure is logged and the reminder is not counted as accepted

### Requirement: Accept three reply types
The system SHALL accept the replies "已喝水", "稍後提醒", and "暫時不需要", and treat no reply within a configurable timeout as a timeout outcome.

#### Scenario: Snooze reply
- **WHEN** the user replies "稍後提醒"
- **THEN** the reply is stored and the cooldown is extended by a configurable amount

#### Scenario: Timeout
- **WHEN** no reply arrives within the timeout
- **THEN** the outcome is stored as timeout

### Requirement: Update reminder parameters from feedback by rules
The system SHALL adjust reminder parameters such as cooldown length or avoided time windows by deterministic rules from stored replies, and SHALL NOT claim online learning.

#### Scenario: Repeated "not needed" replies
- **WHEN** the user replies "暫時不需要" repeatedly in the same time window
- **THEN** reminders in that window are suppressed according to the configured rule
