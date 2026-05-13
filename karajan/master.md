# GRETA SaaS — Documento Maestro para Karajan Code
**Versión:** 1.0
**Referencia de framework:** GRETA v1.0-RC2

---

## INSTRUCCIONES DE EJECUCIÓN PARA KARAJAN CODE

Lee este documento completo de principio a fin antes de ejecutar nada. Contiene todo el contexto necesario: el framework GRETA que es el núcleo del producto, los requisitos completos del SaaS y los cuatro prompts que debes ejecutar en orden estricto.

**Reglas de ejecución:**
1. Activa los roles de research, discovery y architecture desde el inicio
2. Ejecuta los prompts en orden estricto: Research → Discovery → Architecture → HUs
3. No empieces el siguiente prompt hasta que el anterior esté completo
4. Usa el output de cada prompt como contexto adicional para el siguiente
5. Si encuentras una ambigüedad o decisión bloqueante que no puedes resolver con la información disponible, detente y pregunta antes de continuar
6. Guarda el output de cada prompt como documento separado:
   - `GRETA-research.md`
   - `GRETA-discovery.md`
   - `GRETA-architecture.md`
   - `GRETA-HUs.md`
7. Al finalizar los cuatro, genera un `GRETA-summary.md` con las decisiones clave tomadas en cada fase y el estado del proyecto listo para comenzar el desarrollo

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

---

## PARTE 3 — PROMPTS PARA KARAJAN CODE

Ejecutar en orden. No pasar al siguiente hasta que el anterior esté completo y guardado.

---

### PROMPT 1 — Research

```
Actúa como agente de research.

Lee el documento maestro completo antes de empezar. Contiene el contexto del framework GRETA y los requisitos del SaaS que vamos a construir.

Tu objetivo es hacer un research exhaustivo para responder las siguientes preguntas. Para cada pregunta, busca información actualizada, contrasta fuentes y presenta las opciones con pros, contras y coste estimado. No des respuestas genéricas — busca datos concretos, precios reales y limitaciones documentadas.

PREGUNTAS DE RESEARCH:

1. STACK PARA SAAS MULTI-TENANT CON INSTANCIAS POR USUARIO
   Compara las siguientes opciones para un SaaS donde cada persona que lidera tiene su instancia privada con datos aislados:
   - GCP + Firebase/Firestore + Vertex AI (opción preferente del autor por familiaridad)
   - AWS + Amplify + Bedrock
   - Supabase + Anthropic API (Claude)
   - Cualquier otra opción que identifiques como relevante
   Para cada opción evalúa:
   - Aislamiento de datos entre instancias (multi-tenancy)
   - Facilidad de crear instancias programáticamente al registrarse un nuevo usuario
   - Coste por instancia en escenarios de 1-10 / 10-100 / 100-1000 instancias activas
   - Madurez del ecosistema y soporte para web + móvil
   - Integración con la capa de IA
   - Cumplimiento GDPR

2. BASE DE DATOS DE GRAFOS MANAGED
   El sistema necesita representar relaciones entre personas, equipos y proyectos con propiedades en los nodos y aristas. Evalúa opciones managed en cloud:
   - Neo4j Aura
   - Amazon Neptune
   - Alternativas que identifiques (FaunaDB, ArangoDB cloud, etc.)
   Para cada opción evalúa:
   - Integración con el stack SaaS candidato
   - Coste por instancia o por volumen
   - Escalabilidad y latencia
   - Soporte para consultas de grafo complejas (personas → equipos → proyectos → assessments)
   - Alternativa: ¿puede Firestore o una base de datos relacional resolver este caso de uso sin necesidad de grafo nativo?

3. CAPA DE IA Y RAG PRIVADA
   El sistema necesita un RAG privado donde los datos de cada instancia no salgan del entorno controlado. Evalúa:
   - Anthropic API (Claude Sonnet/Haiku) — tiene política documentada de no usar datos de usuario para entrenamiento. Evalúa si es suficiente para cumplir GDPR en contexto europeo.
   - Vertex AI (Matching Engine + Gemini) en GCP
   - AWS Bedrock (Knowledge Bases + Claude o Titan)
   - Solución propia con Qdrant Cloud + modelo con garantías de privacidad
   Para cada opción evalúa:
   - Garantías contractuales de privacidad de datos (DPA, GDPR, Data Processing Agreements)
   - Coste por llamada y por volumen de embeddings almacenados
   - Latencia para procesamiento de documentos de hasta 10.000 palabras (transcripciones largas)
   - Calidad del modelo para análisis de texto cualitativo en español — evaluaciones o benchmarks disponibles
   - Facilidad de implementación de RAG con contexto por instancia aislada

4. PROCESAMIENTO DE CV Y DOCUMENTOS
   El sistema necesita extraer información estructurada de CVs en diferentes formatos (PDF, Word, LinkedIn) y textos libres (resúmenes de conversaciones, notas de logros).
   Evalúa:
   - Opciones de parsing de CV (Affinda, Sovren, parseur, extracción con LLM directo)
   - Coste y precisión para CVs en español
   - Cómo integrar el parsing con el pipeline de IA de GRETA

5. WEB Y MÓVIL
   El producto necesita versión web y móvil. Evalúa:
   - PWA vs apps nativas (iOS + Android)
   - Si el stack frontend elegido (React, Vue, etc.) soporta bien PWA con las funcionalidades necesarias (carga offline de mapas de equipo, notificaciones)
   - Frameworks de desarrollo móvil que compartan código con web: React Native, Flutter, Expo
   - Coste de mantenimiento y publicación en stores

6. CIFRADO Y PRIVACIDAD AVANZADA
   El sistema maneja datos especialmente sensibles — transcripciones de conversaciones privadas, assessments emocionales de personas reales.
   Investiga:
   - Mejores prácticas de cifrado a nivel de campo en el stack candidato
   - GDPR compliance en arquitecturas SaaS multi-tenant en Europa
   - Opciones de cifrado gestionado por el cliente (CMEK) en GCP y AWS
   - Qué categoría de datos personales representa un assessment emocional según el GDPR y qué obligaciones genera
   - Consideraciones específicas para datos de evaluación de personas en el contexto laboral europeo

7. MODELO DE COSTES
   Construye un modelo de costes para tres escenarios de adopción:
   - Early adopter: 1-10 personas que lideran, 5-50 personas por instancia, 2-5 conversaciones procesadas por semana
   - Crecimiento: 10-100 líderes, 10-100 personas por instancia, 5-20 conversaciones por semana
   - Escala: 100-1000 líderes, 10-200 personas por instancia, variable
   Incluye: infraestructura, IA (embeddings + inferencia), almacenamiento, base de datos de grafos, red, coste operativo estimado.
   Propón también un modelo de pricing para el SaaS basado en los costes calculados.

OUTPUT ESPERADO:
- Tabla comparativa de stacks con puntuación ponderada por criterio
- Recomendación razonada del stack óptimo para GRETA SaaS
- Modelo de costes detallado por escenario
- Modelo de pricing sugerido para el SaaS
- Lista de riesgos técnicos identificados con mitigación propuesta
- Preguntas que el research no ha podido responder y requieren decisión de producto o arquitectura
```

---

### PROMPT 2 — Discovery

```
Actúa como agente de discovery.

Lee el documento maestro completo. Tienes el contexto del framework GRETA, los requisitos del SaaS y los resultados del research anterior. Úsalos todos.

Tu objetivo es definir con precisión qué construimos — no cómo. Qué: actores, casos de uso, flujos, datos y reglas de negocio.

TAREAS DE DISCOVERY:

1. ACTORES DEL SISTEMA
   Define todos los actores que interactúan con el sistema:
   - Tipos de usuario (persona que lidera / observador invitado con vista read-only / administrador del SaaS)
   - Sistemas externos (LLM, storage, auth, grafo, parser de CV...)
   - Para cada actor: qué puede hacer, qué no puede hacer, qué ve, qué no ve

2. MAPA DE CASOS DE USO
   Para cada actor, lista todos los casos de uso organizados por épica:
   - Gestión de cuenta e instancia
   - Gestión de equipos y personas
   - Ficha de persona (CV, logros, campos personalizados)
   - Assessment y diagnóstico manual
   - Procesamiento de conversaciones con IA
   - Procesamiento de CV y documentos con IA
   - Visualización y reporting (radar, grafo, evolución, dashboard)
   - Compartición de vistas read-only
   - Administración del SaaS
   Para cada caso de uso: descripción breve, actor, precondiciones, postcondiciones, notas

3. FLUJOS PRINCIPALES
   Describe en pasos numerados los flujos completos de estos casos de uso críticos:
   - Registro de nueva persona y creación automática de instancia
   - Alta de equipo y personas en una instancia
   - Subida y procesamiento de CV con IA → sugerencias de nivel → validación
   - Subida de transcripción de conversación con IA → sugerencias de nivel con razonamiento → validación
   - Creación de assessment manual sin IA
   - Generación y compartición de una vista read-only
   - Consulta del grafo de relaciones entre personas, equipos y proyectos

4. MODELO DE DATOS CONCEPTUAL
   Define las entidades principales y sus relaciones:
   Entidades mínimas: Instancia, Persona que lidera, Equipo, Persona del equipo, Dimensión, Assessment, Nivel (con color), Conversación, Documento procesado (CV, logros), Sugerencia IA, Validación, Vista compartida, Campo personalizado, Proyecto
   Para cada entidad:
   - Atributos clave
   - Relaciones con otras entidades y cardinalidad
   - Clasificación de sensibilidad del dato (normal / sensible / muy sensible)
   Entidades del grafo (nodos y aristas):
   - Qué nodos existen y qué propiedades tienen
   - Qué aristas existen, en qué dirección y qué propiedades tienen
   - Casos de consulta típicos que el grafo debe resolver

5. REGLAS DE NEGOCIO CRÍTICAS
   Lista todas las reglas que el sistema debe respetar:
   - Aislamiento entre instancias (ningún dato cruza de una instancia a otra)
   - Quién puede ver qué dentro de una instancia
   - Reglas de compartición de vistas (qué puede compartirse, cómo se controla el acceso)
   - Reglas de validación de assessments (quien lidera siempre valida antes de guardar)
   - Reglas de la IA (la IA sugiere, nunca decide; toda sugerencia queda registrada aunque se rechace)
   - Reglas de retención y borrado de datos (GDPR: derecho al olvido)
   - Reglas de cifrado por tipo de dato
   - Reglas de campos personalizados (límites, tipos, quién los define)

6. REQUISITOS NO FUNCIONALES
   Define con precisión:
   - Latencia máxima aceptable por operación (procesamiento de conversación con IA, carga del grafo, carga del radar chart)
   - Disponibilidad requerida
   - Volumen de datos estimado por instancia (pequeña / mediana / grande)
   - Requisitos de auditoría y logging de accesos a datos sensibles
   - Requisitos de backup y recuperación
   - Requisitos de accesibilidad (WCAG mínimo)

OUTPUT ESPERADO:
- Documento de discovery completo con todas las secciones
- Lista de decisiones de producto que requieren validación antes de la arquitectura
- Lista de riesgos de producto identificados
- Preguntas abiertas que el discovery no ha resuelto
```

---

### PROMPT 3 — Architecture

```
Actúa como agente de arquitectura.

Lee el documento maestro completo. Tienes el contexto de GRETA, los requisitos del SaaS, el research y el discovery. Úsalos todos.

Tu objetivo es definir la arquitectura técnica completa. Toma decisiones — no presentes opciones sin decidir. Si hay ambigüedad, decide y justifica. Si una decisión depende de algo externo (coste, preferencia del producto), nómbralo y asume el valor más probable según el research.

TAREAS DE ARQUITECTURA:

1. DECISIONES DE ARQUITECTURA (ADRs)
   Para cada decisión relevante, crea un ADR con: contexto, opciones consideradas, decisión, justificación, consecuencias.
   ADRs mínimos requeridos:
   - Stack principal del SaaS (frontend, backend, base de datos, auth)
   - Estrategia de multi-tenancy e isolación de instancias
   - Base de datos principal y estrategia de grafo (nativa vs emulada)
   - Estrategia de cifrado y gestión de claves
   - Arquitectura de la capa RAG (vector store, embedding model, LLM)
   - Modelo de autenticación y autorización
   - Estrategia de compartición de vistas read-only
   - Estrategia web + móvil (PWA vs nativa)
   - Pipeline de procesamiento de CV y documentos
   - Estrategia de creación programática de instancias

2. DIAGRAMA DE ARQUITECTURA
   Describe en texto estructurado la arquitectura completa con todos sus componentes y cómo se conectan:
   - Frontend web y móvil
   - Backend / API layer
   - Autenticación y autorización
   - Base de datos principal (por instancia o shared con aislamiento)
   - Capa de grafo
   - Pipeline de IA: ingesta → chunking → embedding → vector store → retrieval → LLM → output
   - Almacenamiento de archivos (CVs, transcripciones)
   - Capa de cifrado
   - Provisioning de instancias nuevas
   - CDN y compartición de vistas read-only
   - Monitoring y alerting
   Para cada componente: tecnología elegida, responsabilidad, interfaces con otros componentes

3. MODELO DE DATOS TÉCNICO
   Basado en el modelo conceptual del discovery:
   - Schema completo de la base de datos principal (colecciones/tablas, campos, tipos, índices)
   - Schema del grafo (nodos con propiedades, aristas con propiedades y dirección)
   - Estructura de los vectores (qué se embeddea, metadatos indexados, estrategia de namespace por instancia)
   - Estrategia de particionado o aislamiento por instancia
   - Estrategia de cifrado a nivel de campo para datos muy sensibles

4. ARQUITECTURA DE LA CAPA IA
   Define con precisión:
   - Pipeline completo de procesamiento de una transcripción de conversación: entrada → chunking → embedding → retrieval → construcción de contexto → llamada al LLM → parsing del output → presentación al usuario
   - Cómo se construye el contexto RAG para cada llamada: documento GRETA base + historial de la persona + input nuevo
   - Estructura del system prompt al LLM (qué instrucciones, qué formato de output se pide)
   - Cómo se parsea y valida el output del LLM antes de mostrarlo
   - Pipeline de procesamiento de CV: entrada → extracción de texto → parsing estructurado → identificación de proyectos/retos → sugerencia de preguntas para conversación
   - Cómo se almacena el historial de sugerencias y validaciones y cómo alimenta futuras llamadas
   - Estrategia de namespace en el vector store para aislamiento por instancia

5. ARQUITECTURA DE SEGURIDAD
   Define:
   - Modelo de amenazas específico para GRETA SaaS (datos sensibles de evaluación de personas, transcripciones privadas)
   - Controles de seguridad por capa
   - Estrategia de cifrado en reposo y en tránsito
   - Gestión de claves (KMS) y rotación
   - Auditoría y logging de accesos a datos sensibles (quién accedió a qué y cuándo)
   - GDPR: flujo de derecho al olvido (borrado completo de una instancia o de una persona)
   - Plan de respuesta ante breach

6. INFRAESTRUCTURA Y DEVOPS
   Define:
   - Cómo se crea una nueva instancia de forma programática al registrarse (IaC, automatización)
   - Estructura de environments (dev, staging, prod)
   - Pipeline CI/CD
   - Estrategia de monitorización y alerting (métricas clave, umbrales)
   - Estrategia de control de costes (cómo se monitoriza y controla el coste por instancia)
   - Estrategia de backup y recuperación por instancia

OUTPUT ESPERADO:
- Documento de arquitectura completo con todos los ADRs
- Diagrama de arquitectura en texto (formato Mermaid o similar para conversión a visual)
- Schema de base de datos completo
- Schema del grafo completo
- Pipeline de IA documentado paso a paso
- Lista completa de componentes con tecnología, versión y justificación
- Estimación de esfuerzo de implementación por capa (S/M/L/XL)
- Riesgos técnicos con mitigación
```

---

### PROMPT 4 — Plan de Historias de Usuario

```
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
- Estimación total de duración hasta MVP y hasta Beta
```

---

*Documento maestro GRETA SaaS v1.0 — para uso con Karajan Code*
*Framework base: GRETA v1.0-RC2*
