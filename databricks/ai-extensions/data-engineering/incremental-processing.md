# Incremental Processing

## Purpose
Repeatable incremental workloads.

## Rules
- **MUST** define change/watermark semantics.
- **MUST** handle retries/idempotency.
- **MUST** define update/late-data behavior when relevant.

## AI guidance
Do not assume append-only sources.
