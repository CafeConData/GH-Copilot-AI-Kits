# .NET AI-Kit
## Baseline
Common rules: `/ai-baseline/`.
## Extensions
- `/ai-extensions/api/`
- `/ai-extensions/testing/`
- `/ai-extensions/architecture/`

## GitHub Copilot
`.github/` contains instructions, specialized agents and their skills.

### Estructura

```text
.github/
├── copilot-instructions.md         # Instrucciones generales
├── instructions/                   # Reglas contextuales
│   ├── api.instructions.md
│   └── testing.instructions.md
├── agents/                         # Agentes especializados
│   ├── developer.agent.md
│   ├── tester.agent.md
│   └── architect.agent.md
├── skills/                         # Procedimientos reutilizables
│   ├── create-endpoint/
│   │   └── SKILL.md
│   └── create-unit-tests/
│       └── SKILL.md
└── prompts/                        # Prompts reutilizables (slash commands)
    ├── new-solution-architecture.prompt.md
    └── scaffold-solution.prompt.md
```

Los nombres de ejemplo corresponden a un kit .NET. Otras tecnologías pueden definir instrucciones, agentes y skills diferentes.

### `copilot-instructions.md`: reglas generales

Es el **punto de entrada de las instrucciones del repositorio**. Se utiliza para establecer criterios transversales: respetar la arquitectura, seguir los estándares del equipo, mantener la seguridad y comprobar los cambios realizados.

**Convención reconocida:** `.github/copilot-instructions.md`. GitHub Copilot puede incorporarlo automáticamente en las interacciones compatibles que usan el repositorio como contexto.

**Buena práctica:** mantenerlo breve. Los detalles extensos sobre arquitectura, API o testing deben permanecer en los documentos de conocimiento del kit y referenciarse desde las instrucciones pertinentes.

### `instructions/`: reglas que dependen del contexto

Contiene instrucciones para **tipos de archivos o áreas concretas del proyecto**. Por ejemplo, las reglas de un controlador HTTP no tienen por qué ser las mismas que las de una prueba unitaria.

**Convención reconocida:** `.github/instructions/**/*.instructions.md`. Cada fichero puede declarar en su encabezado YAML el patrón `applyTo` que determina a qué archivos se aplica.

Ejemplo de `testing.instructions.md`:

```
---
applyTo: "**/*Tests/**/*.cs"
---

- Test observable behavior.
- Keep tests deterministic.
- Follow the testing ai-baseline defined by this AI-Kit.
```

Cuando Copilot trabaja con archivos que coinciden con el patrón, puede combinar estas reglas con las instrucciones generales del repositorio. **No se seleccionan agentes por estar en esta carpeta**: aquí se definen reglas, no roles.

### `agents/`: especialistas para distintos trabajos

Contiene **agentes personalizados**: perfiles que describen una especialización, su responsabilidad, sus límites y, opcionalmente, las herramientas disponibles.

**Convención reconocida en VS Code:** `.github/agents/<nombre>.agent.md`. Estos perfiles pueden aparecer en el selector de agentes de Copilot Chat, según la versión y las capacidades habilitadas.

Ejemplo de `developer.agent.md`:

```
---
name: developer
description: Implementa y revisa APIs ASP.NET Core.
---

- Especialista en desarrollo de APIs.
- Sigue los estándares generales y la documentación de API del AI-Kit.
- No sitúes lógica de negocio en los controladores.
```

Un agente responde a la pregunta **«¿qué especialista debe abordar esta tarea?»**. La selección o invocación del agente es distinta de la aplicación automática de instrucciones mediante `applyTo`.

### `skills/`: procedimientos reutilizables

Contiene **skills**: instrucciones para ejecutar tareas concretas, como crear un endpoint, revisar una consulta SQL o preparar pruebas de integración.

**Convención reconocida:** cada skill debe tener su propio directorio con un archivo llamado exactamente `SKILL.md` y un encabezado YAML con, al menos, `name` y `description`.

Ejemplo de `.github/skills/create-endpoint/SKILL.md`:

```
---
name: create-endpoint
description: Crea un endpoint HTTP siguiendo los estándares del proyecto.
---

1. Revisa los endpoints existentes y sus convenciones.
2. Define los contratos de entrada y salida.
3. Implementa o reutiliza el caso de uso.
4. Aplica validación, seguridad y gestión de errores.
5. Añade y ejecuta las pruebas pertinentes.
```

Las skills responden a **«¿cómo se realiza esta tarea?»**. Copilot puede descubrirlas y utilizarlas cuando resulten relevantes, en las superficies que admiten Agent Skills. **No existe una relación obligatoria de uno a uno entre agentes y skills**: un agente puede utilizar varias skills y una skill puede resultar útil para más de un agente.

**Importante sobre la ubicación:** para maximizar la compatibilidad, se recomienda colocar cada skill directamente bajo `.github/skills/<nombre>/SKILL.md`. Si el equipo quiere agruparlas visualmente por especialización, puede usar prefijos en los nombres (por ejemplo, `api-create-endpoint/` y `testing-create-unit-tests/`). No debe darse por supuesto que una estructura adicional como `.github/skills/api/create-endpoint/SKILL.md` se descubrirá en todas las implementaciones.

### `prompts/`: prompts reutilizables para arrancar un flujo de trabajo

Contiene **prompt files**: plantillas de prompt reutilizables, invocables como slash command (`/nombre-del-prompt`) desde Copilot Chat, pensadas para arrancar una tarea completa (por ejemplo, diseñar la arquitectura de una solución nueva) sin tener que redactar el prompt cada vez.

**Convención reconocida en VS Code/Copilot Chat:** `.github/prompts/<nombre>.prompt.md`. El encabezado YAML admite, entre otros, `description`, `name`, `agent` (agente al que se dirige el prompt, puede ser uno personalizado como `architect`) y `argument-hint`. El cuerpo es Markdown y puede usar variables `${input:variable}` que Copilot solicitará al invocar el prompt.

Ejemplo de `new-solution-architecture.prompt.md` (resumido):

```
---
description: "Propone una Clean Architecture completa para una solución .NET nueva"
name: new-solution-architecture
agent: architect
argument-hint: "Dominio, requisitos y restricciones de la nueva solución"
---

- Dominio: ${input:domain}
- Requisitos: ${input:requirements}
- Restricciones: ${input:constraints}

Entrega capas, proyectos, patrones, cross-cutting concerns y próximos pasos.
```

A diferencia de las skills (que un agente descubre y aplica cuando resulta relevante), un prompt file se **invoca explícitamente** por el desarrollador, normalmente al principio de una tarea nueva (p. ej. "empezar una solución desde cero").

### Convenciones frente a documentación propia

| Elemento | ¿Lo reconoce Copilot por convención? | Para qué sirve |
| --- | --- | --- |
| `.github/copilot-instructions.md` | Sí | Reglas generales del repositorio |
| `.github/instructions/**/*.instructions.md` | Sí | Reglas específicas de archivos, normalmente mediante `applyTo` |
| `.github/agents/*.agent.md` | Sí, en las superficies compatibles | Perfiles de agentes personalizados |
| `.github/skills/<skill>/SKILL.md` | Sí, con soporte de Agent Skills | Procedimientos reutilizables |
| `.github/prompts/*.prompt.md` | Sí, como slash commands en Copilot Chat | Prompts reutilizables para arrancar una tarea |
| `ai-baseline/*.md`, `api/*.md`, `testing/*.md` | No por esos nombres | Conocimiento propio del AI-Kit |
| `.github/README.md` | No | Documentación para personas |

Los documentos de conocimiento pueden ser consultados por Copilot si se le indican o se incorporan al contexto; **no se cargan automáticamente solo por estar en esas carpetas**. Tampoco debe asumirse que una referencia genérica a una carpeta hace que Copilot lea todos sus documentos.

### Cómo encajan las piezas

En resumen:

- **Knowledge:** cómo desarrollamos software aquí.
- **Instructions:** qué reglas debe seguir Copilot.
- **Agents:** qué especialista aborda el trabajo.
- **Skills:** qué procedimiento seguir para una tarea.

### Uso en VS Code y distribución

Para que estas convenciones funcionen de forma predecible, la carpeta `.github` debe instalarse en la **raíz del repositorio o workspace consumidor**, de acuerdo con las capacidades de la versión de VS Code y GitHub Copilot utilizada. En un repositorio central que almacena varios AI-Kits (`dotnet/`, `databricks/`, etc.), no debe asumirse que todas las carpetas `.github` anidadas se activarán automáticamente al abrir la raíz común.

El repositorio de AI-Kits es el **origen de las plantillas**; el proyecto consumidor es el lugar donde se utilizan. Si el proyecto ya tiene una carpeta `.github`, hay que integrar su contenido sin sobrescribir reglas o agentes existentes indiscriminadamente.

### Recomendaciones de mantenimiento

1. Mantén las instrucciones generales cortas y evita repetir documentación extensa.
2. Usa `applyTo` específico y comprueba que coincide con los archivos reales del proyecto.
3. Define agentes por responsabilidad y skills por tarea concreta; evita duplicar estándares en ambos.
4. Añade `name` y `description` al YAML de cada `SKILL.md`.
5. Comprueba en VS Code que las instrucciones, los agentes y las skills se detectan como esperas: el soporte puede variar según versión, configuración y superficie de Copilot.

### Documentación oficial

- [Instrucciones personalizadas y compatibilidad](https://docs.github.com/en/copilot/reference/custom-instructions-support)
- [Uso de agentes personalizados en el IDE](https://docs.github.com/en/copilot/how-tos/copilot-in-your-ide/use-copilot-agents/use-custom-agents)
- [Creación de Agent Skills](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)
- [Prompt files en VS Code](https://code.visualstudio.com/docs/agent-customization/prompt-files)
- [Prompt files en GitHub Copilot Chat](https://docs.github.com/en/copilot/github-copilot-chat/customizing-github-copilot-chat/prompt-files)
