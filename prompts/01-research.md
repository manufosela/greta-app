# PROMPT 1 — Research

> Antes de leer este prompt, carga `../context-greta.md` (Partes 1 y 2 del documento maestro). Este prompt asume ese contexto.

---

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

---

**Output destination:** `outputs/GRETA-research.md` (en la raíz de greta-app/).
