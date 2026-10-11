# Retries and Idempotency

## Purpose
Safe workflow retries.

## Rules
- **MUST** understand side effects before retries.
- **MUST** make repeatable writes idempotent where required.

## AI guidance
Never add retries blindly around non-idempotent operations.
