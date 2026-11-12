# Spec Delta

## Purpose

限定系統只做行為提醒，並保護使用者的運動、飲食與飲水資料。

## ADDED Requirements

### Requirement: No medical diagnosis or hydration-status claims
The system SHALL NOT diagnose, judge dehydration, or give medical-grade hydration advice in any notification, dashboard text, or report.

#### Scenario: Vocabulary check
- **WHEN** a generated message contains a prohibited term from the configured list
- **THEN** it is rejected and a template message is used

### Requirement: Caution for restricted-fluid users
The system SHALL show a notice advising users with medically restricted fluid intake to consult a healthcare professional and SHALL let them pause reminders.

#### Scenario: Pause reminders
- **WHEN** the user pauses reminders
- **THEN** the agent returns SKIP until the user resumes

### Requirement: Minimize and de-identify data sent to the language model
The system SHALL send only the fields needed to draft a message and SHALL remove identifiers before any call to an external language model API.

#### Scenario: Outgoing payload
- **WHEN** the system calls the language model API
- **THEN** the payload contains no user name, LINE ID, or raw weight stream

### Requirement: Support data deletion and no camera use
The system SHALL let the user delete stored records, and SHALL NOT use camera images.

#### Scenario: Delete records
- **WHEN** the user requests deletion
- **THEN** their records are removed from the database
