# Sesión 6: Recursos IA generativa (IV): recap y consolidación

Consolida todos los recursos de personalización: prompts, instructions, skills, agents y MCP

## Duración
1 hora

## Audiencia
- Desarrolladores software que han completado las sesiones 1-5

## Prerrequisitos
- [ ] Haber completado la [Sesión 5: Recursos IA generativa (III): MCP](05-recursos-ia-03-mcp.md)
- [ ] Traer todos los artefactos creados: `prompts.md`, `AGENTS.md`, skills, agents

## Objetivos
Al finalizar la sesión, el alumno será capaz de:
- [ ] Elegir el recurso adecuado (prompt, instruction, skill, agent o MCP) para una necesidad concreta.
- [ ] Diagnosticar por qué un recurso no se activa o no funciona como esperaba.
- [ ] Combinar varios recursos en un flujo de trabajo real.
- [ ] Resolver dudas pendientes de las sesiones anteriores.

---

## Calentamiento (5 min)

En parejas: muestra un artefacto que creaste esta semana (prompt, skill, agent...). ¿Qué problema resolvió? ¿Qué mejorarías?

---

## El ecosistema completo

Los 5 recursos de personalización, resumidos:

| Recurso | Qué es | Quién lo activa | Úsalo para... |
|---|---|---|---|
| **Prompt guardado** | Una tarea concreta reutilizable | Tú, manualmente | Tareas puntuales que repites |
| **Instruction** | Una regla transversal | Copilot, siempre | Convenciones que deben cumplirse |
| **Skill** | Un procedimiento empaquetado | Copilot, cuando la tarea encaja | Procedimientos del equipo |
| **Custom Agent** | Un especialista con rol y herramientas | Tú, al elegir interlocutor | Trabajo especializado o con permisos acotados |
| **MCP** | Una conexión a un sistema externo | Copilot, cuando es relevante | Consultar datos o sistemas fuera del IDE |

Regla práctica: **tarea → prompt, regla → instruction, procedimiento → skill, especialista → agent, sistema externo → MCP**.

---

## Casos de diagnóstico: ¿qué está pasando aquí?

Escenarios reales para discutir en grupo:

| Escenario | Diagnóstico | Solución |
|---|---|---|
| "Mi skill no se activa nunca" | La descripción no coincide con cómo describes la tarea | Reescribe la descripción pensando en *cuándo* debe activarse |
| "Copilot no respeta mi AGENTS.md" | El fichero es demasiado largo o genérico | Recorta a lo esencial; reglas concretas, no aspiraciones |
| "El agent no puede editar ficheros" | Herramientas limitadas por diseño | ¿Es un bug o una feature? Revisa el principio de mínimo privilegio |
| "MCP no devuelve datos actualizados" | El servidor puede estar cacheando o desconectado | Verifica la conexión y los logs del servidor |
| "Mi prompt funciona en mi proyecto pero no en el de mi compañero" | Falta contexto del proyecto | Mueve el contexto a AGENTS.md o instructions |

---

## Ejercicio integrador: el flujo completo

**Escenario:** Tu equipo tiene un nuevo servicio de notificaciones. Debes:
1. Crear el endpoint REST (Agent)
2. Con las convenciones del equipo (AGENTS.md + instructions)
3. Generando los tests con la skill de testing del equipo
4. Consultando el esquema de BD real (MCP)
5. Documentando la decisión en un prompt guardado para futuros endpoints

**Tu turno (20 min):**
1. Elige una tarea real de tu proyecto que toque varios recursos.
2. Ejecuta el flujo completo: planifica (opcional), ejecuta con Agent, verifica con tests, documenta.
3. Anota: ¿qué recurso aportó más valor? ¿Cuál sobró?

---

## Cierre: hacia el framework RPI

Ya tienes todas las piezas: modos de interacción (Sesión 2), recursos de personalización (Sesiones 3-5). En la [Sesión 7](07-rpi.md) las combinaremos en un **método de trabajo completo**: Research, Plan, Implement.

Trae a la próxima sesión: una tarea real de tu backlog que quieras abordar con el framework RPI.
