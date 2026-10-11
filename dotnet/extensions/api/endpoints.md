# Endpoints

## Purpose
Thin ASP.NET Core endpoints.

## Rules
- **MUST** keep endpoints focused on transport.
- **MUST NOT** implement business logic in controllers/handlers.
- **SHOULD** preserve the project's Controllers/Minimal APIs choice.

## AI guidance
Bind → validate → invoke Application → map to HTTP.
