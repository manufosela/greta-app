# GRETA SaaS — Documento Maestro para Karajan Code
**Versión:** 2.0
**Referencia de framework:** GRETA v1.0-RC3

---

## INSTRUCCIONES PARA EL PLANNER

Lee este documento completo antes de generar nada. Contiene el contexto del framework GRETA (Parte 1) y los requisitos del SaaS que hay que construir (Parte 2).

**Tu trabajo es descomponer el SaaS GRETA en HUs atómicas y ejecutables individualmente con `kj run`.** No estás generando documentación intermedia (research, discovery, architecture). Estás generando las HUs reales del producto, cada una una unidad de trabajo concreta que un coder puede implementar.

**Reglas duras:**
1. Cada HU es atómica: un coder la implementa en una pasada, con tests verificables. Si una pieza es demasiado grande para una HU, pártela en varias HUs encadenadas con `blocked_by`.
2. ID con prefijo de épica: `[AUTH-001]`, `[PROFILE-002]`, `[ASSESS-003]`, `[AI-004]`, `[IMPACT-005]`, `[GUARD-006]`, `[NETWORK-007]`, etc.
3. Idioma de las HUs: **español**. Historia "como [actor], quiero [acción], para [valor]".
4. Acceptance tests EJECUTABLES en cada HU (shell o gherkin). Sin tests, la HU no es ejecutable.
5. Dependencias entre HUs expresadas en `blocked_by`. Si una HU depende de otra, declárala.
6. **No generes una lista de "docs a producir"**. Genera HUs del producto: una HU que crea un Cloud Function, una HU que añade un campo a Firestore, una HU que monta un componente Lit, etc.

---

## PARTE 1 — CONTEXTO: QUÉ ES GRETA

GRETA (Guía de Referencia de Equipos de Trabajo Afectivo) es un framework de diagnóstico y composición de equipos de ingeniería. Su propósito es dar a quien lidera una lectura estructurada de su equipo — no como una foto de competencias individuales, sino como un sistema vivo con gaps, fortalezas y dinámicas propias.

GRETA no es un cuestionario ni una herramienta de evaluación de personas. Es una guía de lectura para quien lidera. Las personas son la estrategia.

### Las cuatro dimensiones

**Dimensión 1 — Rol de contribución**
Cómo aporta cada persona al equipo. Usa la taxonomía de 9 roles de Belbin como referencia interna (PL/Cerebro, ME/Monitor evaluador, SP/Especialista, CO/Coordinador, RI/Investigador de recursos, TW/Cohesionador, SH/Impulsor, IMP/Implementador, CF/Finalizador). El nivel describe la consciencia y flexibilidad con que se ejerce el rol natural.

**Dimensión 2 — Conocimiento**
Qué sabe el equipo colectivamente. Áreas: Ingeniería, Bases de datos, APIs y comunicación, Frontend, Operaciones, Seguridad, Arquitectura, Calidad, IA aplicada. Bus factor por área. Nivel de dominio: básico / en progreso / sólido. Perfil global: I-shape, T-shape, π-shape o Comb.

**Dimensión 3 — Seniority**
Madurez profesional — no título organizativo. Tres capacidades: anticipación, resiliencia ejecutiva y criterio bajo incertidumbre. El CV orientado a proyectos y retos es input relevante procesable con IA.

**Dimensión 4 — Emocional**
Cómo está cada persona dentro del equipo. Tres subdimensiones: motivación, alineamiento y metaconsciencia. La más dinámica y la más exigente para quien lidera.

### La escala de niveles

| Color | Nivel | Grupo |
|-------|-------|-------|
| 🔴 Rojo | Tiro | Ejecuta con guía |
| 🟠 Naranja | Novicius | Ejecuta con guía |
| 🟡 Amarillo | Peritus | Ejecuta con autonomía |
| 🟢 Verde | Expertus | Ejecuta con autonomía |
| 🔵 Azul | Veteranus | Decide y anticipa |
| 🟣 Violeta | Primus | Decide y anticipa |
| ⚪ Blanco | Magister | Transforma (aspiracional) |

### La capa de impacto

Capa por encima de las cuatro dimensiones que conecta la composición del equipo con los outcomes reales. Tres niveles: outcomes del negocio → contribución del equipo → contribución individual (formulada como hipótesis).

Cada outcome tiene señales tempranas (leading indicators). Las intervenciones inciertas se validan con experimentos acotados. Los gaps se priorizan por valor (cuánto limita el outcome) y riesgo (cuán disruptiva es la intervención). El marco de referencia es *Impact Driven Growth* de Carlos Iglesias.

**Output vs outcome:** output es lo que hace el equipo (actividad). Outcome es lo que cambia gracias a lo que hace el equipo (valor). Solo los outcomes miden impacto real. Los guardarraíles de la IA detectan cuando se confunde uno con otro.

### Los guardarraíles de la IA

La IA protege el espíritu del framework en todos los puntos de entrada donde puede producirse un desvío. Nunca bloquea — aconseja, contextualiza y propone alternativas. Quien lidera siempre tiene la última palabra.

9 puntos de aplicación: al definir outcomes, al definir contribuciones individuales, al registrar evidencias, al procesar transcripciones, al asignar niveles sin base suficiente, al crear vistas compartidas, al leer bajadas de nivel, al detectar sesgos de atención, al interpretar el mapa de forma comparativa.

### GRETA en red — automedición y liderazgo multinivel

**Automedición de quien lidera:** quien lidera también tiene un perfil en las cuatro dimensiones y puede (y debe) medirse con el mismo rigor. Requiere feedback estructurado del equipo y revisión con alguien externo para compensar el sesgo del observador.

**Evaluación del equipo a quien lidera:** herramienta de escucha estructurada — no métrica de ego ni autoflagelación. Dos modalidades: proactiva libre (cualquier persona del equipo envía feedback cuando quiere) y periódica invitada (quien lidera activa una ronda). Anonimato estructural en cuatro capas. Preguntas ancladas a comportamientos observables por dimensión. El resultado es un mapa de percepción, no un nivel.

**Evaluación en tres direcciones:** equipo → quien lidera, quien lidera → su líder (CTO/VP), equipo → TL. Nada fluye automáticamente — todo es decisión activa de quien lo comparte.

**GRETA en red:** TLs, EMs y CTOs usan GRETA cada uno en su contexto. Pueden compartir visibilidad de forma voluntaria. La red existe porque cada nodo decide conectarse — no por defecto. Tres tipos de visibilidad compartida: lateral (entre pares), hacia arriba (voluntaria y agregada), del todo (para quien coordina, con masa crítica suficiente). La vista del todo muestra patrones, nunca personas ni niveles individuales.

**Los tres mecanismos de transparencia voluntaria:**
1. Quien está arriba comparte primero — invierte la asimetría de poder
2. La app muestra el porcentaje de participación, no quién participa
3. La vista del conjunto solo aparece con masa crítica suficiente — umbral configurable

### Principios críticos de uso

- GRETA lo gestiona quien lidera con transparencia y criterio
- El seguimiento frecuente es el método
- En la conversación nunca se habla de niveles — solo de comportamientos concretos
- Los niveles no son jerarquía — son etiquetas de tendencia
- Cada persona puede estar en niveles distintos en cada dimensión
- La subjetividad no se elimina — se gestiona
- El diagnóstico caduca — requiere actualización continua
- Nada en la red fluye automáticamente — todo es decisión activa

### Los tres escenarios operativos

1. **Heredar** un equipo ya formado
2. **Formar** un equipo nuevo desde el mapa de gaps
3. **Modificar** un equipo que no está funcionando

---

## PARTE 2 — REQUISITOS DEL SAAS GRETA

### Modelo de negocio
- SaaS con instancias independientes por persona que lidera
- Una persona que lidera = una instancia privada y aislada
- El SaaS crea la instancia automáticamente al registrarse
- Escalable hacia arriba y hacia abajo en cualquier momento
- Posibilidad de conectar instancias en red (TL ↔ EM ↔ CTO) por consentimiento explícito mutuo
- Posibilidad de compartir vistas específicas (read-only) con terceros sin darles acceso a la app
- Producto web y móvil

### Privacidad y seguridad
- Datos encriptados en reposo y en tránsito
- Privacidad total por instancia — ninguna instancia ve datos de otra sin consentimiento explícito
- Transcripciones de O2O, assessments emocionales y evaluaciones de liderazgo: datos especialmente sensibles
- Cumplimiento GDPR desde el diseño
- Cifrado a nivel de campo para datos muy sensibles
- Los datos de IA no pueden usarse para entrenamiento de modelos externos
- **Anonimato estructural en evaluaciones de liderazgo:**
  - Umbral mínimo de respuestas configurable antes de mostrar resultados
  - Resultados siempre agregados, nunca individuales
  - La app no registra quién respondió ni cuándo
  - Imposible cruzar resultados con otras variables del sistema para identificar a quien respondió

### Infraestructura
- Cloud — no on-premise
- Arquitectura multi-tenant con aislamiento fuerte por instancia
- Stack preferente del autor: **GCP + Firebase/Firestore + Vertex AI** (alternativas válidas si el planner lo justifica: AWS+Amplify+Bedrock, Supabase+Anthropic API). El planner decide y razona la elección en el approach del plan.
- Base de datos de grafos para interconectar personas, equipos, proyectos e instancias en red
- Coste por instancia en escenarios: 1-10 / 10-100 / 100-1000 instancias
- Web y móvil

### Capa de IA y RAG
- GRETA como contexto base del RAG (siempre presente)
- Historial de assessments y conversaciones por instancia como contexto acumulado
- Inputs procesables por IA:
  - Transcripción o resumen de conversación (O2O)
  - CV de cada persona
  - Logros y contribuciones documentados
  - Campos adicionales configurables
  - Outcomes y contribuciones (para guardarraíles)
  - Respuestas de evaluaciones de liderazgo (para mapa de percepción)
- Output para assessments: nivel sugerido + razonamiento + citas + preguntas para siguiente conversación
- Output para guardarraíles: evaluación de outcomes/contribuciones/evidencias + alternativas sugeridas
- Output para evaluaciones de liderazgo: mapa de percepción agregado + divergencias con autodiagnóstico + preguntas sugeridas para O2O
- Output para vista de red: patrones agregados entre instancias conectadas (nunca datos individuales)
- Flujo de validación: quien lidera siempre valida antes de que cualquier sugerencia actualice el mapa
- Evaluar Anthropic API (Claude) como modelo principal

### Capa de visualización
- **Radar chart por persona** — 4 dimensiones con los 7 colores
- **Evolución temporal por persona** — histórico con contextualización de cambios
- **Dashboard de equipo** — cobertura de roles, bus factor, score de salud
- **Dashboard de impacto** — outcomes con señales tempranas, hipótesis de contribución, evidencias, matriz valor/riesgo de gaps
- **Vista de grafo** — relaciones entre personas, equipos y proyectos, con colores de nivel por dimensión
- **Comparativa entre equipos**
- **Mapa de calor de conocimiento**
- **Radar de autodiagnóstico de quien lidera** — las cuatro dimensiones sobre sí mismo
- **Mapa de percepción** — divergencias entre autodiagnóstico y percepción del equipo, por dimensión
- **Vista de red** — porcentaje de participación en la red, patrones agregados entre instancias conectadas (solo si hay masa crítica), sin datos individuales de otras instancias
- **Vistas compartibles (read-only)** — con revisión por guardarraíl antes de compartir

### Evaluación de liderazgo — requisitos específicos
- **Dos modalidades:** proactiva libre (equipo envía cuando quiere) y periódica invitada (quien lidera activa)
- **Tres direcciones:** equipo → quien lidera, quien lidera → su líder, equipo → TL
- **Preguntas organizadas por dimensión GRETA** — no valoraciones genéricas sino comportamientos observables
- **Anonimato estructural en cuatro capas** (ver sección de privacidad)
- **Umbral configurable** según tamaño del equipo — sin umbral no se muestran resultados
- **El resultado:** mapa de percepción agregado, no niveles individuales
- **La IA** detecta divergencias entre autodiagnóstico y percepción y sugiere preguntas para la siguiente O2O
- **Para evaluación hacia arriba** (quien lidera evalúa a su líder): preguntas adaptadas, sin anonimato (un solo evaluador), orientadas a "¿las condiciones que genera tu líder te permiten hacer tu trabajo?"

### GRETA en red — requisitos específicos
- **Conexión entre instancias por consentimiento mutuo explícito** — ambas partes deben activarlo
- **Nada fluye automáticamente** — toda visibilidad compartida es una decisión activa
- **Tres tipos de conexión:** lateral (entre pares), hacia arriba (EM → CTO), hacia abajo (CTO ve agregado de EMs)
- **Vista del todo solo con masa crítica** — umbral configurable; sin masa crítica no se muestra
- **La app muestra el porcentaje de participación** en la red, nunca quién participa y quién no
- **Lo que nunca aparece en la vista de red:** niveles individuales de ninguna persona, nombres, datos de dimensión emocional desagregados, evaluaciones de liderazgo individuales
- **Lo que sí aparece en la vista de red:** patrones de gap comunes entre equipos, tendencias de impacto, porcentaje de outcomes alcanzados por cluster de equipos, alertas sistémicas (ej: dimensión emocional comprometida en varios equipos a la vez)

### Flujo principal de uso
1. Persona que lidera se registra → instancia creada automáticamente
2. Configura equipos y personas
3. Sube CV → IA extrae proyectos y retos → sugiere preguntas de seniority
4. Define el reto específico del equipo
5. Define outcomes con señales tempranas → guardarraíl 1 evalúa
6. Formula hipótesis de contribución individual por persona → guardarraíl 2 evalúa
7. Sube transcripción de conversación → pipeline RAG → sugerencias de nivel → validación
8. Registra evidencias de impacto → guardarraíl 3 evalúa
9. Activa evaluación de liderazgo (periódica) o recibe feedback libre del equipo
10. La IA genera mapa de percepción y divergencias con autodiagnóstico
11. Opcionalmente conecta su instancia con la de su líder o peers → visibilidad compartida agregada
12. Las visualizaciones se actualizan en tiempo real

---

## LO QUE NECESITO QUE GENERES

Una lista de HUs **atómicas y ejecutables individualmente con `kj run`**. Cada HU es una unidad de trabajo de un coder — no un documento, no una fase. Para cada HU:

- **id** con prefijo de épica (`[AUTH-001]`, `[PROFILE-002]`, etc.) — los prefijos posibles según las épicas: AUTH (autenticación + instancias), PEOPLE (CRUD personas/equipos), PROFILE (ficha de persona con CV), ASSESS (assessments manuales por dimensión), AI (pipeline RAG + Claude), IMPACT (outcomes, señales tempranas, hipótesis, evidencias, experimentos, gaps), GUARD (los 9 guardarraíles), LEAD (evaluación de liderazgo), NETWORK (GRETA en red, conexión entre instancias), VIZ (visualizaciones).
- **title**: breve, descriptivo, < 80 chars.
- **scope / description**: párrafo corto con qué se construye y cómo encaja en el sistema.
- **acceptance_criteria**: 3-8 criterios verificables en formato Given/When/Then o lista plana.
- **acceptance_tests**: lista de tests EJECUTABLES (shell o gherkin) que demuestran que la HU está implementada.
- **blocked_by**: array de ids de HUs que deben completarse antes (mínimo `[PREFLIGHT-000]` para todas excepto la propia HU-0).
- **task_type**: sw / infra / doc / add-tests / refactor / nocode.
- **estimación** (opcional): XS / S / M / L / XL.

**Reglas finales:**
- Cubre las 10 épicas listadas arriba con HUs proporcionales a su complejidad.
- Si una funcionalidad requiere decisiones arquitectónicas no resueltas (ej: elección de stack), genera una HU **spike** específica con scope "investigar y decidir" y blocked_by hacia las HUs que dependen de esa decisión.
- HUs de infra (envelope encryption, audit log, Cloud Functions setup, Vector Search namespace, cron runner, retry queue) van separadas de las HUs funcionales que las usan, con dependencias declaradas.
- Componentes UI del catálogo `@manufosela/*`: HUs separadas para publicar componentes nuevos al catálogo (`tree-view`, `quadrant-matrix`, `inline-banner`, `rich-text-viewer`, `heatmap-chart`, `level-selector`, `file-uploader`) ANTES de usarlos en HUs del SaaS.
- Idioma de HUs: **español**.
- NO escribas un documento markdown describiendo HUs. **Genera HUs reales como entradas separadas en el plan**, cada una ejecutable por separado con `kj run --plan <id> --hu <hu-id>`.

---

*Documento maestro GRETA SaaS v2.0 — para uso con Karajan Code*
*Framework base: GRETA v1.0-RC3*
*Referencias: Impact Driven Growth (Carlos Iglesias), idg.runroom.com*
