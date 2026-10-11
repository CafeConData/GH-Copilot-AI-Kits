# Persistence

## Purpose
EF Core without leaking storage concerns.

## Rules
- **MUST** keep EF Core in Infrastructure.
- **MUST** avoid N+1 patterns.
- **MAY** use repositories only when they add value.

## AI guidance
Do not add Repository/Unit of Work mechanically.
