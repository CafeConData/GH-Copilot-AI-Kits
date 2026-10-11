
---
description: "Implementa CRUD de productos en la solución CoffeeAndData existente."
name: new-crud-entity
agent: architect
argument-hint: "Resumen de acciones CRUD a implementar"
---

Usa `/ai-baseline/` y `/ai-extensions/api/`. Aplica la skill `api/create-endpoint`.

## Descripción CRUD
${input:descriptionCRUD}

## Entregable esperado
1. Modelo de dominio y sus invariantes en Domain, con validaciones explícitas de cada campo.
2. Casos de uso de creación, consulta (por id y listado paginado), actualización y eliminación en Application, junto con el contrato de persistencia requerido.
3. Implementación de persistencia en Infrastructure (EF Core, configuración de entidad, migración) sin exponer detalles de almacenamiento a capas superiores.
4. Endpoints HTTP (Minimal APIs) en Presentation con DTOs propios, validación de entrada, códigos de estado correctos y documentación OpenAPI.
5. Manejo de errores mediante Problem Details, sin exponer detalles internos.
6. Pruebas relevantes (unitarias de los casos de uso y, si aplica, de integración de los endpoints/persistencia).
7. Confirmación de que el proyecto compila y las pruebas pasan, indicando explícitamente qué se ejecutó y su resultado.
8. Lista de ambigüedades o decisiones pendientes de confirmar antes de dar la tarea por cerrada.

## Ejemplo valor de parámetro

Implementa CRUD de productos en la solución CoffeeAndData existente.

Sigue /ai-baseline/, /ai-extensions/api/ y las instrucciones
aplicables del repositorio. Aplica la skill dotnet-webapi.
Inspecciona primero el código y reutiliza las convenciones existentes.

Modelo de dominio Product:
- Id: Guid, generado por el servidor.
- Nombre: obligatorio, máximo 150 caracteres.
- Descripcion: opcional, máximo 1000 caracteres.
- Precio: decimal, mayor o igual a cero, precisión SQL decimal(18,2).

Estas reglas son la propuesta inicial; confirma cualquier ambigüedad
antes de implementar. No asumir unicidad del nombre.

Implementa endpoints mediante Minimal APIs:
- POST /api/products: crear; devolver 201 y cabecera Location.
- GET /api/products/{id}: consultar; devolver 200 o 404.
- GET /api/products: listar con paginación y orden estable.
- PUT /api/products/{id}: actualizar los campos editables;
  devolver 200 o 404.
- DELETE /api/products/{id}: eliminar físicamente;
  devolver 204 o 404.

Respeta las capas:
- Domain: entidad e invariantes.
- Application: casos de uso y contrato específico de persistencia.
- Infrastructure: EF Core, SQL Server y migración.
- Presentation: DTOs, validación HTTP, endpoints y OpenAPI.

No exponer entidades de dominio como contratos HTTP.
No introducir Mediator, repositorios genéricos ni Unit of Work.
Usar I/O asíncrono y propagar CancellationToken.
Representar errores mediante Problem Details sin detalles internos.
Mantener Swagger UI disponible únicamente en Development.
No aplicar migraciones a una base de datos sin autorización.

Añade pruebas relevantes, ejecuta compilación y pruebas,
y comunica los resultados y cualquier validación pendiente.