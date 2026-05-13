# PROMPT 2 — Discovery

> Antes de leer este prompt, carga `../context-greta.md` (Partes 1 y 2 del documento maestro) **y** `../outputs/GRETA-research.md` (output de la fase Research). Este prompt asume ese contexto.

---

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

---

**Output destination:** `outputs/GRETA-discovery.md` (en la raíz de greta-app/).
