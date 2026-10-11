# Architecture

## Purpose
Clean Architecture baseline.

## Rules
- **MUST** keep business rules independent from frameworks/infrastructure.
- **MUST** direct dependencies inward.
- **MUST NOT** reference Infrastructure from Domain.
- **MUST NOT** place HTTP concerns in Application or Domain.

## AI guidance
Identify the affected layer first; do not introduce architectural patterns without justification.
