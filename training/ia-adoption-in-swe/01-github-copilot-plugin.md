# Sesión 1: GitHub Copilot Plugin en IntelliJ

## Duración
1 hora

## Audiencia
- Desarrolladores software sin experiencia previa con IA generativa
- Preferible: ya tienen IntelliJ IDEA instalado

## Objetivos
- [ ] Configurar el plugin de GitHub Copilot en IDE
- [ ] Dominar los flujos básicos de interacción

---

## 1. Instalación y configuración plugin GitHub Copilot en IntelliJ IDEA
1. Instalar plugin **GitHub Copilot** desde **Settings/Preferences > Plugins > Marketplace**.
2. Reiniciar el IDE
3. Autorizar la cuenta de GitHub.

## 2. Overview del plugin GitHub Copilot

![Panel de GitHub Copilot Chat en IntelliJ IDEA](images/github-copilot-plugin-intellij.png)

### Barra superior
- **Reloj (historial)**: abre el historial de sesiones recientes.
- **AI Credit usage**: muestra el consumo de créditos de IA de tu plan (usados vs. disponibles) y el ciclo de facturación. Útil para controlar el gasto de las sesiones.
- **`+` (nueva sesión)**: inicia una conversación nueva, limpiando el contexto actual.
- **Customizations**: acceso a las personalizaciones de Copilot: instrucciones del agente, prompts personalizados, modos de chat y MCP servers.
- **Menú `⋮`**: opciones adicionales de visualización.
- **Botón `—`**: oculta el panel del plugin.

### Sección "Sessions"
- Lista de conversaciones recientes con su coste en **Credits** y antigüedad; permite reanudar sesiones anteriores.
- Iconos de la barra de sesiones: **buscar** sesiones, **filtrar**, **refrescar** la lista y alternar la vista del panel lateral.
- **Archivar** en cada sesión: archiva la conversación.
- **More >**: expande el historial completo.

### Zona de prompt (parte inferior)
- **Clip (añadir contexto)**: adjunta imágenes o ficheros como contexto.
- **Chip de fichero** (ej. `01-fundamentos-github-copilot.md`): contexto explícito añadido; el `+` permite añadir más ficheros/símbolos (`#file`).
- **Campo "Ask Copilot"**: donde escribes el prompt.
- **Selector de modo (`Agent/Ask/Plan`)**: cambia entre **Ask** (solo preguntar, sin modificar código), **Edit** (edición dirigida) y **Agent** (ejecuta tareas multi-paso modificando el proyecto).
- **Selector de modelo** (ej. `Kimi K3`): elige el LLM a usar según la tarea (velocidad vs. razonamiento).
- **Selector de esfuerzo** (ej. `Low`): nivel de razonamiento del modelo (Low/Medium/High); más esfuerzo = mejor razonamiento pero más coste y latencia.
- **Botón enviar (`↑`)**: envía el prompt.
- **Entorno de ejecución** (`Local/GitHub/Cloud`): define dónde ejecutar el código y los cambios generados.

#### Modos de ejecución: Ask, Agent, Plan
- **Ask**: responder dudas, explicar código o proponer ideas **sin modificar archivos**. Úsalo cuando no quieras realizar cambios.
- **Agent / Edits**: aplicar cambios directamente en el proyecto.
- **Plan**: descomponer tareas grandes en pasos antes de ejecutar.

## 3. Buenas prácticas de uso
- No compartir datos sensibles.
- Elegir el modelo y nivel de esfuerzo adecuado según la tarea.
- Revisar siempre el código generado.
- Prompts claros y específicos.
- Mantener contexto coherente.
- Refactorizar y testear lo generado.
