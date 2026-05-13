# PROMPT 4 — Plan de Historias de Usuario

> Antes de leer este prompt, carga `../context-greta.md`, `../outputs/GRETA-research.md`, `../outputs/GRETA-discovery.md` y `../outputs/GRETA-architecture.md`. Este prompt asume todo ese contexto.

---

Actúa como agente de planificación.

Lee el documento maestro completo. Tienes el contexto de GRETA, los requisitos, el discovery y la arquitectura definida. Úsalos todos.

Tu objetivo es crear el plan completo de Historias de Usuario (HUs) listas para ser ejecutadas por un equipo de desarrollo, organizadas en épicas, priorizadas y con criterios de aceptación completos.

FORMATO DE CADA HU:
- **ID:** [ÉPICA]-[NÚMERO] (ej: AUTH-001)
- **Título:** breve y descriptivo
- **Historia:** Como [actor], quiero [acción], para [valor/objetivo]
- **Criterios de aceptación:** lista de condiciones verificables (mínimo 3, máximo 8)
- **Dependencias:** IDs de HUs que deben estar completas antes
- **Estimación:** XS / S / M / L / XL (complejidad relativa, no horas)
- **Notas técnicas:** consideraciones de implementación relevantes según la arquitectura

ÉPICAS REQUERIDAS:

**AUTH — Autenticación y gestión de instancia**
Registro, creación automática de instancia, login, gestión de sesión, configuración de cuenta, borrado completo de cuenta y datos (GDPR).

**PEOPLE — Gestión de equipos y personas**
Crear/editar/archivar equipos, dar de alta personas, gestionar la relación persona-equipo (una persona puede estar en varios equipos), campos personalizados por persona.

**PROFILE — Ficha de persona**
CV upload y procesamiento con IA, logros y contribuciones, campos adicionales configurables, historial de documentos procesados, sugerencias de preguntas generadas por la IA.

**ASSESSMENT — Diagnóstico y niveles**
Assessment manual de las 4 dimensiones con los 7 niveles y colores, historial de assessments por persona, comparación entre assessments, justificación de nivel con comportamientos observados.

**AI — Procesamiento con IA**
Subida de transcripción o resumen de conversación, pipeline RAG completo, presentación de sugerencias con razonamiento y citas del texto, validación/ajuste/rechazo por quien lidera, historial de sugerencias y validaciones.

**VIZ — Visualización**
Radar chart por persona (4 dimensiones, 7 colores), evolución temporal por persona, dashboard de equipo (cobertura de roles, bus factor, score de salud), mapa de calor de conocimiento por equipo, comparativa entre equipos.

**GRAPH — Vista de grafo**
Visualización interactiva de las relaciones entre personas, equipos y proyectos con los colores de nivel de cada dimensión seleccionable, filtros por dimensión y nivel, zoom y navegación.

**SHARE — Compartición de vistas**
Crear vista compartible (selección de contenido), generar link de acceso temporal o permanente, vista read-only para el destinatario sin acceso a la app, gestionar y revocar accesos compartidos.

**INFRA — Infraestructura y operaciones**
Provisioning automático de instancia nueva, cifrado y gestión de claves, backup y recuperación por instancia, monitoring y alerting, pipeline CI/CD, borrado completo GDPR.

TAREAS DE PLANIFICACIÓN:

1. Crea todas las HUs para cada épica con el formato definido
2. Identifica el MVP mínimo — el subconjunto de HUs que permite validar el valor central del producto con una persona real que lidera un equipo real
3. Identifica una versión Beta — el subconjunto que añade la capa de IA y las visualizaciones principales
4. Propón una secuencia de sprints de 2 semanas (asumiendo equipo de 2-3 developers + 1 diseñadora/diseñador) con las HUs agrupadas por sprint
5. Identifica las HUs de mayor riesgo técnico y propón spikes de investigación donde sea necesario
6. Lista las dependencias externas (APIs de terceros, servicios cloud, credenciales) que deben estar configuradas antes de empezar el desarrollo

OUTPUT ESPERADO:
- Lista completa de HUs organizadas por épica con todos los campos
- Definición del MVP con justificación y lista de HUs incluidas
- Definición de la Beta con lista de HUs adicionales
- Plan de sprints con HUs por sprint y objetivo de cada sprint
- Lista de spikes técnicos recomendados con duración estimada
- Checklist de dependencias externas y configuración previa necesaria

---

**Output destination:** `outputs/GRETA-HUs.md` (en la raíz de greta-app/).
