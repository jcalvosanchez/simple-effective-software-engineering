# Sesión 4: Recursos IA generativa (II): skills y agentes personalizados

Domina los recursos que personalizan cómo trabaja la IA por ti: skills reutilizables y agentes especializados

## Duración
1 hora

## Audiencia
- Desarrolladores software que han completado las sesiones 1-3

## Prerrequisitos
- [ ] Haber completado la [Sesión 3: Recursos IA generativa (I): prompts e instrucciones](03-recursos-ia-01-prompts-instructions.md)
- [ ] Traer tu `prompts.md` y tu `AGENTS.md` de la Sesión 3 (los usaremos en el calentamiento)

## Objetivos
Al finalizar la sesión, el alumno será capaz de:
- [ ] Explicar qué es una Skill, cuándo se activa automáticamente y crear una nueva.
- [ ] Invocar una Skill existente en una conversación.
- [ ] Describir qué es un Custom Agent y en qué se diferencia de una Skill.
- [ ] Crear un Custom Agent en su proyecto.
- [ ] Elegir el recurso adecuado (prompt, instruction, skill o agent) para una necesidad concreta.

---

## Calentamiento (5 min)

Repaso en parejas: enseña a tu compañero el `prompts.md` y el `AGENTS.md` que creaste en la Sesión 3. ¿Ha cambiado algo en cómo trabajas esta semana?

---

## Skills: capacidades empaquetadas que se activan solas

Una **Skill** es un paquete reutilizable que enseña a Copilot a hacer una tarea específica de tu dominio: combina instrucciones, contexto y recursos en un fichero que la IA **activa automáticamente** cuando detecta que la tarea encaja — o que tú invocas explícitamente.

Piénsalo como la diferencia entre darle a alguien una orden ("haz X") y enseñarle un oficio ("cuando veas X, ya sabes qué hacer").

### Casos de uso

- Tareas repetitivas de tu dominio que requieren conocimiento específico: "genera un postmortem con el formato del equipo", "prepara las notas de release con nuestra plantilla"
- Estandarizar cómo la IA ejecuta una tarea compleja (no solo *qué* debe cumplir —eso es una instruction— sino *cómo* lo hace paso a paso)
- Compartir capacidades entre el equipo: una skill bien hecha funciona para todos

### Cómo usarlo

- Una skill vive en un fichero (típicamente `.github/skills/<nombre>/SKILL.md` o similar según el entorno) con:
  - **Descripción**: qué hace y cuándo debe activarse. Es la parte clave: de ella depende que Copilot la active automáticamente en el momento adecuado.
  - **Instrucciones**: los pasos a seguir, con el detalle que necesite la tarea.
  - **Recursos opcionales**: plantillas, ejemplos, scripts de apoyo.
- **Activación automática**: cuando describes una tarea que encaja con la descripción de la skill, Copilot la aplica sin que se lo pidas.
- **Invocación explícita**: también puedes pedirla directamente ("usa la skill de postmortem para...").

**Skill vs. instruction**: una instruction es una *regla* que siempre se cumple ("los tests usan JUnit 5"); una skill es un *procedimiento* que se ejecuta cuando toca ("cómo generar el postmortem del equipo"). Regla → instruction. Procedimiento → skill.

### Demo
1. **Baseline**: pide en el chat "genera el postmortem del incidente de ayer" sin ninguna skill. Muestra el resultado: formato genérico, inventado.
2. **Añade la skill**: crea la skill del ejemplo de abajo con la plantilla de postmortem del equipo.
3. **Repite la misma petición** en un chat nuevo.
4. **Compara**: ahora sigue la plantilla del equipo, y se ha activado sin mencionarla en el prompt.

### Ejemplo

✅ **`.github/skills/postmortem/SKILL.md`:**
```markdown
---
description: Genera un postmortem de incidente siguiendo la plantilla del equipo. Actívala cuando el usuario pida documentar un incidente, postmortem o retrospectiva de un fallo.
---

# Skill: Postmortem del equipo

Genera un postmortem con esta estructura:
1. **Resumen** (2-3 líneas): qué pasó, impacto, duración.
2. **Timeline**: detección → diagnóstico → mitigación → resolución.
3. **Causa raíz**: análisis de los 5 porqués.
4. **Acciones**: preventivas (qué no vuelva a pasar) y de detección (cómo enterarnos antes), cada una con responsable y fecha.
Tono: blameless, orientado a aprendizaje, sin señalar personas.
```

### Tu turno (15 min)
1. Identifica una tarea repetitiva de tu equipo que siga un procedimiento conocido (changelog, postmortem, onboarding de un servicio, revisión de PR...).
2. Crea su skill: escribe la descripción pensando en *cuándo debe activarse* y los pasos con el detalle justo.
3. Pruébala de las dos formas: invocándola explícitamente y con una petición que debería activarla sola. ¿Se activa cuando toca? Si no, ajusta la descripción.

---

## Custom Agents: un especialista con rol, herramientas y límites

Un **Custom Agent** es una configuración que convierte a Copilot en un especialista: defines su **rol** (qué es y qué sabe), sus **instrucciones** (cómo trabaja) y sus **herramientas permitidas** (qué puede tocar). A diferencia de una skill (un procedimiento que se activa dentro de cualquier conversación), un agent es un *interlocutor* al que eliges para una conversación completa.

### Casos de uso

- Un "revisor de seguridad" que solo lee código y busca vulnerabilidades, sin permiso para editar
- Un "experto en migraciones" que conoce las convenciones de BD del proyecto y solo toca los ficheros de migración
- Un "generador de tests" especializado en las convenciones de testing del equipo
- Acotar el riesgo: un agent con herramientas limitadas no puede ejecutar comandos de terminal aunque se lo pidas

### Cómo usarlo

- Se define en un fichero de configuración (típicamente `.github/agents/<nombre>.md` o similar según el entorno) con:
  - **Nombre y descripción**: qué es y para qué sirve.
  - **Instrucciones**: su rol, conocimiento y forma de trabajar.
  - **Herramientas permitidas**: lectura, edición, terminal... solo las que necesite.
- Se selecciona en el chat como interlocutor: toda la conversación ocurre con ese especialista.
- **Principio de mínimo privilegio**: dale solo las herramientas que necesita. Un revisor que no edita no puede romper nada (regla 3 del [Nivel 0, Sesión 2](02-fundamentos-ia-swe.md#nivel-0---buenas-prácticas-de-jugar-seguro-a-trabajar-con-criterio)).

**Agent vs. skill**: la skill es un *procedimiento* que cualquier conversación puede activar; el agent es un *especialista* al que eliges hablar. Si la tarea es "cuando toque X, hazlo así" → skill. Si es "para este tipo de trabajo, quiero hablar con este experto" → agent.

### Demo
1. **Baseline**: pide al chat general "revisa la seguridad de este endpoint". Muestra la respuesta: genérica, mezcla estilo y seguridad.
2. **Crea el agent**: define el `security-reviewer` del ejemplo de abajo (solo lectura).
3. **Repite la misma petición** seleccionando el agent.
4. **Compara**: enfoque exclusivo en seguridad, y muestra que si le pides "arréglalo", no puede editar: sus herramientas se lo impiden.

### Ejemplo

✅ **`.github/agents/security-reviewer.md`:**
```markdown
---
name: security-reviewer
description: Revisor de seguridad. Analiza código buscando vulnerabilidades (OWASP Top 10) y las reporta con severidad y recomendación. Solo lectura: nunca edita ficheros.
tools: [read, search]
---

Eres un revisor senior de seguridad. Al analizar código:
- Busca vulnerabilidades del OWASP Top 10: inyección, autenticación rota, exposición de datos, configuración insegura.
- Reporta cada hallazgo con: severidad (alta/media/baja), ubicación, explotación posible y recomendación concreta.
- No edites código: tu trabajo es reportar, no arreglar.
- Responde en español.
```

### Tu turno (15 min)
1. Piensa qué especialista le vendría bien a tu equipo (revisor de PRs, experto en BD, guardián de la API pública...).
2. Crea su agent: rol, instrucciones y —sobre todo— herramientas mínimas necesarias.
3. Mantén una conversación con él sobre código real de tu proyecto. ¿Se nota la especialización frente al chat general?

---

## Cierre: ¿qué recurso uso para qué?

Los 4 recursos de personalización, resumidos:

| Recurso | Qué es | Quién lo activa | Úsalo para... |
|---|---|---|---|
| **Prompt guardado** | Una tarea concreta reutilizable | Tú, manualmente | Tareas puntuales que repites (changelog, demo) |
| **Instruction** | Una regla transversal | Copilot, siempre | Convenciones que deben cumplirse en todo el código |
| **Skill** | Un procedimiento empaquetado | Copilot, cuando la tarea encaja (o tú explícitamente) | Procedimientos del equipo con pasos conocidos |
| **Custom Agent** | Un especialista con rol y herramientas | Tú, al elegir interlocutor | Trabajo especializado o con permisos acotados |

Regla práctica: **tarea → prompt, regla → instruction, procedimiento → skill, especialista → agent**.

Y recuerda: todos estos recursos son texto en ficheros de tu repo. Se versionan, se revisan en PR y mejoran con el tiempo — trátalos como código.

---

## Siguiente sesión

En la [Sesión 5](05-recursos-ia-03-mcp.md) veremos **MCP (Model Context Protocol)**: cómo conectar Copilot con sistemas externos como bases de datos, APIs y herramientas de tu stack.
