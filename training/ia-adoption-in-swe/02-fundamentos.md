# Sesión 2: Fundamentos de IA generativa en SwE

## Duración
1 hora

## Audiencia
- Desarrolladores software sin experiencia previa con IA generativa
- Preferible: ya tienen IntelliJ IDEA instalado y el plugin de GitHub Copilot configurado

## Objetivos
- [ ] Autocompletado
- [ ] Modo Ask
- [ ] Modo Agente
- [ ] Modo Plan

---

## Nivel 1 - Autocompletado de código

Autocompletado de **GitHub Copilot**, no el nativo de IntelliJ.

### Casos de uso

- Completar implementaciones de interfaces y clases abstractas​
- Generar código repetitivo (DTOs, mapeos, validaciones)​
- Sugerir parámetros de métodos y configuraciones​
- Escribir consultas LINQ y Entity Framework​

### Cómo usarlo

- **Aparición de sugerencias**: Las sugerencias aparecen de forma proactiva mientras escribes código, mostrándose directamente en tu línea de código o en un pequeño recuadro flotante.
  - Comentarios como "prompts" para guiar sugerencias.
- **Aceptar sugerencias**:
  - `Tab` para aceptar, `Ctrl+→` para palabra a palabra.
  - `Esc` para rechazar.
- **Sugerencias alternativas**: Para ver otras opciones de autocompletado, presiona Ctrl + Espacio (o Cmd + Espacio en Mac) o el atajo de tu IDE, lo que abrirá una lista de sugerencias.
  - Navegar entre alternativas (`Alt+]` / `Alt+[`).
- **Coste**: cada sugerencia inline consume tokens/créditos del plan de Copilot.

## Nivel 2 - Chat como consultor técnico

Comienza tu viaje con Copilot usando el chat como un consultor técnico siempre disponible. Es la forma más natural de interactuar y aprender.

### Casos de uso

- Consultar sintaxis de los lenguajes de programación usados en el proyecto
- Explorar APIs de terceros y librerías
- Resolver dudas de arquitectura
- Comprender mensajes de error

### Consejos prácticos para el chat en el IDE

- **Preguntas efectivas**: Sé específico, incluye el contexto relevante del código y el problema que intentas resolver.
- **Modo limitado**: No puede hacer acciones sobre el proyecto, no tiene permisos de escritura.
- **Mejores prácticas**: Pide ejemplos de código, explica lo que ya intentaste y especifica el formato de respuesta deseado (ej. "solo código", "explicación paso a paso").
- **Aprovechar el contexto**: Selecciona código en el editor antes de preguntar para que Copilot use esa información. Menciona archivos relacionados en tu consulta.
- **Refinar y iterar**: Si la respuesta inicial no es útil, pide aclaraciones o proporciona más detalles. Prueba diferentes enfoques o haz preguntas de seguimiento.

## Nivel 3 - Modo Agente

### Qué es el modo Agente

El modo Agent en GitHub Copilot es una capacidad avanzada que permite a la IA comprender descripciones en lenguaje natural y generar bloques de código más extensos y complejos.

A diferencia de las sugerencias línea por línea, el Agent puede construir funciones, clases y estructuras completas basándose en el contexto y tu intención.

### Casos de uso

- Generación de funciones completas
- Creación de clases con múltiples métodos
- Implementación de patrones de diseño
- Generación de código boilerplate
- Refactorización de código existente

### Consejos de uso:
- Escribe comentarios descriptivos antes de generar código.
- Usa `//` para activar sugerencias contextuales.
- Revisa siempre el código generado antes de aceptarlo.
- Combina con el chat para aclaraciones.
- Aprovecha el contexto de archivos abiertos.

## Nivel 4 - Modo Plan -> Planificación de tareas complejas
En este nivel, Copilot ayuda a **diseñar antes de construir**: descompone problemas complejos, sugiere arquitecturas y crea planes de implementación detallados.

**Fases del plan:**
1. **Definición**: describe el problema de alto nivel y los objetivos del proyecto.
2. **Desglose**: Copilot sugiere componentes, capas y división de responsabilidades.
3. **Implementación**: genera código base para cada componente siguiendo el plan.

**Casos de uso estratégicos:**
- Diseñar arquitectura de microservicios.
- Planificar migración de aplicaciones legacy.
- Estructurar proyectos multi-tenant.
- Definir estrategia de testing end-to-end.

**Consejos de uso:**
- Empieza con una descripción clara del problema.
- Define objetivos y restricciones antes de pedir el plan.
- Revisa la arquitectura sugerida antes de implementar.
- Itera sobre el diseño si no se ajusta a tus necesidades.
- Documenta las decisiones arquitectónicas importantes.
