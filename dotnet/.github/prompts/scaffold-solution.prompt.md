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

Arquitectura aprobada para Coffee And Data: MVP de catálogo de productos con creación, consulta individual y listado. No incluir todavía stock, almacenes ni movimientos. Los campos del producto y las reglas de duplicados quedan pendientes de acordar durante la implementación.

Crear una solución .NET 10 con estos proyectos:
- CoffeeAndData.Inventory.Domain (classlib)
- CoffeeAndData.Inventory.Application (classlib)
- CoffeeAndData.Inventory.Infrastructure (classlib)
- CoffeeAndData.Inventory.Api (webapi)
- CoffeeAndData.Inventory.Tests (xUnit; pruebas unitarias y de integración organizadas en carpetas separadas)

Reutilizar el proyecto web ASP.NET Core existente como CoffeeAndData.Inventory.Api; no crear un segundo host de API ni eliminar código de ejemplo sin necesidad. Crear la solución en la raíz del workspace.

Referencias:
- Application -> Domain
- Infrastructure -> Application y Domain
- Api -> Application e Infrastructure, solo para composición de dependencias
- Tests -> los proyectos que necesite probar
Domain no debe depender de otras capas. Application no debe depender de HTTP ni EF Core.

Namespaces raíz: CoffeeAndData.Inventory.Domain, CoffeeAndData.Inventory.Application, CoffeeAndData.Inventory.Infrastructure, CoffeeAndData.Inventory.Api y CoffeeAndData.Inventory.Tests. Agrupar el código por funcionalidad: Products en Domain y Application; Persistence/Configurations y Persistence/Migrations en Infrastructure; Endpoints y Contracts en Api; Products y Api en Tests.

Paquetes mínimos:
- Domain y Application: ninguno.
- Infrastructure: Microsoft.EntityFrameworkCore.SqlServer para persistencia local en SQL Server.
- Api: conservar los paquetes ASP.NET Core/OpenAPI del proyecto existente.
- Tests: paquetes mínimos de xUnit y Microsoft.NET.Test.Sdk; añadir Microsoft.AspNetCore.Mvc.Testing solo si las pruebas de integración ejercitan la API mediante TestServer. No añadir proveedores, herramientas de observabilidad ni paquetes de mediación.

Usar casos de uso explícitos. No introducir CQRS/Mediator, repositorios genéricos ni Unit of Work. Se puede añadir un contrato específico de persistencia de productos si es necesario para que Application no dependa de EF Core. Mantener la solución sencilla para un desarrollador y un plazo de una semana.

No implementar casos de uso todavía: entregar los comandos y estructura de scaffolding, referencias y paquetes. Siguiente paso: delegar en api-developer la implementación del primer caso de uso de creación de producto.
