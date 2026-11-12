# Spec Delta

## Purpose

從杯墊重量時序中切出「拿起—放回」片段，區分飲水與其他重量變化，並只對飲水事件估計飲水量，作為所有提醒決策的資料基礎。

## ADDED Requirements

### Requirement: Segment weight stream into candidate events
The system SHALL segment the cup-weight stream into candidate segments whenever the reading leaves its stable level (cup lifted, or movement exceeds a configurable threshold) and returns to a stable level.

#### Scenario: Cup lifted and put back
- **WHEN** the reading drops below the off-pad threshold and later returns to a stable value
- **THEN** one candidate segment is produced with pre-weight, post-weight, and duration

#### Scenario: Sensor noise only
- **WHEN** readings fluctuate within the noise band and never leave the stable level
- **THEN** no candidate segment is produced

### Requirement: Classify each segment by event type
The system SHALL assign each candidate segment exactly one type from Drinking, Refilling, CupMovement, and Other, using features that include weight difference, slope statistics, stability, and segment duration.

#### Scenario: Weight decreases after lifting
- **WHEN** the post-weight is lower than the pre-weight and the classifier outputs Drinking
- **THEN** the segment is stored with type Drinking

#### Scenario: Weight increases
- **WHEN** the post-weight is higher than the pre-weight
- **THEN** the segment SHALL NOT be counted as Drinking and is stored as Refilling or Other

### Requirement: Estimate volume only for Drinking events
The system SHALL estimate drinking volume as max(0, pre-weight − post-weight) divided by an assumed liquid density, and SHALL NOT add volume for non-Drinking segments.

#### Scenario: Illustrative drinking event
- **WHEN** a Drinking segment has pre-weight 550 g and post-weight 420 g (illustrative values)
- **THEN** the estimated volume is about 130 mL under the stated 1 g/mL density assumption

#### Scenario: Pouring out is not drinking
- **WHEN** a segment is classified as Other with a weight decrease
- **THEN** the daily intake total is unchanged

### Requirement: Provide a threshold baseline
The system SHALL provide a threshold-based detector on weight difference and duration, so that classifier results can be compared against it.

#### Scenario: Baseline comparison
- **WHEN** the same labeled segments are evaluated with the baseline and the classifier
- **THEN** Precision, Recall, F1-score, a confusion matrix, and volume MAE are reported for both

### Requirement: Preserve ground-truth labels separately
The system SHALL store manually confirmed labels separately from predicted labels so that user replies alone are not treated as ground truth.

#### Scenario: Manual confirmation
- **WHEN** a team member confirms a segment as Drinking in the annotation record
- **THEN** the label is stored in the ground-truth field without overwriting the predicted type
