---
description: "Propone una Clean Architecture completa para una solución .NET nueva, desde cero"
name: new-solution-architecture
agent: architect
argument-hint: "Dominio, requisitos principales y restricciones de la nueva solución"
---

Usa `/ai-baseline/` y `/ai-extensions/architecture/`. Aplica las skills `architecture/propose-architecture` y `architecture/define-project-structure`.

## Datos de entrada
- Dominio / contexto de negocio: ${input:domain}
- Requisitos principales (funcionales y no funcionales): ${input:requirements}
- Restricciones (equipo, infraestructura, plazos, integraciones existentes): ${input:constraints}

## Entregable esperado
1. Capas propuestas (Domain, Application, Infrastructure, Presentation, y otras si se justifican) y dirección de dependencias.
2. Proyectos/carpetas por capa, con convención de namespaces.
3. Patrones a aplicar y patrones a evitar, según `/ai-extensions/architecture/design-patterns.md`.
4. Cross-cutting concerns: inyección de dependencias, manejo de errores, observabilidad.
5. Riesgos, trade-offs y alternativas descartadas.
6. Próximos pasos sugeridos para `api-developer` y `test-engineer`.

No generes código todavía: produce solo la propuesta de arquitectura para validarla antes de crear el scaffolding de proyectos.
