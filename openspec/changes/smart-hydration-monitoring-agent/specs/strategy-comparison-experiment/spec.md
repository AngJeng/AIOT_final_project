# Spec Delta

## Purpose

以一致的記錄與指標比較三種提醒策略，作為回答 RQ3 的實驗依據，且不超出小樣本所能支持的結論。

## ADDED Requirements

### Requirement: Support three reminder strategies
The system SHALL support strategy A (fixed interval), B (ML-based: prediction plus fixed rules), and C (ML plus agent with context, preferences, and feedback), selectable per experiment period.

#### Scenario: Switch strategy
- **WHEN** the experimenter selects strategy B for a period
- **THEN** reminders in that period follow strategy B only and the period is recorded

### Requirement: Record order and exposure
The system SHALL record the strategy, user, period, and order for every reminder so that order and novelty effects can be examined.

#### Scenario: Crossover record
- **WHEN** the same user uses all three strategies in turn
- **THEN** the stored order identifies which strategy came first, second, and third

### Requirement: Report behavioral metrics only
The system SHALL compute reminder acceptance rate, ineffective reminder ratio, user satisfaction, system latency, and record completeness, and SHALL report them descriptively without significance or generalization claims.

#### Scenario: Metric report
- **WHEN** an experiment ends
- **THEN** a per-strategy table of these metrics is produced together with the sample size and stated limitations
