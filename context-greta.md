# GRETA SaaS — Contexto base

Este fichero contiene **Partes 1 y 2** del documento maestro: qué es el framework GRETA + qué requisitos tiene el SaaS. Es el contexto que CADA prompt (research, discovery, architecture, HUs) debe tener cargado antes de ejecutarse.

> Fuente original: `master.md`. No editar este fichero a mano salvo que se actualice también el master.

---

## PARTE 1 — CONTEXTO: QUÉ ES GRETA

GRETA (Guía de Referencia de Equipos de Trabajo Afectivo) es un framework de diagnóstico y composición de equipos de ingeniería basado en cuatro dimensiones. Su propósito es dar a quien lidera una lectura estructurada de su equipo — no como una foto de competencias individuales, sino como un sistema vivo con gaps, fortalezas y dinámicas propias.

GRETA no es un cuestionario ni una herramienta de evaluación de personas. Es una guía de lectura para quien lidera. Las personas son la estrategia.

### Las cuatro dimensiones

**Dimensión 1 — Rol de contribución**
Describe cómo aporta cada persona al equipo. Utiliza la taxonomía de 9 roles de Belbin como referencia interna (Cerebro/PL, Monitor evaluador/ME, Especialista/SP, Coordinador/CO, Investigador de recursos/RI, Cohesionador/TW, Impulsor/SH, Implementador/IMP, Finalizador/CF). El nivel describe la consciencia y flexibilidad con que se ejerce el rol natural.

**Dimensión 2 — Conocimiento**
Describe qué sabe el equipo colectivamente. Cubre áreas técnicas agrupadas en: Ingeniería, Bases de datos, APIs y comunicación, Frontend, Operaciones, Seguridad, Arquitectura, Calidad e IA aplicada. Cada área tiene un bus factor (número de personas que la dominan). Cada persona tiene un nivel de dominio por área: básico / en progreso / sólido. Y un perfil global: I-shape, T-shape, π-shape o Comb.

**Dimensión 3 — Seniority**
Describe la madurez profesional — no el título organizativo. Se construye sobre tres capacidades: anticipación, resiliencia ejecutiva y criterio bajo incertidumbre. El criterio no son los años sino los contextos: proyectos, empresas, tipos de problemas, situaciones de crisis. El CV de cada persona (especialmente orientado a proyectos y retos) es un input relevante para esta dimensión.

**Dimensión 4 — Emocional**
Describe cómo está cada persona dentro del equipo. Tres subdimensiones: motivación (intrínseca vs extrínseca), alineamiento (¿comparte el propósito?) y metaconsciencia (¿tiene consciencia de cómo afecta al sistema?). Es la más dinámica y la más exigente para quien lidera. Requiere tiempo, relación y múltiples observadores para ser fiable.

### La escala de niveles (transversal a las 4 dimensiones)

Cada persona tiene un nivel independiente en cada dimensión. Los siete niveles con sus colores:

| Color | Nivel | Grupo | Descripción |
|-------|-------|-------|-------------|
| 🔴 Rojo | Tiro | Ejecuta con guía | Empieza, necesita estructura |
| 🟠 Naranja | Novicius | Ejecuta con guía | Gana soltura, autonomía parcial |
| 🟡 Amarillo | Peritus | Ejecuta con autonomía | Autónomo en lo conocido |
| 🟢 Verde | Expertus | Ejecuta con autonomía | Criterioso, empieza a anticipar |
| 🔵 Azul | Veteranus | Decide y anticipa | Referente en su área |
| 🟣 Violeta | Primus | Decide y anticipa | Multiplica a otros |
| ⚪ Blanco | Magister | Transforma | Nivel aspiracional |

Los colores son fundamentales para la visualización: tonos cálidos = niveles iniciales, tonos fríos = niveles avanzados. Un mapa de equipo todo en tonos cálidos indica homogeneidad en niveles bajos. La mezcla de colores indica diversidad saludable.

### Los tres escenarios operativos

1. **Heredar** un equipo ya formado y leer lo que hay
2. **Formar** un equipo nuevo desde el mapa de gaps
3. **Modificar** un equipo que no está funcionando

### Principios críticos de uso (relevantes para el diseño de la app)

- Quien lidera gestiona GRETA con transparencia y criterio — puede compartirlo, pero los niveles individuales comparativos no se publican al equipo para evitar competición
- El seguimiento frecuente es el método — la app debe facilitar la observación continua
- En la conversación nunca se habla de niveles — se habla de comportamientos concretos
- Los niveles no son jerarquía — son etiquetas de tendencia, no juicios permanentes
- Cada persona puede estar en niveles distintos en cada dimensión
- La subjetividad no se elimina — se gestiona con múltiples observadores y autocrítica
- El diagnóstico caduca — la app debe facilitar la actualización continua

### Inputs para el diagnóstico (relevantes para la app)

- Observación directa (notas del líder)
- Transcripciones o resúmenes de conversaciones (O2O)
- CV y trayectoria de cada persona (procesable con IA)
- Logros y contribuciones documentados
- Cuestionario Belbin formal (opcional)
- Feedback de compañeros

---

## PARTE 2 — REQUISITOS DEL SAAS GRETA

### Modelo de negocio
- SaaS con instancias independientes por persona que lidera
- Una persona que lidera = una instancia privada y aislada
- El SaaS crea la instancia automáticamente al registrarse
- Al crear la instancia se configuran equipos y personas iniciales
- Escalable hacia arriba y hacia abajo en cualquier momento
- Posibilidad de compartir vistas específicas (read-only) con terceros — CEO, otras personas con rol de liderazgo — sin darles acceso a la app completa
- Producto web y móvil

### Privacidad y seguridad
- Datos encriptados en reposo y en tránsito
- Privacidad total por instancia — ninguna instancia ve datos de otra
- Las transcripciones de O2O y los assessments emocionales son datos especialmente sensibles — requieren tratamiento específico
- Cumplimiento GDPR desde el diseño (privacy by design)
- Cifrado a nivel de campo para datos especialmente sensibles
- Los datos de IA no pueden usarse para entrenamiento de modelos externos
- El modelo de IA debe tener garantías contractuales de privacidad de datos

### Infraestructura
- Cloud — no on-premise
- Arquitectura multi-tenant con aislamiento fuerte por instancia
- El research debe incluir comparativa de stacks:
  - **Opción A:** GCP + Firebase + Vertex AI (preferencia del autor por familiaridad con Firebase/Firestore y GCP)
  - **Opción B:** AWS + Amplify + Bedrock
  - **Opción C:** Supabase + Anthropic API
  - **Opción D:** cualquier otra que el research identifique como superior
- Base de datos de grafos para interconectar personas, equipos y proyectos — evaluar opciones managed (Neo4j Aura, Amazon Neptune, alternativas)
- Evaluar coste por instancia en escenarios: 1-10 / 10-100 / 100-1000 líderes
- Web y móvil (evaluar si PWA o apps nativas)

### Capa de IA y RAG
- GRETA como contexto base del RAG (documento estático, siempre presente)
- Historial de assessments y conversaciones de cada persona como contexto acumulado por instancia (RAG dinámico)
- Inputs procesables por IA:
  - Transcripción o resumen de conversación (O2O)
  - CV de cada persona
  - Logros y contribuciones documentados
  - Campos adicionales configurables por quien lidera
- Output de la IA para cada input:
  - Sugerencia de nivel por dimensión con razonamiento
  - Citas del texto que justifican la sugerencia
  - Preguntas sugeridas para la siguiente conversación
- Flujo de validación: quien lidera valida, ajusta o rechaza cada sugerencia antes de que actualice el mapa
- El historial de sugerencias y validaciones se guarda para mejorar la precisión con el tiempo
- Evaluar Anthropic API (Claude) como modelo principal — tiene política de no usar datos de usuario para entrenamiento

### Capa de visualización
- Visible solo por la persona propietaria de la instancia
- **Radar chart (diagrama de araña) por persona** — 4 dimensiones con los 7 colores del espectro
- **Evolución temporal por persona** — histórico de niveles en cada dimensión
- **Dashboard de equipo** — cobertura de roles de contribución, bus factor por área de conocimiento, score de salud del equipo
- **Vista de grafo** — interconexión entre personas, equipos y proyectos con los colores de nivel
- **Comparativa entre equipos** — para quien gestiona múltiples equipos
- **Mapa de calor de conocimiento** — por equipo, quién sabe qué y a qué nivel
- **Vistas compartibles (read-only)** — seleccionables por quien lidera, con link temporal o permanente, sin acceso a la app completa

### Flujo principal de uso
1. Persona que lidera se registra → el SaaS crea su instancia
2. Configura equipos y personas (puede importar desde CSV o LinkedIn)
3. Sube CV de cada persona → la IA extrae contexto y sugiere preguntas
4. Tiene una conversación y sube la transcripción o resumen
5. La IA procesa el texto con el contexto GRETA + historial de la persona
6. La IA propone niveles por dimensión con razonamiento y citas
7. Quien lidera valida, ajusta o rechaza cada sugerencia
8. El assessment validado actualiza el mapa y el historial
9. Las visualizaciones se actualizan en tiempo real

### Campos configurables
La app debe permitir añadir campos personalizados a la ficha de cada persona — logros, contribuciones notables, situaciones gestionadas, cualquier otro dato relevante — y que estos campos también puedan ser procesados por la IA para refinar el diagnóstico o sugerir preguntas para la siguiente conversación.
