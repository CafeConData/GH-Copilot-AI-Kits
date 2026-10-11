---
description: "Genera el scaffolding de proyectos de una solución .NET a partir de una arquitectura ya aprobada"
name: scaffold-solution
agent: architect
argument-hint: "Resumen de la arquitectura aprobada (capas, proyectos, patrones)"
---

Usa `/ai-baseline/` y `/ai-extensions/architecture/`. Aplica la skill `architecture/define-project-structure`.

## Arquitectura aprobada
${input:approvedArchitecture}

## Entregable esperado
1. Lista de proyectos a crear (`dotnet new`) por capa, con su tipo (classlib, webapi, xunit, etc.).
2. Referencias entre proyectos respetando la dirección de dependencias.
3. Convención de namespaces y carpetas dentro de cada proyecto.
4. Paquetes NuGet mínimos imprescindibles por capa (sin añadir dependencias especulativas).
5. Siguiente paso recomendado: delegar en `api-developer` la implementación del primer caso de uso.
