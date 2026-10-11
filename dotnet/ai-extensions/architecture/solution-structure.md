# Solution Structure

## Purpose
Predictable mapping between architecture layers and solution projects.

## Rules
- **MUST** map one project per layer (Domain, Application, Infrastructure, Presentation) unless the repository already uses another convention.
- **MUST** align namespaces with project/layer boundaries.
- **SHOULD** group by feature within a layer when it improves cohesion.
- **MUST NOT** introduce new projects or layers without justification.

## AI guidance
Reuse the existing solution layout; only propose structural changes when requested or clearly required.
