# Sesión 3: Recursos IA generativa: instrucciones, prompts, skills, agentes, mcp

## Duración
1,5 horas

## Audiencia
- Desarrolladores software sin experiencia previa con IA generativa
- Preferible: ya tienen IntelliJ IDEA instalado

## Objetivos
- [ ] Instrucciones
- [ ] Prompts
- [ ] Skills
- [ ] Agentes
- [ ] MCP

---

## Instructions

### Fichero `*.instructions.md`
- Ubicación: `.github/instructions/*.instructions.md`.
- Define estilo, convenciones y reglas del equipo para GitHub Copilot.
- Ejemplo práctico de instrucciones personalizadas.

### Fichero `AGENTS.md`
Archivo de configuración que da contexto específico a GitHub Copilot **sobre el proyecto**. Define cómo quiere la IA que trabaje, qué patrones seguir, qué nomenclatura usar y qué convenciones respetar.
- Nombre del archivo: `AGENTS.md`.
- Ubicación: raíz del proyecto.
- Formato: Markdown (`.md`).
- Buenas prácticas: no debe ser extenso; debe contener lo necesario para que trabaje como quieres.
- **Generación automática desde VS Code:** GitHub Copilot en VS Code puede generar automáticamente un fichero instructions base adaptado a tu proyecto.

### Ventajas de usar Instructions
- Contexto persistente para todo el equipo.
- Consistencia en el código generado.
- Respeta tus patrones de arquitectura.
- Mantiene tu nomenclatura y convenciones.
- Reduce iteraciones y correcciones.
- Mejora la calidad del código generado.

### Contenido esencial
- Contexto del Proyecto (5-10 líneas): Stack tecnológico principal. Propósito del sistema Arquitectura general
- Patrones Críticos (must-follow): Formato de respuestas API estandarizado. Convenciones de código específicas del proyecto. Sistema de validación y manejo de errores. Patrones que difieren de prácticas comunes.
- Workflow de Desarrollo: Comandos esenciales no obvios. Operaciones de base de datos (migraciones, seeds). Cómo levantar el entorno local. Credenciales de desarrollo
- Guías de Implementación Checklist para tareas comunes (añadir entidades, endpoints). Estructura de archivos y dónde va cada cosa. Ejemplos concretos del código existente.
- Preferencias del Desarrollador: Estilo de código (naming, imports, async patterns). Cómo quieres las respuestas de la IA. Restricciones explícitas (DO/DON'T) Idioma de respuesta y formato.
- Errores Comunes: + Soluciones Pitfalls específicos del proyecto. Troubleshooting con comandos concretos

### Chat Instructions: contexto temporal para conversaciones
Las Chat Instructions son instrucciones específicas que se aplican únicamente durante una conversación activa con GitHub Copilot Chat. A diferencia del fichero `.github/copilot-instructions.md`, que es permanente, estas instrucciones son temporales y se utilizan para dar contexto específico a una tarea o conversación concreta.

**Cuándo usar Chat Instructions:**
- Tareas puntuales que requieren contexto específico.
- Cuando necesitas que la IA adopte un rol temporal (revisor de código, arquitecto, etc.).
- Para establecer restricciones específicas de una conversación.
- Cuando trabajas en una feature que requiere convenciones particulares.
- Para debugging o análisis de código específico.

**Cómo definirlas:**
- En la ventana de chat de GitHub Copilot, establece las instrucciones al inicio de la conversación; guiarán todas las respuestas subsiguientes.

**Ejemplos de uso:**
- "Actúa como revisor senior de código C#, enfócate en performance y seguridad".
- "Responde siempre en español y usa ejemplos con Entity Framework".
- "Estoy trabajando en el módulo de facturación, considera las reglas fiscales españolas".
- "Sugiere mejoras siguiendo los principios SOLID".
- "Analiza este código buscando vulnerabilidades de seguridad".

**Diferencias clave:**
- **Chat Instructions**: temporales, específicas de la conversación actual.
- **Copilot Instructions (.md)**: permanentes, aplican a todo el proyecto.
