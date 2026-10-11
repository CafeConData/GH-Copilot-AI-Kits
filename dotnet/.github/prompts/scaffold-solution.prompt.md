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


## Ejemplos de valor de parámetros

Arquitectura aprobada para Coffee And Data: monolito ASP.NET Core con SQL Server, desplegado localmente como un único proceso. MVP de catálogo de productos: creación, consulta por identificador y listado paginado. No incluir stock, movimientos, edición, eliminación ni integraciones externas. Confirmar campos y reglas de unicidad antes de implementar los casos de uso.

Crear una solución .NET 10 en la raíz del workspace con estos proyectos:
- src/CoffeeAndData.Domain (classlib).
- src/CoffeeAndData.Application (classlib).
- src/CoffeeAndData.Infrastructure (classlib).
- src/CoffeeAndData.Presentation (webapi).
- tests/CoffeeAndData.UnitTests (xunit).
- tests/CoffeeAndData.IntegrationTests (xunit).

Antes de crear proyectos, comprobar si ya existe una solución o un host ASP.NET Core y reutilizarlos cuando corresponda. No duplicar el host ni eliminar código existente innecesariamente.

Referencias:
- Application -> Domain.
- Infrastructure -> Application y Domain.
- Presentation -> Application e Infrastructure; esta última solo para composición de dependencias.
- UnitTests -> Domain y Application.
- IntegrationTests -> Presentation e Infrastructure.

Domain no tendrá dependencias de frameworks o infraestructura. Application no dependerá de HTTP ni EF Core. Los endpoints consumirán casos de uso de Application, nunca directamente el DbContext.

Namespaces raíz iguales al nombre de cada proyecto. Carpetas:
- Domain: Products.
- Application: Products/CreateProduct, Products/GetProduct, Products/ListProducts y Products/Persistence.
- Infrastructure: Products, Persistence/Configurations y Persistence/Migrations.
- Presentation: Products y ErrorHandling.
- UnitTests: Products.
- IntegrationTests: Products y Api.

Paquetes mínimos:
- Domain y Application: ninguno.
- Infrastructure: Microsoft.EntityFrameworkCore.SqlServer compatible con .NET 10.
- Presentation: conservar los paquetes ASP.NET Core/OpenAPI de la plantilla o del proyecto reutilizado.
- Ambos proyectos de pruebas: Microsoft.NET.Test.Sdk, xunit y xunit.runner.visualstudio.
- IntegrationTests: Microsoft.AspNetCore.Mvc.Testing para pruebas HTTP con TestServer.

Usar casos de uso explícitos e inyección por constructor. Permitir un contrato específico de persistencia de productos en Application, implementado en Infrastructure, sin exponer tipos de EF Core ni IQueryable. No introducir Mediator, CQRS, repositorios genéricos, Unit of Work adicional ni infraestructura distribuida.

No implementar entidades, contratos ni casos de uso todavía. Entregar únicamente comandos de scaffolding, referencias, paquetes y estructura propuesta. Siguiente paso: delegar en api-developer la creación de producto, tras acordar sus campos y reglas de duplicados.
