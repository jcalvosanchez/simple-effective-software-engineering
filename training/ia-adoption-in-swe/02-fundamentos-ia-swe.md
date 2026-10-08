# Sesión 2: Fundamentos de IA generativa en Ingeniería de Software

Con las reglas de seguridad como base, recorreremos 4 niveles de menor a mayor autonomía de la IA: te sugiere → te responde → ejecuta contigo → planifica para ti.

## Duración
1 hora

## Audiencia
- Desarrolladores software sin experiencia previa con IA generativa

## Prerrequisitos
- [ ] Haber completado la [Sesión 1: GitHub Copilot - Tu primer asistente de IA](01-github-copilot-plugin.md) (incluida la sección "Tu turno: juega seguro")

## Objetivos
Al finalizar la sesión, el alumno será capaz de:
- [ ] Explicar 3 riesgos de seguridad y privacidad al usar IA generativa en el desarrollo de software.
- [ ] Enumerar 2 buenas prácticas para el uso de IA generativa en el desarrollo de software.
- [ ] Aceptar/rechazar/guiar sugerencias de `autocompletado`.
- [ ] Resolver dudas técnicas reales usando el Modo `Ask`.
- [ ] Utilizar el Modo `Agent` para ejecutar una tarea de refactorización multi-fichero, revisando y aprobando cada cambio.
- [ ] Utilizar el Modo `Plan` para analizar el impacto de un cambio en una API y generar un plan de migración.

---

## Nivel 0 - Buenas prácticas: de jugar seguro a trabajar con criterio

En la Sesión 1 aprendiste las [3 reglas de seguridad](01-github-copilot-plugin.md#3-antes-de-tu-primer-prompt-3-reglas-de-seguridad). Vamos primero a recordarlas:

1. **No compartas datos sensibles** (Sesión 1) → sigue vigente en cada prompt, cada fichero adjunto y cada sugerencia que aceptes: el autocompletado también envía contexto de tu código.
2. **Los créditos son limitados** (Sesión 1) → elige el modelo y nivel de esfuerzo adecuado a cada tarea: no uses un modelo de razonamiento profundo para renombrar una variable.
3. **La IA se equivoca** (Sesión 1) → revisa siempre el código generado antes de aceptarlo, y refactoriza y testea lo que incorpores a tu proyecto. El código que aceptas es tu responsabilidad, no de Copilot.

Y a extenderlas con dos prácticas de mejora continua que marcan la diferencia:
4. **Mantén el contexto coherente**: una conversación por tema. Si cambias de tarea, abre una nueva sesión; un historial mezclado degrada las respuestas.
   - **Cuándo abrir un chat nuevo**: al cambiar de tarea, de fichero o de objetivo. También si la conversación se alarga y las respuestas empiezan a degradarse (señal de contexto saturado).
   - **Cómo hacerlo sin perder el hilo (handoff)**: si necesitas continuidad, abre el chat nuevo con un resumen de 2-3 líneas de lo que venías haciendo y qué quieres conseguir.
   - **Qué pierdes al cambiar**: el historial de la conversación anterior — por eso el resumen de handoff es tu puente.
5. **Prompts claros y específicos**: contexto concreto, objetivo concreto. "Mejora esto" da malos resultados; "extrae esta lógica de validación a un método reutilizable con tests" da buenos resultados.

### Ejemplos: aprende a detectar el riesgo

**Regla 1 — Datos sensibles:**
❌ "Aquí está mi API key `ghp_x7...`, Da ¿por qué falla la autenticación?"
→ El secreto ya viajó a los servidores del proveedor. Aunque borres el mensaje, considérala comprometida: rota la clave.
✅ "Tengo un error 401 al llamar a la API con este header (token omitido). ¿Qué puede fallar?"
❌ "Dado este fichero de configuración {que contiene con credenciales}, ¿puedes generar un test unitario para este endpoint?"

**Regla 3 — La IA se equivoca:**
❌ Copiar una sugerencia de 30 líneas y commitear sin ejecutar.
→ Compila ≠ es correcto. Puede pasar los tests y aun así violar una regla de negocio que Copilot no conoce.
✅ Aceptar, ejecutar los tests, leer el diff línea a línea y *después* commitear.

**Regla 5 — Prompts claros:**
❌ "Mejora esto" (sin selección, sin contexto, sin criterio de "mejor")
→ Copilot adivinará: quizá optimice rendimiento cuando tú querías legibilidad.
✅ "Extrae esta validación a un método reutilizable y añade tests para los casos límite."

## Nivel 1 - Autocompletado de código

Autocompletado de **GitHub Copilot**, no el nativo de IntelliJ. 
- las sugerencias de Copilot aparecen como **texto gris fantasma** (pueden ser varias líneas)
- las de IntelliJ aparecen en un **popup con lista de opciones**.

### Casos de uso

- Completar implementaciones de interfaces y clases abstractas
- Generar código repetitivo (DTOs, mapeos, validaciones, builders)
- Sugerir parámetros de métodos y configuraciones
- Escribir consultas con Streams y JPA

### Cómo usarlo

**Qué ves**: mientras escribes, Copilot sugiere de forma proactiva texto gris fantasma en tu línea o en las siguientes.

**Cómo interactúas**:
- `Tab` para aceptar la sugerencia completa, `Ctrl+→` para aceptar palabra a palabra.
- `Esc` para rechazar.
- `Alt+]` / `Alt+[` para navegar entre sugerencias alternativas.
- Para ver hasta 10 alternativas a la vez, abre el *Completions Panel* desde el icono de Copilot.
- Los atajos pueden variar según la versión del plugin: verifícalos en `Settings > Keymap` buscando "Copilot".

**Cómo guiar las sugerencias** — Copilot decide qué sugerir a partir de:
- El **fichero actual** y las **pestañas abiertas** del editor.
- Los **nombres** que eliges: `calculateMonthlyInterest` genera mejores sugerencias que `calc`.
- Los **comentarios como prompts**: describe la intención y Copilot genera la implementación.

**Cómo controlarlo**: puedes desactivar el autocompletado temporalmente desde el icono de Copilot en la barra de estado (*Disable Completions*). Útil en demos o al escribir datos sensibles de prueba (regla 1 del Nivel 0).

**Coste**: el autocompletado es el consumo más barato de todos los niveles y está pensado para uso continuo; no lo desactives por miedo al gasto. Si tienes curiosidad, revisa tu consumo en *AI Credit usage* (Sesión 1).

### Ejemplo

```java
// Valida que un IBAN sea español: empieza por "ES", 2 dígitos de control y 20 dígitos de cuenta
```
→ Copilot generará el método completo bajo el comentario.

### Tu turno (5 min)
1. Escribe el comentario `// Método que formatea un importe en euros con separador de miles` y acepta la sugerencia con `Tab`.
2. Repite rechazando con `Esc` y navega alternativas con `Alt+]` hasta encontrar una que te guste más.
3. Compara: borra el comentario y empieza a escribir el método a mano. ¿Sugiere igual de bien sin la intención escrita?

## Nivel 2 - Modo `Ask` como Chat con consultor técnico

Es la forma más natural de interactuar y aprender.

En la [Sesión 1](./01-github-copilot-plugin.md) enviaste tus primeros prompts jugando. Aquí formalizamos cómo escribirlos eficientemente para usar el chat en modo Ask como un consultor técnico siempre disponible. La calidad de sus respuestas depende directamente de la calidad de tus prompts.

### Casos de uso

- Consultar sintaxis de los lenguajes de programación y frameworks del proyecto
- Explorar APIs de terceros y librerías
- Resolver dudas de arquitectura y patrones de diseño
- Comprender mensajes de error y stack traces
- Comparar alternativas antes de decidir (ej. "¿`Optional.orElseThrow` o excepción checked aquí?")

### Cómo usarlo

#### Anatomía de una buena pregunta

**Modo seguro**: el chat solo lee y responde; nunca modifica tu código. Es el sitio ideal para preguntar sin miedo (a diferencia del modo Agent del Nivel 3, que sí toca tus ficheros).

Lo que marca la diferencia es qué pones en cada prompt:

1. **Contexto**: selecciona el código relevante en el editor antes de preguntar, o adjunta ficheros con el clip / `#file`. Copilot responderá sobre *tu* código, no sobre un caso genérico.
2. **Problema**: qué ocurre y qué esperabas que ocurriera. Incluye el error exacto si lo hay.
3. **Formato deseado**: "solo código", "explicación paso a paso", "como si fuera junior".
4. **Iteración**: si la primera respuesta no sirve, refina: añade detalle, cambia el enfoque o pide aclaraciones. Preguntar bien es una conversación, no un disparo único.

#### Qué hacer con la respuesta

- Las respuestas con código incluyen botones para **copiar** o **insertar en el cursor**.
- **Verifica antes de incorporar** (regla 3 del Nivel 0): a veces Copilot inventa métodos o librerías que no existen (*alucinaciones*). Si no compila, desconfía y pídele que se corrija.

### Ejemplo

❌ **Pregunta mala**: "¿por qué falla mi código?"

✅ **Pregunta buena**: *(con el método `processOrder` seleccionado en el editor)*
> "Este método lanza `NullPointerException` cuando el pedido no tiene dirección de envío (stack trace adjunto). ¿Cómo lo gestiono siguiendo el patrón de validación del proyecto? Dame solo el código."

La buena pregunta tiene contexto (selección), problema concreto (excepción + condición) y formato ("solo el código").

### Tu turno (5 min)
1. Selecciona un método de tu proyecto que nunca hayas entendido del todo (todos tenemos uno) y pide al chat que te lo explique línea a línea.
2. Itera: pide la misma explicación "como si fuera junior" o con un diagrama de flujo en texto.
3. Compara las dos respuestas: ¿cuál te sirve más? ¿Por qué?

## Nivel 3 - Modo `Agent` ejecuta mientras tú diriges y supervisas

Con el selector del chat en modo `Agent`, describes una tarea completa en lenguaje natural y Copilot la ejecuta de forma autónoma: analiza tu proyecto, edita varios ficheros, crea clases nuevas, ejecuta comandos (compilar, tests) e itera si algo falla. Tú supervisas: revisas cada diff y apruebas cada comando antes de que se ejecute.

### Casos de uso

- "Crea el CRUD completo de la entidad `Customer` con sus tests unitarios"
- "Añade validación de entrada a todos los endpoints de este controller"
- "Migra esta clase de JUnit 4 a JUnit 5"
- "Extrae la lógica de cálculo de `OrderService` a una clase de dominio y actualiza los tests"

### Cómo usarlo: el bucle de supervisión

1. **Describe la tarea** de forma acotada: "Añade validación al `OrderController`" funciona mejor que "mejora la seguridad de la API". Adjunta con `#file` los ficheros clave para acotar dónde trabaja.
2. **Revisa el plan** que propone antes de ejecutar: ¿toca solo lo que pediste?
3. **Aprueba cada comando de terminal** leyéndolo antes: un comando mal dirigido es un incidente (reglas 1 y 3 del Nivel 0).
4. **Revisa el diff** de cada fichero modificado: busca lo que sobra (cambios que no pediste) tanto como lo que falta.
5. **Verifica**: ejecuta los tests. Si fallan, díselo ("el test X falla con Y") y observa cómo itera hasta arreglarlo.

**Red de seguridad**: haz commit de tus cambios *antes* de lanzar el Agent. Si el resultado no te convence, `git checkout .` y a empezar de nuevo. Con esta red, puedes experimentar sin miedo.

### Ejemplo

> "Crea la clase `IbanValidator` con un método que valide IBANs españoles y su test unitario con casos válidos e inválidos."

→ Copilot analiza el proyecto, crea la clase y el test, ejecuta los tests, y si alguno falla, corrige y reintenta. Tú apruebas los pasos y revisas el diff final.

### Tu turno (10 min)
1. Haz commit de tu estado actual (tu red de seguridad).
2. Lanza en modo Agent el prompt del ejemplo (o una tarea equivalente de tu proyecto).
3. Observa cómo planifica y edita; aprueba los pasos leyendo lo que propone.
4. Revisa el diff: ¿ha tocado algo que no pedías?
5. Ejecuta los tests. Si alguno falla, informa al Agent del error y observa cómo itera.

## Nivel 4 - Modo `Plan`: antes de actuar, piensa

El modo Plan añade un punto de control *antes* de la ejecución: describes una tarea grande y Copilot investiga tu proyecto y genera un **plan en pasos que puedes leer y editar**. Solo cuando el plan te convence, lo ejecutas (normalmente en modo Agent).

**¿Qué hace y qué no hace?**
- ✅ Investiga tu proyecto (lee ficheros, busca usos) y genera un plan en texto/markdown dentro del chat.
- ✅ Te deja editar el plan: pide cambios en la conversación o corrige los pasos directamente.
- ❌ **No modifica tu código**: la ejecución la haces tú pasando al modo Agent (botón *Start implementation* al final del plan), donde vuelve el bucle de supervisión del Nivel 3.

### Casos de uso

- Añadir una feature que toca 4-5 ficheros (ej. "integra `IbanValidator` en todos los servicios que validan cuentas")
- Refactorizar un módulo sin romper los tests
- Planificar la migración de una librería a una versión nueva
- Cualquier tarea que "te da pereza por grande": el plan la hace abordable

### Cómo usarlo

1. **Describe la tarea** con objetivos y restricciones: qué quieres conseguir y qué no debe cambiar.
2. **Revisa el plan generado**: Copilot habrá investigado el proyecto y propuesto los pasos. ¿El enfoque es correcto? ¿Falta algo?
3. **Edita el plan**: este es el diferencial frente al modo Agent. Corrige pasos, reordena, elimina lo que sobre. El plan es tuyo.
4. **Ejecuta con Agent**: una vez aprobado, el plan se ejecuta paso a paso con el bucle de supervisión del Nivel 3.

**Consejo**: guarda el plan generado como markdown en el repo. Sirve de documentación de la decisión y de contexto para futuras sesiones.

### Ejemplo

> "Integra `IbanValidator` en todos los servicios que reciben cuentas bancarias, sin cambiar las firmas públicas de los métodos y manteniendo los tests en verde."

→ Copilot genera un plan: ficheros afectados, orden de cambios, tests a actualizar. Lo revisas, ajustas y ejecutas.

### Tu turno
Abre este mismo fichero md y lanza en modo Plan:

> "Añade una sección 'Errores comunes' al final de cada nivel de `02-fundamentos-ia-swe.md`, con 2-3 errores típicos de un desarrollador que empieza a usar Copilot."
  
Revisa el plan: ¿propone tocar solo ese fichero? ¿Los errores que sugiere tienen sentido? Edita lo que no te convenza. No hace falta ejecutarlo: trae el plan a la Sesión 3 y comentamos si el enfoque era el adecuado.

---

## Cierre: ¿qué modo uso para qué?

| Modo | ¿Toca mi código? | Úsalo para... |
|---|---|---|
| **Autocompletado** | Sugiere, tú aceptas | Escribir código línea a línea |
| **Ask** | No | Preguntar, entender, explorar sin miedo |
| **Agent** | Sí, con tu supervisión | Tareas acotadas multi-fichero |
| **Plan** | No; genera un plan que luego ejecuta Agent | Tareas grandes donde quieres aprobar el enfoque antes |

Regla práctica: si la tarea cabe en una frase y toca 1-3 ficheros → Agent directo. Si es más grande o quieres controlar el enfoque antes de tocar código → Plan.
