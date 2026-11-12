# Spec Delta

## Purpose

預測使用者未來 60 分鐘內發生飲水事件的機率，供 Agent 判斷是否需要提醒；此能力為探索性，資料不足時必須降級。

## ADDED Requirements

### Requirement: Predict drinking probability for the next 60 minutes
The system SHALL output a probability in [0, 1] that a Drinking event occurs within the next 60 minutes, from drinking history and user-logged context features.

#### Scenario: Normal prediction
- **WHEN** the agent requests a prediction for a user at time t
- **THEN** the system returns a probability computed from features available at or before t only

### Requirement: Avoid temporal leakage in evaluation
The system SHALL evaluate prediction models with a time-ordered split in which training data precede test data.

#### Scenario: Time-ordered split
- **WHEN** an experiment dataset is prepared
- **THEN** no test sample precedes any training sample in time

### Requirement: Degrade gracefully with insufficient data
The system SHALL fall back to a Historical Average Rule when the model is unavailable or the labeled data are insufficient, and SHALL NOT claim a reliable personalized predictor in that case.

#### Scenario: Model unavailable
- **WHEN** the prediction call fails or no trained model exists
- **THEN** the fallback rule supplies the value and the decision log marks the source as fallback

### Requirement: Compare against baselines
The system SHALL report Precision, Recall, F1-score, and PR-AUC for the model, the Fixed Interval Rule, and the Historical Average Rule on the same test split.

#### Scenario: Baseline report
- **WHEN** Experiment 2 is run
- **THEN** a table lists all three methods with the four metrics on identical data
