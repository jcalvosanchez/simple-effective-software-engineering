# Sesión 5: Recursos IA generativa (III): MCP — conecta Copilot con tu stack

Domina el recurso que conecta Copilot con sistemas externos: bases de datos, APIs, herramientas de tu stack

## Duración
1 hora

## Audiencia
- Desarrolladores software que han completado las sesiones 1-4

## Prerrequisitos
- [ ] Haber completado la [Sesión 4: Recursos IA generativa (II): skills y agentes personalizados](04-recursos-ia-02-skills-agentes.md)
- [ ] Traer tu `prompts.md`, `AGENTS.md` y la skill o agent que creaste (los usaremos en el calentamiento)

## Objetivos
Al finalizar la sesión, el alumno será capaz de:
- [ ] Explicar qué es MCP (Model Context Protocol) y qué problema resuelve.
- [ ] Identificar servidores MCP disponibles en su entorno y qué capacidades aportan.
- [ ] Usar un servidor MCP en una conversación para consultar un sistema externo.
- [ ] Evaluar cuándo una integración MCP es preferible a copiar/pegar contexto manualmente.

---

## Calentamiento (5 min)

Enseña la skill o el agent que creaste en la Sesión 4. ¿Lo has usado esta semana? ¿Se activó cuando esperabas?

---

## MCP: el puente entre Copilot y tus sistemas

**MCP (Model Context Protocol)** es un protocolo estándar que permite a Copilot conectarse con sistemas externos: bases de datos, APIs, herramientas de CI/CD, sistemas de tickets, documentación interna... cualquier cosa que exponga un servidor MCP.

Sin MCP, Copilot solo ve lo que tú le muestras: el código abierto, los ficheros que adjuntas, el contexto de la conversación. Con MCP, Copilot puede *consultar* sistemas externos directamente, sin que tú copies y pegues.

### Casos de uso

- Consultar el esquema de la base de datos sin abrir el cliente SQL
- Leer los logs de error del entorno de desarrollo sin cambiar de ventana
- Consultar el estado de un pipeline de CI/CD o los tickets del sprint
- Acceder a documentación interna o a la wiki del proyecto

### Cómo usarlo

**1. Qué es un servidor MCP:**
- Un proceso que expone capacidades (tools) a través del protocolo MCP.
- Puede ser local (tu máquina) o remoto (un servicio del equipo).
- Ejemplos: servidor MCP de PostgreSQL, de GitHub, de Slack, de filesystem...

**2. Cómo se configura:**
- Los servidores MCP se configuran en el IDE (Settings > GitHub Copilot > MCP Servers o similar según versión).
- Tu equipo o infraestructura puede proporcionar servidores preconfigurados.
- Una vez configurado, el servidor está disponible en todas tus conversaciones.

**3. Cómo se usa en una conversación:**
- No necesitas hacer nada especial: si el servidor MCP está activo, Copilot puede invocar sus tools cuando detecta que son relevantes.
- También puedes pedirlo explícitamente: "usa el servidor de base de datos para ver el esquema de la tabla users".

**4. Seguridad:**
- Los servidores MCP tienen permisos propios: un servidor de solo lectura no puede modificar la BD.
- Revisa qué servidores tienes activos y qué permisos tienen (regla 1 del [Nivel 0, Sesión 2](02-fundamentos-ia-swe.md#nivel-0---buenas-prácticas-de-jugar-seguro-a-trabajar-con-criterio)).

### Capacidades MCP: Tools, Resources y Prompts

Un servidor MCP expone tres tipos de capacidades que Copilot usa automáticamente:

| Capacidad | Qué es | Ejemplo |
|---|---|---|
| **Tools** | Acciones que Copilot invoca | `create_issue`, `search_code`, `list_pull_requests` |
| **Resources** | Datos estructurados en tiempo real (URIs) | `jira://sprint/current`, `jira://issue/PROJ-123` |
| **Prompts** | Workflows preconfigurados del servidor | `/analytics-dashboard`, `/performance-report` |

**Flujo transparente:** pides algo en lenguaje natural → Copilot detecta la capacidad relevante → la invoca → te devuelve el resultado. No necesitas saber que MCP existe.

**Ejemplo (GitHub MCP):**
- Tool: `create_issue` → "Crea issue para este bug con logs adjuntos"
- Resource: `jira://sprint/current` → "Lista bugs críticos del sprint"
- Prompt: `/performance-report` → genera reporte con datos reales del servidor

### Demo
1. **Baseline**: pide al chat "¿qué columnas tiene la tabla `orders`?" sin MCP. Muestra que Copilot no puede saberlo: solo ve el código, no la BD.
2. **Configura el servidor MCP** de PostgreSQL (o el que uses tu equipo).
3. **Repite la misma pregunta** en un chat nuevo.
4. **Compara**: ahora Copilot consulta el esquema real y responde con datos actualizados.

### Ejemplo

❌ **Sin MCP (copiar/pegar manual):**
> Tú: [copias el esquema de la tabla desde tu cliente SQL]
> Tú: "Con este esquema, genera la query para..."

✅ **Con MCP:**
> Tú: "¿Qué columnas tiene la tabla `orders` y cuál es el índice más usado?"
> Copilot: [consulta el servidor MCP de PostgreSQL y responde con datos reales]

### Tu turno (10 min)
1. Pregunta a tu TL o infraestructura qué servidores MCP están disponibles en tu entorno.
2. Si hay alguno configurado, úsalo en una conversación: pide información que normalmente buscarías en otra herramienta.
3. Si no hay ninguno, identifica un caso de uso donde MCP ahorraría tiempo: ¿qué consultas haces cada día que podrían automatizarse?

---

## Siguiente sesión

En la [Sesión 6](06-recursos-ia-04-recap.md) haremos un **repaso consolidado** de todos los recursos (prompts, instructions, skills, agents, MCP) y resolveremos dudas antes de pasar al framework RPI.
