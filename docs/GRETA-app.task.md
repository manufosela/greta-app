# GRETA SaaS — Documento Maestro para Karajan Code
**Versión:** 1.2
**Referencia de framework:** GRETA v1.0-RC3

---

## INSTRUCCIONES DE EJECUCIÓN PARA KARAJAN CODE

Lee este documento completo de principio a fin antes de ejecutar nada. Contiene todo el contexto necesario: el framework GRETA que es el núcleo del producto, los requisitos completos del SaaS y los cuatro prompts que debes ejecutar en orden estricto.

**Reglas de ejecución:**
1. Activa los roles de research, discovery y architecture desde el inicio
2. Ejecuta los prompts en orden estricto: Research → Discovery → Architecture → HUs
3. No empieces el siguiente prompt hasta que el anterior esté completo
4. Usa el output de cada prompt como contexto adicional para el siguiente
5. Si encuentras una ambigüedad o decisión bloqueante, detente y pregunta antes de continuar
6. Guarda el output de cada prompt como documento separado:
   - `GRETA-research.md`
   - `GRETA-discovery.md`
   - `GRETA-architecture.md`
   - `GRETA-HUs.md`
7. Al finalizar los cuatro, genera `GRETA-summary.md` con las decisiones clave y el estado del proyecto listo para comenzar el desarrollo

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
- Comparativa de stacks en el research:
  - **Opción A:** GCP + Firebase/Firestore + Vertex AI (preferencia del autor)
  - **Opción B:** AWS + Amplify + Bedrock
  - **Opción C:** Supabase + Anthropic API (Claude)
  - **Opción D:** cualquier otra que el research identifique como superior
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

## PARTE 3 — PROMPTS PARA KARAJAN CODE

---

### PROMPT 1 — Research

```
Actúa como agente de research.

Lee el documento maestro completo antes de empezar. Contiene el contexto del framework GRETA — incluyendo la capa de impacto, los guardarraíles de la IA, el sistema de evaluación de liderazgo y el modelo de red multinivel — y los requisitos del SaaS.

Tu objetivo es hacer un research exhaustivo. Para cada pregunta, busca información actualizada, contrasta fuentes y presenta opciones con pros, contras y coste estimado. No des respuestas genéricas — busca datos concretos, precios reales y limitaciones documentadas.

PREGUNTAS DE RESEARCH:

1. STACK PARA SAAS MULTI-TENANT CON INSTANCIAS POR USUARIO
   Compara: GCP + Firebase/Firestore + Vertex AI (preferencia del autor), AWS + Amplify + Bedrock, Supabase + Anthropic API (Claude), y cualquier otra opción relevante.
   Para cada opción: aislamiento de datos entre instancias, creación programática de instancias, coste en escenarios 1-10 / 10-100 / 100-1000 instancias, soporte web + móvil, integración con capa de IA, cumplimiento GDPR.

2. BASE DE DATOS DE GRAFOS MANAGED
   El sistema necesita representar relaciones entre personas, equipos, proyectos e instancias conectadas en red. Evalúa: Neo4j Aura, Amazon Neptune, alternativas.
   Para cada opción: integración con el stack candidato, coste, escalabilidad, soporte para consultas complejas. Evalúa también: ¿puede Firestore o una base de datos relacional resolver este caso sin grafo nativo?

3. CAPA DE IA Y RAG PRIVADA
   El sistema necesita RAG privado con datos aislados por instancia. Evalúa: Anthropic API (Claude), Vertex AI, AWS Bedrock, Qdrant Cloud + modelo con garantías de privacidad.
   Para cada opción: garantías contractuales de privacidad (DPA, GDPR), coste por llamada y embeddings, latencia para documentos de hasta 10.000 palabras, calidad para análisis cualitativo en español, facilidad de RAG con namespace por instancia.

4. GUARDARRAÍLES DE IA — IMPLEMENTACIÓN TÉCNICA
   Los guardarraíles evalúan en tiempo real si un texto cumple criterios del framework y proponen alternativas.
   Investiga: clasificadores con LLM vs fine-tuning vs clasificadores ligeros, latencia para feedback en tiempo real vs al guardar, coste por evaluación, alternativas con modelos más pequeños.

5. ANONIMATO ESTRUCTURAL EN EVALUACIONES DE LIDERAZGO
   El sistema debe garantizar anonimato real en evaluaciones donde el equipo evalúa a quien lidera — no solo promesas, sino garantías técnicas.
   Investiga: mejores prácticas de anonimato estructural en sistemas de feedback 360, cómo implementar el umbral mínimo de respuestas técnicamente, cómo evitar que cruzar datos del sistema permita identificar a evaluadores, consideraciones GDPR específicas para datos de evaluación de personas en el ámbito laboral europeo.

6. MODELO DE RED ENTRE INSTANCIAS
   El sistema necesita conectar instancias de forma opcional y consensuada, compartir datos agregados entre ellas y garantizar que nunca se exponen datos individuales de otras instancias.
   Investiga: patrones de arquitectura para federación de datos entre tenants (tenant federation), cómo implementar vistas agregadas sin exponer datos raw, cómo gestionar el consentimiento mutuo entre instancias técnicamente.

7. PROCESAMIENTO DE CV Y DOCUMENTOS
   Evalúa: parsers de CV (Affinda, extracción directa con LLM), coste y precisión para CVs en español, integración con el pipeline RAG de GRETA.

8. WEB Y MÓVIL
   Evalúa: PWA vs apps nativas, React Native / Flutter / Expo, soporte offline para mapas de equipo, notificaciones, coste de publicación en stores.

9. CIFRADO Y PRIVACIDAD AVANZADA
   Investiga: cifrado a nivel de campo para datos especialmente sensibles (evaluaciones emocionales, transcripciones, evaluaciones de liderazgo), GDPR compliance en SaaS multi-tenant en Europa, CMEK en GCP y AWS, obligaciones específicas para datos de evaluación de personas en contexto laboral europeo.

10. MODELO DE COSTES Y PRICING
    Escenarios: early adopter (1-10 instancias, 5-50 personas, 2-5 conversaciones/semana), crecimiento (10-100 instancias), escala (100-1000 instancias).
    Incluye: infraestructura, IA (assessments + guardarraíles + evaluaciones de liderazgo + red), almacenamiento, grafo, red, coste operativo.
    Propón un modelo de pricing para el SaaS.

OUTPUT ESPERADO:
- Tabla comparativa de stacks con puntuación ponderada
- Recomendación razonada del stack óptimo
- Recomendación sobre guardarraíles y anonimato estructural
- Recomendación sobre arquitectura de red entre instancias
- Modelo de costes detallado por escenario
- Modelo de pricing sugerido
- Riesgos técnicos con mitigación
- Preguntas que requieren decisión de producto o arquitectura
```

---

### PROMPT 2 — Discovery

```
Actúa como agente de discovery.

Lee el documento maestro completo. Tienes el contexto de GRETA, los requisitos del SaaS y los resultados del research. Úsalos todos.

Tu objetivo es definir con precisión qué construimos — no cómo.

TAREAS DE DISCOVERY:

1. ACTORES DEL SISTEMA
   Define todos los actores:
   - Tipos de usuario: persona que lidera (TL, EM, CTO — cualquier nivel), observador invitado read-only, administrador del SaaS, evaluador anónimo (persona del equipo que responde una evaluación de liderazgo sin estar registrada en el sistema)
   - Sistemas externos: LLM, storage, auth, grafo, parser de CV, motor de guardarraíles, sistema de red entre instancias
   Para cada actor: qué puede hacer, qué no puede hacer, qué ve, qué no ve.

2. MAPA DE CASOS DE USO
   Por actor, organizado por épica:
   - Gestión de cuenta e instancia
   - Gestión de equipos y personas
   - Ficha de persona (CV, logros, campos personalizados)
   - Assessment y diagnóstico manual (4 dimensiones, 7 niveles)
   - Autodiagnóstico de quien lidera (sus propias 4 dimensiones)
   - Evaluación del equipo a quien lidera (las dos modalidades, las tres direcciones)
   - Procesamiento de conversaciones con IA + guardarraíles
   - Procesamiento de CV y documentos con IA
   - Capa de impacto (reto específico, outcomes, señales tempranas, hipótesis, experimentos, priorización)
   - Guardarraíles de IA (los 9 puntos)
   - Red entre instancias (conexión, consentimiento, visibilidad compartida, vista del todo)
   - Visualización (radar, grafo, evolución, dashboard de equipo, dashboard de impacto, mapa de percepción, vista de red)
   - Compartición de vistas read-only
   - Administración del SaaS

3. FLUJOS PRINCIPALES
   Describe en pasos numerados los flujos de estos casos críticos:
   - Registro y creación automática de instancia
   - Definición de reto específico + outcome + señal temprana → guardarraíl 1 → guardado
   - Formulación de hipótesis de contribución individual → guardarraíl 2
   - Evaluación de liderazgo proactiva libre (equipo envía feedback anónimo)
   - Evaluación de liderazgo periódica invitada (quien lidera activa ronda)
   - Recepción del mapa de percepción y divergencias con autodiagnóstico
   - Conexión de dos instancias en red (consentimiento mutuo)
   - Activación de vista del todo cuando hay masa crítica
   - Experimento de equipo acotado: definición → observación → decisión
   - Subida de transcripción → pipeline RAG → validación de sugerencias

4. MODELO DE DATOS CONCEPTUAL
   Entidades mínimas: Instancia, Persona que lidera, Equipo, Persona del equipo, Dimensión, Assessment, Nivel (con color), Conversación, Documento procesado, Sugerencia IA, Validación/Rechazo, Reto específico, Outcome, Señal temprana, Hipótesis de contribución, Experimento de equipo, Evidencia de impacto, Evaluación de liderazgo (agregada, nunca individual), Respuesta de evaluación (anónima), Mapa de percepción, Conexión de red, Vista compartida, Campo personalizado, Proyecto, Alerta de guardarraíl.
   Para cada entidad: atributos clave, relaciones, cardinalidad, clasificación de sensibilidad.
   Entidades del grafo: nodos (persona, equipo, proyecto, instancia), aristas (pertenece a, lidera, conectada con), propiedades, casos de consulta.
   Modelo de anonimato: cómo se almacenan las respuestas de evaluación de liderazgo garantizando que no son rastreables a quien las envió.

5. REGLAS DE NEGOCIO CRÍTICAS
   - Aislamiento entre instancias (ningún dato cruza sin consentimiento explícito mutuo)
   - Anonimato estructural en evaluaciones de liderazgo (cuatro capas)
   - Umbral mínimo de respuestas antes de mostrar resultados de evaluación (configurable)
   - La IA sugiere, nunca decide — toda sugerencia requiere validación
   - Los guardarraíles no bloquean — advierten con registro
   - Conexión de red: requiere consentimiento activo de ambas instancias
   - Vista del todo: solo con masa crítica suficiente (umbral configurable)
   - La vista de red nunca expone niveles individuales, nombres ni datos emocionales desagregados
   - Reglas de retención y borrado GDPR (incluyendo datos de evaluaciones anónimas)
   - Quien lidera decide qué comparte en la red — nada es automático

6. REQUISITOS NO FUNCIONALES
   - Latencia por operación (guardarraíl en tiempo real, procesamiento de conversación, carga de vista de red, evaluación de liderazgo)
   - Disponibilidad
   - Volumen de datos por instancia y por red de instancias
   - Auditoría de accesos a datos sensibles
   - Backup y recuperación
   - Accesibilidad (WCAG mínimo)

OUTPUT ESPERADO:
- Documento de discovery completo
- Decisiones de producto que requieren validación
- Riesgos de producto identificados
- Preguntas abiertas
```

---

### PROMPT 3 — Architecture

```
Actúa como agente de arquitectura.

Lee el documento maestro completo. Tienes el contexto de GRETA, los requisitos, el research y el discovery. Toma decisiones — no presentes opciones sin decidir.

TAREAS DE ARQUITECTURA:

1. ADRs (Architecture Decision Records)
   ADRs mínimos:
   - Stack principal
   - Estrategia de multi-tenancy e isolación de instancias
   - Base de datos principal y estrategia de grafo
   - Estrategia de cifrado y KMS
   - Arquitectura de la capa RAG (assessments + guardarraíles + evaluaciones)
   - Implementación técnica de guardarraíles (inline vs al guardar, prompt especializado vs general)
   - Implementación técnica del anonimato estructural en evaluaciones de liderazgo
   - Arquitectura de red entre instancias (federación, consentimiento, datos agregados)
   - Modelo de autenticación y autorización
   - Estrategia de vistas compartidas read-only
   - Estrategia web + móvil
   - Pipeline de CV y documentos
   - Provisioning programático de instancias

2. DIAGRAMA DE ARQUITECTURA
   Todos los componentes y conexiones:
   - Frontend web y móvil
   - Backend / API layer
   - Autenticación y autorización
   - Base de datos principal por instancia
   - Capa de grafo (relaciones intra e inter instancias)
   - Pipeline de IA: assessments, guardarraíles, evaluaciones de liderazgo, vista de red
   - Sistema de anonimato para evaluaciones (cómo se garantiza técnicamente)
   - Sistema de red entre instancias (consentimiento, federación, agregación)
   - Almacenamiento de archivos
   - Capa de cifrado
   - Provisioning de instancias
   - CDN y vistas compartidas
   - Monitoring y alerting

3. MODELO DE DATOS TÉCNICO
   - Schema completo de la base de datos principal
   - Schema del grafo (nodos, aristas, propiedades — incluyendo nodos de instancia para la red)
   - Estructura de vectores (namespace por instancia, qué se embeddea para assessments vs evaluaciones vs red)
   - Modelo de almacenamiento anónimo de respuestas de evaluación de liderazgo
   - Estrategia de datos agregados inter-instancia (cómo se computan sin exponer datos raw)
   - Cifrado a nivel de campo para datos muy sensibles

4. ARQUITECTURA DE LA CAPA IA
   Pipeline de assessments (transcripción → RAG → sugerencia de nivel).
   Pipeline de guardarraíles (input → evaluación → advertencia o aprobación).
   Pipeline de evaluaciones de liderazgo:
   - Cómo se recogen las respuestas anónimas
   - Cómo se agregan garantizando anonimato
   - Cómo la IA genera el mapa de percepción
   - Cómo se detectan divergencias con el autodiagnóstico
   - Cómo se sugieren preguntas para la siguiente O2O
   Pipeline de vista de red:
   - Cómo se computan los patrones agregados entre instancias conectadas
   - Cómo se garantiza que nunca se exponen datos individuales de otras instancias
   - Cómo se gestiona el umbral de masa crítica
   Pipeline de CV y documentos.
   Sistema de prompts: estructura del system prompt para cada pipeline, cómo viaja el contexto GRETA en cada caso.

5. ARQUITECTURA DE SEGURIDAD
   - Modelo de amenazas específico para GRETA SaaS (datos sensibles, evaluaciones anónimas, red entre instancias)
   - Controles de seguridad por capa
   - Garantías técnicas del anonimato en evaluaciones (no solo promesas — implementación)
   - Garantías técnicas del aislamiento en la red (cómo se garantiza que instancia A nunca ve datos raw de instancia B)
   - KMS y rotación de claves
   - Auditoría y logging
   - Flujo de derecho al olvido GDPR (incluyendo datos anónimos de evaluaciones)
   - Plan de respuesta ante breach

6. INFRAESTRUCTURA Y DEVOPS
   - Provisioning automático de instancia
   - Environments (dev, staging, prod)
   - CI/CD
   - Monitorización (métricas de red, patrones de uso, alertas de masa crítica)
   - Control de costes por instancia y por red
   - Backup y recuperación

OUTPUT ESPERADO:
- Documento de arquitectura con todos los ADRs
- Diagrama en formato Mermaid
- Schema de base de datos completo
- Schema del grafo completo
- Pipelines de IA documentados (assessments, guardarraíles, evaluaciones, red)
- Implementación técnica del anonimato estructural
- Implementación técnica de la federación entre instancias
- Lista de componentes con tecnología y justificación
- Estimación de esfuerzo por capa (XS/S/M/L/XL)
- Riesgos técnicos con mitigación
```

---

### PROMPT 4 — Plan de Historias de Usuario

```
Actúa como agente de planificación.

Lee el documento maestro completo. Tienes el contexto de GRETA, los requisitos, el discovery y la arquitectura. Úsalos todos.

FORMATO DE CADA HU:
- **ID:** [ÉPICA]-[NÚMERO]
- **Título:** breve y descriptivo
- **Historia:** Como [actor], quiero [acción], para [valor/objetivo]
- **Criterios de aceptación:** lista verificable (mínimo 3, máximo 8)
- **Dependencias:** IDs de HUs previas
- **Estimación:** XS / S / M / L / XL
- **Notas técnicas:** consideraciones de implementación

ÉPICAS REQUERIDAS:

**AUTH — Autenticación y gestión de instancia**
Registro, creación automática de instancia, login, configuración de cuenta, borrado completo GDPR.

**PEOPLE — Gestión de equipos y personas**
Crear/editar/archivar equipos, dar de alta personas, gestionar relación persona-equipo, campos personalizados.

**PROFILE — Ficha de persona**
CV upload y procesamiento con IA, logros y contribuciones, campos adicionales, sugerencias de preguntas de seniority.

**ASSESSMENT — Diagnóstico y niveles**
Assessment manual de las 4 dimensiones con 7 niveles y colores, registro de observaciones, historial de assessments.

**SELF — Autodiagnóstico de quien lidera**
Las 4 dimensiones aplicadas a quien lidera, radar de autodiagnóstico, comparación con mapa de percepción del equipo.

**EVAL360 — Evaluación de liderazgo**
Evaluación proactiva libre (equipo envía feedback anónimo en cualquier momento), evaluación periódica invitada (quien lidera activa ronda), evaluación en tres direcciones (equipo→líder, líder→su líder, equipo→TL), anonimato estructural en cuatro capas, umbral configurable, mapa de percepción agregado, detección de divergencias con autodiagnóstico, sugerencias de preguntas para O2O.

**IMPACT — Capa de impacto**
Reto específico del equipo, definición de outcomes con señales tempranas, hipótesis de contribución individual, experimentos de equipo acotados, registro de evidencias, matriz de priorización valor/riesgo, dashboard de impacto.

**GUARD — Guardarraíles de IA**
Los 9 guardarraíles como funcionalidades independientes, registro de advertencias en historial, tono y formato de cada guardarraíl.

**AI — Procesamiento con IA**
Pipeline RAG para transcripciones, pipeline de CV, sugerencias con razonamiento y citas, validación/rechazo, historial de sugerencias.

**NET — GRETA en red**
Solicitud y aceptación de conexión entre instancias (consentimiento mutuo), tipos de conexión (lateral, hacia arriba, hacia abajo), configuración de qué se comparte, vista del porcentaje de participación, vista del todo con masa crítica, patrones agregados entre instancias conectadas, garantías de que nunca se exponen datos individuales de otras instancias.

**VIZ — Visualización**
Radar chart por persona, evolución temporal, dashboard de equipo, mapa de calor de conocimiento, comparativa entre equipos, vista de grafo, vista de red.

**SHARE — Compartición de vistas**
Vistas compartibles read-only, links temporales o permanentes, revisión por guardarraíl antes de compartir, gestión y revocación de accesos.

**INFRA — Infraestructura y operaciones**
Provisioning automático, cifrado y KMS, backup y recuperación, monitoring, CI/CD, borrado GDPR.

TAREAS DE PLANIFICACIÓN:

1. Crea todas las HUs por épica con el formato definido
2. Define el **MVP mínimo** — debe incluir: assessment básico de las 4 dimensiones, al menos 3 guardarraíles críticos, evaluación de liderazgo básica (una dirección, una modalidad), y autodiagnóstico de quien lidera
3. Define la **Beta** — añade: capa de impacto completa, guardarraíles completos, evaluación en tres direcciones, red entre instancias básica (conexión lateral entre pares)
4. Define la **versión completa** — añade: red multinivel completa, vista del todo, todas las visualizaciones
5. Propón secuencia de sprints de 2 semanas (2-3 developers + 1 diseñadora/diseñador)
6. Identifica HUs de mayor riesgo técnico y propón spikes (especialmente: anonimato estructural, federación entre instancias, guardarraíles en tiempo real)
7. Lista dependencias externas que deben estar configuradas antes de empezar

OUTPUT ESPERADO:
- Lista completa de HUs por épica
- Definición del MVP con justificación y lista de HUs
- Definición de la Beta y la versión completa
- Plan de sprints con HUs por sprint y objetivo
- Lista de spikes técnicos con duración estimada
- Checklist de dependencias externas
- Estimación total de duración hasta MVP, Beta y versión completa
```

---

*Documento maestro GRETA SaaS v1.2 — para uso con Karajan Code*
*Framework base: GRETA v1.0-RC3*
*Referencias: Impact Driven Growth (Carlos Iglesias), idg.runroom.com*
