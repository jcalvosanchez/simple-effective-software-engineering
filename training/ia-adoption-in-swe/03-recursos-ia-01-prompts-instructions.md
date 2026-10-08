# Sesión 3: Recursos IA generativa (I): prompts e instrucciones

Domina los recursos que personalizan cómo te habla la IA: prompts reutilizables e instrucciones persistentes

## Duración
1 hora

## Audiencia
- Desarrolladores software sin experiencia previa con IA generativa

## Prerrequisitos
- [ ] Haber completado la [Sesión 2: Fundamentos de IA generativa en Ingeniería de Software](02-fundamentos-ia-swe.md) (incluida la sección "Tu turno" del Nivel 4: traer el plan generado)

## Objetivos
Al finalizar la sesión, el alumno será capaz de:
- [ ] Usar prompts predefinidos
- [ ] Guardar prompts propios en una prompt library para reutilizarlos y compartirlos.
- [ ] Definir Chat Instructions para dar contexto temporal a una conversación.
- [ ] Crear un fichero `*.instructions.md` con reglas del equipo acotadas por tipo de fichero.
- [ ] Explicar qué hace el fichero `AGENTS.md` de su proyecto y crear uno en un proyecto real.

---

## Prompts avanzados: predefinidos, reutilizar y compartir

Un `prompt` es la instrucción en lenguaje natural que envías a la IA para obtener una respuesta o acción. Es la unidad básica de interacción: todo lo que escribes en el chat de Copilot es un prompt.
- Ya escribes prompts desde la [Sesión 1](01-github-copilot-plugin.md).
- En la [Sesión 2](02-fundamentos-ia-swe.md#nivel-2---modo-ask-como-chat-con-consultor-técnico) aprendiste a escribirlo eficientemente (contexto + problema + formato + iteración).
- En esta sesión subimos un nivel: **prompts predefinidos** que Copilot ya trae empaquetados, y **cómo guardar los tuyos** para reutilizarlos y compartirlos.

> **Recuerda** las [buenas prácticas del Nivel 0 de la Sesión 2](02-fundamentos-ia-swe.md#nivel-0---buenas-prácticas-de-jugar-seguro-a-trabajar-con-criterio): prompts claros y específicos, contexto coherente, y ajusta el **modelo LLM** y el **nivel de esfuerzo** (Low/Medium/High) según la complejidad de la tarea.

### Casos de uso

- Usar un prompt predefinido de Copilot para una tarea común (explicar, arreglar, testear)
- Guardar un prompt que repites cada semana (ej. "genera el changelog de esta release")
- Compartir con el equipo los prompts que funcionan

### Cómo usarlo

**1. Prompts predefinidos (slash commands y @-mentions):**

| Comando | Qué hace | Cuándo usarlo |
|---|---|---|
| `@workspace` | Busca en todo el proyecto | No sabes qué ficheros son relevantes |
| `/explain` | Explica el código seleccionado | Entender código heredado |
| `/fix` | Propone arreglo para el error seleccionado | Tienes un error en pantalla |
| `/tests` | Genera tests para el código seleccionado | Cobertura rápida |

Son prompts empaquetados: mismo resultado que escribirlo tú, pero más rápido.

**2. Prompt library (guardar, reutilizar, compartir):**
- Cuando un prompt funciona bien, guárdalo: en un fichero `prompts.md` de tu proyecto, en un snippet del IDE, o en la documentación del equipo.
- Nómbralo con su propósito: no "prompt1", sino "generar-changelog-release".
- Usa **placeholders** (`{VERSION}`, `{FECHA}`) para lo que cambia en cada uso: conviertes el prompt en una plantilla.
- Comparte los buenos con el equipo: un prompt que ahorra 10 minutos a ti, ahorra 100 al equipo.

### Ejemplo

**Prompt efímero (se pierde):**
> "Genera el changelog de la versión 2.4.0 con los commits desde el 1 de octubre, agrupados por tipo de cambio"

**Prompt guardado en `prompts.md` (plantilla reutilizable):**
```markdown
## generar-changelog-release
Genera el changelog de la versión {VERSION} con los commits desde {FECHA}, agrupados por tipo de cambio (feat, fix, docs, refactor) usando el siguiente {FORMATO} de salida.
- {VERSION}: 2.4.0
- {FECHA}: 1 de octubre
- {FORMATO}: Markdown con encabezados y viñetas con enlaces a los PRs.
```

### Tu turno (5 min)
1. Usa `/explain` sobre un método de tu proyecto que no hayas escrito tú.
2. Escribe un prompt para una tarea que repitas cada sprint (ej. "prepara la demo", "revisa los logs de error"), con placeholders para lo que cambia en cada uso.
3. Guárdalo en un fichero `prompts.md` de tu proyecto con un nombre descriptivo.

## Instructions como reglas persistentes para la IA

Una `instruction` es una regla persistente que le dices a Copilot una vez y aplica siempre: estilo de código, convenciones del equipo, patrones de arquitectura.

A diferencia de un `prompt` (que escribes en cada conversación), o un `prompt` guardado (reutilizable, pero bajo demanda: lo invocas tú cuando lo necesitas), las instructions son persistentes (viven en ficheros del proyecto) y se aplican automáticamente a todas las respuestas y sugerencias dentro del contexto del proyecto.

### Ventajas
- Contexto persistente para todo el equipo
- Consistencia en el código generado
- Menos iteraciones y correcciones

### Tipos

| Tipo | Ubicación | Alcance |
|---|---|---|
| Chat Instructions | Prompt al inicio de un chat | Temporal: solo esa conversación |
| `*.instructions.md` | `.github/instructions/` | Persistente: Reglas del equipo que se aplican siempre (estilo, convenciones, patrones...). Pueden acotarse a rutas concretas (ej. solo `**/*.test.*`) |
| `AGENTS.md` | Raíz del proyecto | Persistente: Contexto del proyecto para agentes de IA (stack, arquitectura, comandos, workflow...) |

## Instructions > Chat Instructions: contexto temporal para una conversación

Instrucciones que escribes al inicio de una conversación y guían todas las respuestas subsiguientes. A diferencia de los ficheros de instructions (permanentes, de todo el proyecto), son temporales y específicas de una tarea o conversación concreta.

### Casos de uso

- Que la IA adopte un rol temporal: revisor de código, arquitecto, experto en seguridad
- Establecer restricciones específicas de una conversación (idioma, formato, framework de ejemplos)
- Trabajar en una feature con convenciones particulares (ej. "considera las reglas fiscales españolas")
- Debugging o análisis de código con un enfoque concreto

### Cómo usarlo

- En la ventana de chat de GitHub Copilot, escribe las instrucciones como primer mensaje de la conversación; aplicarán a todo lo que venga después.
- Sé explícito sobre rol, enfoque y formato: igual que un prompt efectivo, pero con alcance de conversación completa.
- Cuando cambies de tarea, abre un chat nuevo (regla 4 del [Nivel 0, Sesión 2](02-fundamentos-ia-swe.md#nivel-0---buenas-prácticas-de-jugar-seguro-a-trabajar-con-criterio)): la Chat Instruction anterior deja de aplicar.

### Demo
1. **Baseline**: abre un chat nuevo y pregunta: "Revisa este método" (selecciona un método del proyecto). Muestra la respuesta: genérica, sin enfoque concreto.
2. **Añade la instruction**: abre otro chat y empieza con: "Actúa como revisor senior de código Java, enfócate en performance y seguridad. Responde en español."
3. **Repite la misma pregunta** sobre el mismo método.
4. **Compara en voz alta**: ¿qué cambió? (enfoque de seguridad/performance, idioma, tono de revisor). La pregunta era idéntica; la diferencia es solo la instruction.

### Ejemplo

> "Actúa como revisor senior de código Java, enfócate en performance y seguridad. Responde siempre en español y sugiere mejoras siguiendo los principios SOLID."

### Tu turno (5 min)
1. Abre un chat nuevo y define una Chat Instruction con un rol temporal (ej. "revisor de seguridad").
2. Pide que analice un método tuyo. ¿Cambia el enfoque de la respuesta respecto a una pregunta sin rol?
3. Abre otro chat sin la instrucción y haz la misma pregunta. Compara.

## Instructions > Ficheros `*.instructions.md`: reglas del equipo

Definen estilo, convenciones y reglas del equipo para GitHub Copilot. Se aplican automáticamente y pueden acotarse por tipo de fichero o ruta.

### Casos de uso

- Que Copilot respete las convenciones del equipo sin repetirlas en cada prompt (naming, formato de respuestas API, manejo de errores)
- Reglas específicas por tipo de fichero: tests, migraciones de BD, configuración
- Establecer restricciones explícitas (DO/DON'T): "no uses librerías de fechas distintas de `java.time`"

### Cómo usarlo

- Crea el fichero en `.github/instructions/` con nombre descriptivo: `java-style.instructions.md`, `tests.instructions.md`.
- Usa el frontmatter `applyTo` para acotar dónde aplica (ej. `**/*.test.java`); sin él, aplica a todo el proyecto.
- Escribe reglas concretas y accionables, no aspiraciones: "usa `Optional` como tipo de retorno en finders" funciona; "escribe código limpio" no.

### Demo
1. **Baseline**: con el proyecto sin `.github/instructions/`, pide en el chat: "Genera un test para este método". Muestra la respuesta: estilo genérico (quizá JUnit 4, nombres tipo `testX`, asserts básicos).
2. **Añade el fichero**: crea `.github/instructions/tests.instructions.md` con las reglas del ejemplo de abajo.
3. **Repite la misma petición** en un chat nuevo.
4. **Compara**: el test generado ahora sigue las convenciones del equipo (JUnit 5, naming `should_..._when_...`, AssertJ) sin que se lo hayas pedido en el prompt.

### Ejemplo

✅ **`.github/instructions/tests.instructions.md`:**
```markdown
---
applyTo: "**/*.test.java"
---
- Tests con JUnit 5 y AssertJ; prohibido JUnit 4.
- Nombres de test: `should_<comportamiento>_when_<condición>`.
- Un `assert` lógico por test; usa `@ParameterizedTest` para casos múltiples.
```

### Tu turno (5 min)
1. Crea `.github/instructions/` en tu proyecto y añade un fichero con 3-4 reglas reales de tu equipo.
2. Pide a Copilot código que antes generaba "a su manera" y comprueba si ahora respeta las reglas.

## Fichero `AGENTS.md`: contexto del proyecto

Archivo de configuración en la raíz del proyecto que da contexto específico a los agentes de IA **sobre el proyecto**: cómo quieres que trabajen, qué patrones seguir, qué nomenclatura usar y qué convenciones respetar.

### Casos de uso

- Dar contexto del proyecto a un compañero nuevo... y a la IA: stack, arquitectura, cómo levantar el entorno
- Documentar comandos esenciales no obvios (migraciones, seeds, credenciales de desarrollo)
- Checklist para tareas comunes: añadir una entidad, crear un endpoint
- Pitfalls específicos del proyecto y su troubleshooting

### Cómo usarlo

- Ubicación: raíz del proyecto. Formato: Markdown.
- No debe ser extenso: contiene lo necesario para que la IA trabaje como quieres. Si supera una página, recorta.
- **Generación automática desde VS Code**: Copilot puede generar un fichero base adaptado a tu proyecto; úsalo como punto de partida y adáptalo.
- Contenido esencial:
  - **Contexto del proyecto** (5-10 líneas): stack tecnológico, propósito del sistema, arquitectura general.
  - **Patrones críticos (must-follow)**: formato de respuestas API, validación y manejo de errores, patrones que difieren de prácticas comunes.
  - **Workflow de desarrollo**: comandos esenciales no obvios, migraciones/seeds, cómo levantar el entorno local.
  - **Guías de implementación**: checklist para tareas comunes, estructura de archivos, ejemplos del código existente.
  - **Errores comunes + soluciones**: pitfalls específicos del proyecto y troubleshooting con comandos concretos.

### Demo
1. **Baseline**: en un proyecto sin `AGENTS.md`, pide en modo Agent: "Crea un endpoint nuevo para listar pedidos". Muestra el resultado: respuesta sin `ApiResponse`, errores genéricos, estilo por defecto.
2. **Añade el fichero**: crea el `AGENTS.md` del ejemplo de abajo en la raíz.
3. **Repite la misma petición** (deshaz antes los cambios del paso 1 con `git checkout .`).
4. **Compara los diffs**: el nuevo código respeta los patrones must-follow y usa los comandos del workflow documentados, sin repetir contexto en el prompt.

### Ejemplo

❌ **Repetir el contexto en cada prompt:**
> "Genera el endpoint, pero recuerda: usamos DTOs de respuesta envueltos en `ApiResponse`, los errores van con `ProblemDetail`, nombres en camelCase y respuestas en español..."

✅ **`AGENTS.md` en la raíz del proyecto:**
```markdown
# Proyecto: API de Pedidos
Stack: Java 21, Spring Boot 3, Maven, PostgreSQL.

## Patrones must-follow
- Toda respuesta de API va envuelta en `ApiResponse<T>`.
- Errores con `ProblemDetail` (RFC 7807), nunca excepciones sin capturar.
- Fechas solo con `java.time`; prohibido `java.util.Date`.

## Workflow
- Levantar entorno: `docker compose up` + `./mvnw spring-boot:run`.
- Migraciones: `./mvnw flyway:migrate`.
```

### AGENTS.md vs README.md

Regla práctica: si un humano nuevo lo necesita → README. Si la IA lo necesita para trabajar bien → AGENTS.md. No repitas; referencia si es necesario.

| | README.md | AGENTS.md |
|---|---|---|
| **Audiencia** | Humanos | Agentes de IA |
| **Propósito** | Presentar el proyecto: qué es, cómo instalar, cómo contribuir | Guiar el trabajo de la IA: patrones, comandos, pitfalls |
| **Contenido** | Marketing + documentación | Instrucciones operativas |

### Tu turno (10 min)
1. Genera (o escribe) un `AGENTS.md` en la raíz de un proyecto real tuyo con las 5 secciones de contenido esencial. Máximo una página.
2. Pide a Copilot una tarea que antes requería repetir contexto (ej. "crea un endpoint nuevo") y comprueba si respeta tus patrones sin que se los repitas. 

---

## Siguiente sesión

En la [Sesión 4](04-recursos-ia-02-skills-agentes.md) veremos **Skills** y **Custom Agents** para personalizar cómo trabaja la IA por ti.
