# Design Patterns

## Purpose
Use architectural patterns deliberately, not by default.

## Rules
- **MAY** use Repository/Unit of Work only when they add real value over EF Core.
- **MAY** use CQRS/Mediator only if complexity justifies it.
- **MUST NOT** introduce layers, patterns or abstractions speculatively.

## AI guidance
Prefer the simplest design that satisfies the dependency rule in `/ai-baseline/architecture.md`.
