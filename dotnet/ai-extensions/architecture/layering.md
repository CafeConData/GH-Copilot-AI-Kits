# Layering

## Purpose
Clear separation between Domain, Application, Infrastructure and Presentation.

## Rules
- **MUST** keep Domain free of framework and infrastructure references.
- **MUST** keep Application orchestrating use cases without HTTP/EF Core details.
- **MUST** confine infrastructure concerns (EF Core, external APIs, messaging) to Infrastructure.
- **MUST** confine HTTP/UI concerns to Presentation.

## AI guidance
Classify new code by responsibility before choosing a layer; do not let a layer know about outer layers.
