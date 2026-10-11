# Databricks AI-Kit
## Baseline
Common rules: `/ai-baseline/`.
## Extensions
- `/data-engineering/`
- `/sql/`
- `/workflows/`
- `/ai-extensions/testing/`
## GitHub Copilot
`.github/` contains instructions, agents and specialized skills.
- `copilot-instructions.md`: instrucciones generales que Copilot aplica siempre en este AI-Kit.
- `agents/`: definiciones de agentes especializados (data-engineer, sql-engineer, tester, workflow-engineer) que combinan ai-baseline, especializaciones y skills para una tarea concreta.
- `instructions/`: instrucciones específicas por tipo de fichero (`applyTo`), aplicadas automáticamente según la extensión o ruta del archivo en el que se trabaja.
- `skills/`: skills reutilizables agrupadas por especialización (data-engineering, sql, testing, workflows) que los agentes invocan para realizar acciones concretas (crear transformaciones, cargas incrementales, revisiones de pipelines, etc.).
