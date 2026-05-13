# PROMPT 3 — Architecture

> Antes de leer este prompt, carga `../context-greta.md`, `../outputs/GRETA-research.md` y `../outputs/GRETA-discovery.md`. Este prompt asume todo ese contexto.

---

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

---

**Output destination:** `outputs/GRETA-architecture.md` (en la raíz de greta-app/).
