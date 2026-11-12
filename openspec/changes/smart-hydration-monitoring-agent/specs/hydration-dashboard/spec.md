# Spec Delta

## Purpose

讓使用者檢視飲水紀錄與 Agent 決策、登記運動與飲食情境，並設定目標與提醒偏好。

## ADDED Requirements

### Requirement: Provide an overview page
The dashboard SHALL show today's intake, number of drinks, time of last drink, goal progress, and recent events.

#### Scenario: Open overview
- **WHEN** the user opens the overview
- **THEN** values are computed from stored Drinking events only

### Requirement: Allow context logging and preference settings
The dashboard SHALL let the user log exercise (type, duration, intensity, start time) and meals (type, caffeine, high-salt flag), and set a daily goal, quiet hours, and pause.

#### Scenario: Log exercise
- **WHEN** the user submits an exercise entry
- **THEN** it is stored and available to the context tool

### Requirement: Provide analytics and decision insights
The dashboard SHALL show hourly pattern, daily trend, drinking interval, and the agent's reminder records, with wording that describes records only.

#### Scenario: Insight wording
- **WHEN** a summary is generated
- **THEN** it contains no diagnosis or hydration-status claim and any example data is labeled Illustrative Example
