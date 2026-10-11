# Query Performance

## Purpose
Evidence-based optimization.

## Rules
- **MUST** measure before optimizing.
- **MUST** inspect Query History/Profile or equivalent evidence.
- **SHOULD** reduce scans/shuffles before scaling compute.
- **MUST NOT** default to scale-up for unexplained slowness.

## AI guidance
Diagnose → measure → bottleneck → optimize → re-run → compare.
