# GRETA — Research (Fase 1)

> **Fecha del research**: 2026-05-10
> **Autor**: Claude Code (sesión interactiva, fase 1 del documento maestro GRETA)
> **Fuentes**: precios y políticas extraídos de las páginas oficiales de cada proveedor con el contexto disponible en mayo 2026. Anclar precios con fecha + URL es obligatorio porque cambian frecuentemente.

---

## Resumen ejecutivo y recomendación

Tras evaluar las 4 opciones de stack contra los 6 criterios del documento (multi-tenancy, provisioning programático, coste por escenario, ecosistema web+móvil, integración IA, GDPR), **la recomendación es:**

> **Stack: Google Cloud + Firebase/Firestore + Vertex AI** (con Claude vía Vertex AI en `europe-west1` o `europe-west4` para garantizar residencia EU).

Razones principales:
1. **Multi-tenancy nativo y maduro** vía Google Identity Platform (GCIP) — silos de usuarios por tenant, etiquetado de billing por tenant, integrado con Firebase Auth.
2. **EU data residency garantizada para Claude** vía Vertex AI regional endpoints — el Anthropic API directo NO ofrece EU residency en mayo 2026.
3. **CMEK nativo** en Firestore para cifrado en reposo con claves del cliente.
4. **Familiaridad del autor** (criterio explícito del documento maestro) — reduce curva y riesgo de implementación.
5. **Coste razonable** hasta el escenario "Crecimiento" (10–100 líderes); el escenario "Escala" (100–1000) requiere revisión de arquitectura pero ningún stack lo resuelve trivialmente.

**Decisiones secundarias dentro del stack:**
- **Grafo**: empezar con Firestore + colecciones de unión (junction collections) para el MVP. Migrar a Neo4j Aura SOLO si las consultas de la "Vista de grafo" se vuelven cuello de botella. Coste evitado: ~$80/mes/instancia mínimo en Aura Professional.
- **Multi-tenancy físico**: una `database` por instancia dentro del mismo proyecto Firebase (Firestore soporta múltiples databases nombrados desde 2024). Aislamiento por reglas + por database, NO compartir colecciones.
- **Vector store**: Vertex AI Vector Search (antes Matching Engine) con namespace por instancia. Alternativa: Firestore Vector Search (GA en 2025) si el volumen es bajo y se quiere reducir piezas.
- **CV parsing**: LLM directo (Claude Haiku vía Vertex) en lugar de Affinda/Sovren — ahorro de ~$0.13/parse + control total + español nativo.
- **Web + móvil**: PWA con **Astro + Lit + JS vanilla (JSDoc/.d.ts)** + Capacitor para distribución en stores cuando convenga. UI construida sobre el **catálogo de componentes `@manufosela/*`** (web components Lit). Apps nativas (Flutter/React Native) sólo si después del MVP se ve necesidad de notificaciones push iOS robustas u offline avanzado.

**Riesgo nº 1 a vigilar**: el coste per-MAU de GCIP escala mal si cada instancia tiene cientos de usuarios. Para GRETA esto NO aplica (cada instancia tiene 1 líder + N personas evaluadas que NO acceden a la app), pero conviene confirmarlo en discovery (¿las personas evaluadas tienen cuenta? No — sólo el líder, pero si la posibilidad de compartir ciertas vistas de dashboard o algunas metricas que no impliquen desvirtuar el framework porque impliquen competencia o manipulación del mismo).

---

## 1. Stack para SaaS multi-tenant con instancias por usuario

### 1.1 Comparativa de las 4 opciones

| Criterio | GCP + Firebase + Vertex AI | AWS + Amplify + Bedrock | Supabase + Anthropic API | Cloudflare + D1 + Workers AI |
|---|---|---|---|---|
| **Multi-tenancy** | GCIP nativo (silos por tenant + billing por tenant) | Cognito user pools + AppSync | Postgres + RLS + branching | Pendiente, joven |
| **Provisioning programático** | Firebase Admin SDK + GCIP REST API | Amplify CLI + Cognito SDK | Management API + branches | Wrangler + KV |
| **EU data residency LLM** | ✅ Claude vía Vertex `europe-west1` | ✅ Claude vía Bedrock EU | ❌ Anthropic directo NO ofrece EU | Limitado |
| **Vector store integrado** | Vertex Vector Search + Firestore Vector | Bedrock Knowledge Bases + OpenSearch | pgvector | Vectorize |
| **GDPR compliance** | DPA, CMEK, EU residency completa | DPA, EU residency | DPA, EU available | DPA, EU |
| **Madurez ecosistema** | Muy alta (Firebase 10+ años) | Muy alta | Alta (joven pero solvente) | Media |
| **Familiaridad autor** | ✅ explícita | media | media | baja |
| **Stack móvil** | FlutterFire / RN Firebase / iOS+Android SDK | Amplify libraries / RN | RN + Capacitor | Capacitor |
| **Sweet spot de coste** | 1–100 instancias; >500 problemático | 10–500; <10 cara para Neptune | 1–50 sweet spot | Nicho |
| **Riesgo lock-in** | Alto (Firebase específico) | Alto | Medio (Postgres-compatible) | Alto |

### 1.2 Detalle por opción

#### GCP + Firebase + Vertex AI (recomendada)

- **Multi-tenancy**: [GCIP multi-tenancy](https://cloud.google.com/blog/products/identity-security/multi-tenancy-support-identity-platform-now-generally-available) crea silos lógicos dentro de un solo proyecto, con configuración de auth, providers y branding independiente por tenant. Billing trazado mediante el label `goog-identitytoolkit-tenant`.
- **Provisioning**: crear tenant es 1 llamada REST. Crear database Firestore por tenant es 1 llamada al [`databases.create`](https://docs.cloud.google.com/firestore/native/docs/manage-databases). Cloud Function de signup hace ambas cosas en <2s.
- **Coste auth**: 50K MAU gratis. Después $0.0055/MAU para auth básica, $0.015/MAU para SAML/OIDC. Para GRETA en escenario Early adopter (1–10 líderes), el auth es prácticamente gratis. ([Identity Platform pricing](https://cloud.google.com/identity-platform/pricing))
- **Coste Firestore**: por reads/writes/storage; ~$0.06/100K reads, $0.18/100K writes, $0.18/GB-mes. Una instancia GRETA típica (5–50 personas, ~100 assessments) ocupa <50MB → ~$0.01/mes en almacenamiento.
- **CMEK**: [Firestore CMEK](https://firebase.google.com/docs/firestore/cmek) está GA. Una clave KMS por instancia → cifrado a nivel database. **Limitación importante**: NO es field-level. Para field-level (transcripciones de O2O, assessments emocionales) hay que cifrar en el cliente con envelope encryption antes de escribir → ver sección Cifrado.
- **Claude EU**: [Vertex AI partner-models — Claude](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/partner-models/claude) garantiza EU residency en `europe-west1` (Bélgica), `europe-west4` (Países Bajos) y otros. Los modelos disponibles incluyen Claude Sonnet 4.6, Claude Haiku 4.5 y Claude Opus 4.7.

#### AWS + Amplify + Bedrock (alternativa válida)

- Más fragmentado: Amplify para frontend+API, Cognito para auth, DynamoDB o Aurora para datos, Bedrock para IA, KMS para cifrado.
- **Bedrock con Claude EU**: igual de bien que Vertex AI. Mismos precios que Anthropic directo + 10% si cross-region inference. Sonnet 4.6: $3/M input, $15/M output ([AWS Bedrock pricing](https://aws.amazon.com/bedrock/pricing/)).
- **Cognito multi-tenant**: posible con user pools por tenant o app clients por tenant. Más complejo que GCIP.
- **Penalización principal**: Amazon Neptune (grafo nativo) cuesta ~$1.92/h en Frankfurt → **$1.382/mes mínimo** ([Amazon Neptune pricing](https://aws.amazon.com/neptune/pricing/)). Inviable para "una instancia por líder". Opción Neptune Serverless suaviza pero sigue siendo cara para tenants pequeños inactivos.

#### Supabase + Anthropic API (descartada por GDPR)

- Sweet spot: Postgres + RLS + Realtime + Storage en un solo SaaS. Pro $25/mes, Team $599/mes con proyectos ilimitados ([Supabase pricing](https://supabase.com/pricing)).
- **Bloqueante GDPR**: el Anthropic API directo solo ofrece `us` y `global` inference geographies — sin EU residency en mayo 2026. La DPA actual es de 1 enero 2026 pero los datos siguen residentes en US ([Anthropic Privacy Center](https://privacy.claude.com/en/articles/7996890-where-are-your-servers-located-do-you-host-your-models-on-eu-servers)).
- **Si igual quisieras Supabase**: tendrías que servir Claude vía AWS Bedrock o Vertex AI desde otro cloud → arquitectura híbrida, complejidad adicional, pierdes parte del beneficio de Supabase.
- **Veredicto**: descartada para GRETA dada la criticidad GDPR de los datos sensibles (transcripciones O2O, assessments emocionales).

#### Cloudflare (mencionada por completitud)

- Joven en este espacio. D1 (SQLite distribuido) y Workers AI son interesantes pero el catálogo de modelos es limitado y la madurez para SaaS multi-tenant con datos sensibles GDPR no está al nivel de los anteriores. **Descartada para producción** pero válida para prototipos.

---

## 2. Base de datos de grafos managed

### 2.1 Comparativa

| Opción | Sweet spot | Coste mínimo / mes | EU residency | Decisión |
|---|---|---|---|---|
| **Neo4j Aura Professional** | Producción ligera | ~$65/mes/instancia ([Neo4j pricing](https://neo4j.com/pricing/)) | Sí (60+ regiones) | Plan B |
| **Neo4j Aura Business Critical** | Producción seria | ~$146/mes/instancia | Sí | Si el volumen crece |
| **Amazon Neptune** | Grandes datasets | $1.382/mes (db.r5d.2xlarge) | Sí (Frankfurt) | ❌ caro para per-tenant |
| **Neptune Serverless** | Cargas variables | ~$0.10/NCU-segundo | Sí | Atractivo si hay picos |
| **Azure Cosmos DB con Gremlin** | Cargas globales | $0.024/RU mínimo | Sí | Plan C |
| **Firestore con junction collections** | MVP / volumen bajo | Incluido en Firebase | Sí | ✅ recomendado para MVP |

### 2.2 Análisis de necesidad real

GRETA modela:
- **Personas** (nodos) — atributos: nombre, rol_belbin, nivel_seniority, etc.
- **Equipos** (nodos) — atributos: nombre, área
- **Proyectos** (nodos) — atributos: cliente, fecha
- **Assessments** (nodos) — atributos: dimensión, nivel, fecha
- **Aristas**: persona ∈ equipo, persona trabajó_en proyecto, persona tiene assessment

Las consultas típicas:
1. *"Qué personas del equipo X tienen nivel Verde+ en dimensión Conocimiento"* → simple SELECT con un JOIN. Firestore lo resuelve.
2. *"Cobertura de roles Belbin en el equipo X"* → agregación sobre la colección `personas` filtrada por `equipo_id`. Firestore lo resuelve con índices compuestos.
3. *"Bus factor por área en el equipo X"* → agregación. Firestore lo resuelve.
4. *"Camino más corto entre persona A y persona B vía proyectos comunes"* → AQUÍ el grafo gana. Pero ¿GRETA realmente necesita esto?

**Veredicto**: para el MVP/Beta, **Firestore con junction collections es suficiente**. Patrón:

```
/instances/{instanceId}/people/{personId}
/instances/{instanceId}/teams/{teamId}
/instances/{instanceId}/projects/{projectId}
/instances/{instanceId}/memberships/{personId_teamId}    # junction
/instances/{instanceId}/contributions/{personId_projectId} # junction
/instances/{instanceId}/assessments/{assessmentId}       # subcolección o top-level
```

Las consultas se diseñan para los patrones de lectura ([artículo de fireship sobre Firestore many-to-many](https://fireship.io/lessons/firestore-nosql-data-modeling-by-example/)).

**Cuándo migrar a Neo4j Aura**:
- La "Vista de grafo" del documento debe permitir navegación interactiva con miles de nodos (cliente con 200+ personas y 50+ proyectos).
- Aparece la necesidad de queries de tipo "shortest path" o "PageRank" sobre el grafo.
- El coste de las consultas en Firestore (lecturas múltiples para reconstruir aristas) supera lo que costaría una Aura Professional.

Para el escenario Early adopter, ese momento NO llega. Para Crecimiento posiblemente. Para Escala casi seguro.

---

## 3. Capa de IA y RAG privada

### 3.1 Comparativa de proveedores LLM

| Proveedor | Modelo recomendado | Precio entrada / salida (M tokens) | EU residency | DPA | Decisión |
|---|---|---|---|---|---|
| **Vertex AI** (Claude) | Claude Sonnet 4.6 | $3 / $15 ([Vertex partner pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing)) | ✅ `europe-west1` | ✅ | **recomendado** |
| **Vertex AI** (Gemini) | Gemini 2.5 Flash | $0.10 / $0.40 (Lite); $1.25/$10 Pro | ✅ EU regions | ✅ | barato para CV parsing |
| **AWS Bedrock** (Claude) | Claude Sonnet 4.6 | $3 / $15 +10% si cross-region | ✅ EU regions | ✅ | alternativa a Vertex |
| **Anthropic API directo** | Sonnet 4.6 | $3 / $15 ([Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing)) | ❌ solo us / global | ✅ | descartado por GDPR |
| **Qdrant Cloud + LLM externo** | propio | $0.078/GB-h ($57/GB-mes RAM) ([Qdrant pricing](https://qdrant.tech/pricing/)) | ✅ EU | ✅ | flexibilidad pero más piezas |

### 3.2 Recomendación de arquitectura RAG

```
[input: transcripción 10K palabras]
        ↓
[chunking: 800 tokens, overlap 100]   ← embedding-aware splitter (LangChain o tiktoken)
        ↓
[embedding: text-embedding-005 en Vertex] (~$0.0001 / 1K tokens)
        ↓
[vector store: Vertex Vector Search]   ← namespace = instanceId → aislamiento físico
        ↓
[retrieval: top-k=5–10 con metadata filter por instanceId]
        ↓
[contexto: GRETA framework (estático, ~3K tokens) + chunks recuperados + input nuevo]
        ↓
[LLM: Claude Sonnet 4.6 vía Vertex AI europe-west1]   ← garantiza EU residency
        ↓
[parser: extracción de sugerencias estructuradas (JSON Schema validado con Valibot/Zod)]
        ↓
[UI: presentar al líder con citas del texto + razonamiento]
```

**Coste estimado por procesamiento de una conversación de ~5K tokens**:
- Embedding: 5K * $0.0001/K = $0.0005
- LLM: 8K input (contexto + chunks + transcript) * $3/M + 2K output * $15/M = $0.024 + $0.030 = **$0.054/conversación**

A 5–20 conversaciones/semana en escenario Crecimiento, son $1–4/líder/mes en LLM. Despreciable.

### 3.3 Calidad para español

- **Claude Sonnet 4.6**: excelente en español, multilingüe nativo, citas precisas con system prompt bien diseñado.
- **Gemini 2.5 Pro**: muy buena, calidad similar a Claude en español según benchmarks de mayo 2026.
- **Sugerencia**: Sonnet para análisis cualitativo (assessments emocionales, motivación, alineamiento — donde matiz importa). Haiku 4.5 o Gemini Flash para extracción estructurada de CVs (mucho más barato).

### 3.4 Aislamiento por instancia en el vector store

Vertex AI Vector Search permite filtros por metadata. Estrategia:
- **Namespace lógico** = `instance_id` como label en cada vector.
- **Filtro obligatorio** en cada query: `instance_id == request.user.instance_id`.
- **Cifrado**: la metadata SÍ es legible por GCP. Para pasar GDPR Art. 9 (ver sección Cifrado), los textos sensibles se cifran con envelope encryption antes de embedderlos NO — eso rompe la búsqueda. La estrategia correcta es: vector + metadata limpia (instance_id, fecha, persona_id), texto original cifrado en Firestore con CMEK + encriptación de campo, recuperación de chunks textuales por id en Firestore.

### 3.5 Pipeline dual: assessments + guardarraíles (nuevo en RC3)

GRETA RC3 §13 define 9 guardarraíles de IA que **avisan sin bloquear**. Conviven con el pipeline de assessments y multiplican la huella LLM. La arquitectura es **dos pipelines independientes** sobre el mismo proveedor (Claude vía Vertex), no un único pipeline más grande:

| Pipeline | Disparador | Modelo recomendado | Frecuencia esperada (instancia Crecimiento) | Coste estimado |
|---|---|---|---|---|
| Assessments — análisis de transcripción | Onboarding de conversación O2O | **Sonnet 4.6** (matiz importa) | ~10–20 / mes | $0.05 / llamada |
| Guardarraíl 1 — outcome equipo | Crear/editar outcome | **Haiku 4.5** | ~5–10 / mes | $0.002 / llamada |
| Guardarraíl 2 — contribución individual | Crear/editar hipótesis de contribución | **Haiku 4.5** | ~15–30 / mes | $0.002 / llamada |
| Guardarraíl 3 — evidencia | Registrar evidencia (alta frecuencia) | **Haiku 4.5** | ~50–150 / mes | $0.0015 / llamada |
| Guardarraíl 4 — análisis de transcripción | onSave junto al assessment | **Sonnet 4.6** (reutiliza contexto) | mismo que assessments | incluido en assessments |
| Guardarraíl 5 — base observacional | Asignar nivel manualmente | **lógica determinista + Haiku para mensaje** | ~5–20 / mes | $0.001 / llamada |
| Guardarraíl 6 — revisión de vista compartida | Antes de generar link | **Haiku 4.5** | ~2–5 / mes | $0.003 / llamada |
| Guardarraíl 7 — bajadas de nivel | Cron tras cada nuevo assessment | **lógica determinista + Haiku para narrativa** | igual que assessments | $0.001 / llamada |
| Guardarraíl 8 — sesgo de atención | Cron diario por instancia | **lógica determinista (sin LLM)** | 1 / día / instancia | ~$0 (cómputo Firestore) |
| Guardarraíl 9 — interpretación comparativa del mapa | Detector de patrones de uso UI + LLM si dispara | **Haiku 4.5** | ~1–5 / mes | $0.002 / llamada |

**Decisión de inline vs onSave**: los guardarraíles 1, 2 y 3 se evalúan **al guardar**, NO mientras el usuario escribe. Razones: (a) coste, una evaluación por keystroke sería 50–100× más caro; (b) UX, una crítica mientras escribes interrumpe el flujo; (c) los guardarraíles deben tener una versión final del texto para evaluar honestamente. La excepción puede ser un "lint" determinista pre-LLM (regex/keywords de actividad — "entregar", "número de", "completar 10") que advierte sin esperar al guardado. El LLM solo se invoca al onSave.

**Decisión Haiku para guardarraíles**: el 80% de los guardarraíles son clasificación + reformulación corta, donde Haiku 4.5 es ~3× más barato que Sonnet y la calidad es suficiente. Sonnet se reserva para análisis de transcripción y casos donde matiz/citas importan (guardarraíl 4).

**Coste adicional dual pipeline / instancia Crecimiento**: ~$3–6 / mes adicionales sobre los $20–40 / mes ya estimados para LLM. Total LLM revisado: **~$25–50 / instancia Crecimiento**. Marginal.

**Latencia**: cada guardarraíl Haiku añade 200–500ms. Aceptable en onSave (el usuario ya espera persistencia). Si los guardarraíles 1–3 se hacen **asincrónicamente tras guardar** y la advertencia aparece como toast/badge en segundo plano, la latencia percibida es 0.

---

## 4. Procesamiento de CV y documentos

### 4.1 Opciones evaluadas

| Opción | Precio | Pros | Contras |
|---|---|---|---|
| **Affinda Resume Parser** | $800/año por 6K parses → $0.13/parse, hasta $18K/780K. ([Affinda pricing](https://www.affinda.com/recruitment-ai-pricing)) | Especializado, 50+ idiomas, español OK, salida estructurada | Coste por uso, vendor adicional |
| **Sovren / Textkernel** | $99/mes profesional ([Sovren](https://www.datatobiz.com/blog/hirelakeai-vs-affinda-vs-sovren/)) | Velocidad, 29 idiomas con dialectos | Coste fijo aunque no se use |
| **LLM directo (Claude Haiku vía Vertex)** | $0.80 / M input + $4 / M output | Mismo proveedor que el resto del pipeline, salida JSON personalizable, español nativo | Necesita prompt engineering para extracción robusta |
| **Gemini 2.5 Flash-Lite** | $0.10 / $0.40 por M | Aún más barato | Calidad ligeramente inferior para texto complejo |

### 4.2 Recomendación

**LLM directo** (Claude Haiku 4.5 vía Vertex). Razones:
- Un CV típico = 2K–5K tokens. Coste: ~$0.005/CV. Affinda costaría $0.13 — 25× más caro.
- Una sola pieza menos en el stack.
- Salida JSON modelable a las necesidades exactas de GRETA (proyectos, retos, situaciones gestionadas, áreas técnicas con nivel inferido).
- Si la calidad no basta tras pruebas, fallback a Affinda solo para CVs problemáticos.

**Pipeline propuesto**:
```
PDF/DOCX → pdf-parse / mammoth → texto plano → Haiku con prompt "extraer JSON con schema X"
                                                    ↓
                                       Vector embeddings de logros para RAG futuro
```

---

## 5. Web y móvil

### 5.1 Comparativa

| Opción | Coste relativo | iOS push | Offline | Stores | Sweet spot |
|---|---|---|---|---|---|
| **PWA pura** (Astro + Lit + Workbox) | 1× | ❌ Limitado en iOS | Bueno con SW | No | MVP rápido |
| **PWA + Capacitor** | 1.2× | ✅ vía wrapper nativo | Bueno | ✅ | **Recomendado MVP/Beta** |
| **React Native + Expo** | 1.5× | ✅ | Excelente | ✅ | Cuando móvil sea crítico |
| **Flutter** | 1.7× | ✅ | Excelente | ✅ | UI premium animada |
| **Apps nativas separadas** | 3× | ✅ | Excelente | ✅ | Sólo si lo anterior falla |

### 5.2 Recomendación

**PWA primero (Astro + Lit) + Capacitor para distribución iOS/Android cuando convenga.**

Argumentos:
- El uso primario de GRETA es **escritorio** (líder revisa el dashboard, el grafo, sube CVs/transcripciones). El móvil es secundario (consulta puntual del radar de una persona, anotación rápida).
- **Las funcionalidades críticas (carga de mapas de equipo offline, notificaciones de seguimiento) NO son críticas para el MVP**. Se pueden añadir cuando haya señal de mercado.
- Capacitor permite empaquetar la PWA como app iOS/Android con plugins nativos para push, biometría, etc. — la curva es 1 sprint adicional vs reescribir en RN/Flutter.
- Si después del MVP los usuarios piden push iOS robusto + offline avanzado, migrar a Expo es factible (mismo lenguaje JS).

### 5.3 Stack frontend concreto

**Lenguaje y meta-framework:**
- **Astro** para la app y la landing pública. Páginas estáticas en `.astro` puro; **islas hidratadas** sólo cuando hay interactividad real (dashboard, radar, grafo, formularios reactivos).
- **Lit** (web components estándar) para todo componente con estado o reactividad. Cada componente vive en su propio módulo y se exporta del catálogo.
- **JS vanilla** — **NO** TypeScript. Tipado mediante **JSDoc** + ficheros `.d.ts` cuando se necesite contratos públicos (props de componentes, schemas de servicios). VS Code valida sin paso de compilación.

**Catálogo de componentes — `@manufosela/*` (regla obligatoria del proyecto):**
- **Cualquier componente del dashboard** (radar, grafo, mapa de calor, dashboard de equipo, evolución temporal, vista compartida, modales, formularios reactivos, tablas, etc.) **se construye exclusivamente con componentes del catálogo `@manufosela/*`** (web components Lit). Si el componente que necesitamos ya existe → se importa por npm. Si no existe → se diseña y publica primero en el catálogo, y luego se importa en GRETA. **GRETA no contiene componentes Lit "huérfanos" propios**; cada uno tiene su sitio en el catálogo.
- **Componentes ya cubiertos por el catálogo (a confirmar al integrar):** `radar-chart` (✓ existe — se usará para el radar de las 4 dimensiones GRETA), y otros que iremos descubriendo al inventariar.
- **Procedimiento al detectar uno nuevo:** (a) revisar primero en el catálogo si ya existe algo cercano; (b) si no, abrir issue/PR en el repo del catálogo `@manufosela/<nombre>`, diseñar API pública (props, eventos, custom properties CSS para theming), publicar a npm; (c) importar en GRETA y consumir. Esto alimenta la biblioteca para futuros proyectos y evita reinventar.
- **Componentes básicos no reactivos (header, footer, layout, página de error, navegación de marketing, breadcrumbs estáticos, secciones de la landing) → se hacen en Astro nativo (`.astro`)**, sin forzar Lit. Lit se reserva para lo que tenga reactividad, estado de cliente o sea reutilizable entre páginas. Esta separación mantiene Astro haciendo lo que mejor hace (SSR/SSG ligero) y Lit donde aporta (componentes reutilizables con estado, publicables al catálogo).

**Estilos y theming:**
- **CSS vanilla** con custom properties (`--greta-color-rojo`, `--greta-color-violeta`, `--greta-radar-bg`, etc.) en `:root` y `:host` por componente. **NO Tailwind, NO frameworks CSS.**
- **Tematización por tenant** mediante una stylesheet inyectada al cargar la instancia: `<link rel="stylesheet" href="/themes/{tenantId}.css">` que sobreescribe las custom properties. Soporta white-label sin tocar componentes.
- Modo claro/oscuro vía `prefers-color-scheme` y `[data-theme]` con custom properties.

**Estado del cliente:**
- **No Redux/Pinia/Zustand.** Combinación recomendada:
  - **Signals** (`@preact/signals-core` o `@lit-labs/signals`) en módulos `stores/*.js` para estado global ligero (usuario logueado, instancia activa, tema).
  - **Firebase `onSnapshot` directo** dentro de un Lit `ReactiveController` para datos de dominio (personas, equipos, assessments). Firebase ya cachea y diffea — no hace falta capa intermedia tipo TanStack Query.
  - **`CustomEvent`** con `bubbles: true, composed: true` para comunicación efímera entre componentes hermanos.
  - **`@lit/context`** sólo si aparece prop drilling profundo (poco probable en MVP).

**PWA y service worker:**
- **Workbox** para Service Worker (precaché del shell, runtime cache de imágenes/fonts, fallback offline).
- Manifest configurado para installable; iconos por tenant cuando se haga white-label avanzado.

**Visualización (todo del catálogo `@manufosela/*`):**
- **Radar chart de 4 dimensiones** → componente `<radar-chart>` ya existente en el catálogo. Se extiende vía custom properties CSS (`--radar-color-rojo` … `--radar-color-blanco`) para representar los 7 niveles GRETA con sus colores oficiales. Si la API actual no soporta los 7 niveles concéntricos o las leyendas que GRETA necesita, **se amplía en el catálogo, no en GRETA**.
- **Vista de grafo interactiva** (personas ↔ equipos ↔ proyectos) → si existe un `<graph-view>` o equivalente en el catálogo, se usa; si no, se publica uno (probablemente envolviendo Cytoscape.js).
- **Evolución temporal por persona, dashboard de equipo, mapa de calor de conocimiento, comparativa entre equipos, vistas compartibles read-only** → cada una corresponde a un componente del catálogo, existente o por crear. Inventario y gap analysis se hacen como **primer task de Discovery (fase 2)**.
- Las librerías subyacentes (D3, Cytoscape, Plotly, etc.) son **detalle de implementación interno del catálogo**, NO dependencias directas de GRETA.

**Móvil:**
- **Capacitor 7+** para empaquetar la PWA como app iOS/Android cuando se quiera distribuir en stores. Plugins nativos a demanda (push, biometría, share). Sin reescritura del código.

---

## 6. Cifrado, privacidad avanzada y GDPR

### 6.1 Categorización GDPR de los datos GRETA

Los assessments emocionales y transcripciones de O2O **pueden caer en Art. 9 GDPR (datos especiales)** si revelan:
- Estado de salud mental (motivación baja sostenida puede inferir burnout)
- Convicciones políticas/religiosas (transcripciones libres pueden contenerlo accidentalmente)
- Datos sindicales (conversaciones laborales)

Aunque la categorización exacta dependa del contenido concreto, **la guía conservadora es tratarlos COMO si fueran Art. 9** ([ICO — special category data](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/lawful-basis/special-category-data/what-are-the-rules-on-special-category-data/)). Esto implica:

1. **Base legal Art. 6** + **condición Art. 9(2)**. La condición típica para evaluación de empleados es Art. 9(2)(h) — "evaluación de la capacidad de trabajo del empleado", siempre que se haga en marco contractual con profesional de salud O cumpliendo Member State law. Para GRETA B2B esto NO encaja porque no hay profesional de salud.
2. **Alternativa**: **consentimiento explícito** Art. 9(2)(a) de la persona evaluada. Esto requiere flujo explícito — la persona evaluada debe consentir que sus datos cualitativos se procesen así.
3. **DPIA obligatoria** ([Art. 35 GDPR](https://gdpr-info.eu/art-35-gdpr/)) para cualquier tratamiento de alto riesgo con datos especiales.

**Consecuencia para el SaaS**: hay un caso de uso de producto que el documento maestro NO resuelve: ¿cómo se obtiene consentimiento de las personas evaluadas? Esto se eleva a discovery.

### 6.2 Estrategia de cifrado por capas

| Capa | Mecanismo | Ámbito |
|---|---|---|
| **En tránsito** | TLS 1.3 obligatorio | todo |
| **En reposo (database)** | Firestore CMEK con clave KMS por instancia | toda la base de la instancia |
| **En reposo (Storage)** | Cloud Storage CMEK con la misma clave | CVs, transcripciones, archivos sensibles |
| **En reposo (campo sensible)** | Envelope encryption en cliente con clave derivada por instancia | transcripciones de O2O, notas emocionales, comportamientos |
| **Backups** | CMEK aplicado automáticamente | sí |

### 6.3 Field-level encryption — implementación

Firebase CMEK NO es field-level ([Firebase CMEK docs](https://firebase.google.com/docs/firestore/cmek)). Para los campos más sensibles:

```js
// Pseudocódigo: cifrado en el cliente (Cloud Function o frontend autenticado)
const dek = await kms.generateDataKey({ keyName: instanceKey })
const ciphertext = aesGcm.encrypt(transcripcion, dek.plaintext)
const wrapped = dek.ciphertext  // ya cifrado por la KEK
await firestore.set({
  instance_id, person_id,
  ciphertext, wrapped_dek: wrapped, iv, alg: "AES-256-GCM",
  // metadata legible: solo person_id, fecha, hash truncado para deduplicación
})
```

Para descifrar en lectura: `kms.decrypt(wrapped_dek)` → AES-GCM decrypt. Coste KMS: $0.03 por 10K operaciones — despreciable.

**Beneficio**: si Google sufre breach del bucket Firestore, los datos cifrados en campo NO son legibles sin la clave KMS (que se controla por IAM). Defense-in-depth real.

### 6.4 Auditoría y derecho al olvido

- **Cloud Audit Logs** activado en KMS (cada `decrypt` queda registrado).
- **Firestore audit logs** activados → quién leyó qué documento y cuándo.
- **Derecho al olvido**: borrado en cascada por `instance_id`. Cloud Function que reciba el request marca la instancia como "pending_deletion", encola un job que:
  1. Marca todas las claves KMS de la instancia como `disabled` (datos ya inaccesibles).
  2. Borra Firestore database completa.
  3. Borra Storage buckets de la instancia.
  4. Borra vectors en Vertex Vector Search filtrados por `instance_id`.
  5. Tras 30 días (ventana de recuperación), `destroy` de las KMS keys.
  6. Tracking GDPR: log irrevocable del borrado completo.

### 6.5 Otras consideraciones

- **Residencia de datos**: todo en `europe-west1` o `europe-west4`. Configurar el proyecto con default region EU.
- **Subprocesadores**: Anthropic, vía Vertex, es subprocesador de Google. Listado en GCP DPA. Documentar en RoPA del SaaS.
- **DPIA**: hacer una al inicio del proyecto. Plantilla CNIL/AEPD/ICO. Adjuntar al pack legal del SaaS.

---

## 7. Modelo de costes y pricing

### 7.1 Modelo de costes por escenario

**Escenario Early adopter** (1–10 líderes, 5–50 personas/instancia, 2–5 conversaciones/semana):

| Componente | Coste mensual |
|---|---|
| Firestore (lecturas, escrituras, storage) | $1–5 / instancia |
| Firebase Auth (50K MAU gratis) | $0 |
| Vertex AI LLM (Sonnet, ~80 conv/mes/líder) | $5 / instancia |
| Vertex AI Vector Search | $1–2 / instancia |
| Vertex AI Guardarraíles (Haiku, RC3) | $1–2 / instancia |
| Cloud Storage (CVs + transcripciones) | $1 / instancia |
| KMS (CMEK + envelope) | $0.5 / instancia |
| Cloud Functions (signup, processing) | $1 / instancia |
| Logging, monitoring | $1 / instancia |
| **Total / instancia** | **~$11–17** |
| **Total escenario (10 instancias)** | **~$110–170** |

**Escenario Crecimiento** (10–100 líderes, 10–100 personas/instancia, 5–20 conv/semana):

| Componente | Coste mensual |
|---|---|
| Firestore | $5–15 / instancia |
| Vertex AI LLM assessments (Sonnet) | $20–40 / instancia |
| Vertex AI Guardarraíles (Haiku, RC3) | $3–6 / instancia |
| Vertex AI Vector Search | $5 / instancia |
| Resto | $5–10 / instancia |
| **Total / instancia** | **~$38–76** |
| **Total escenario (50 instancias media)** | **~$1.900–3.800** |

**Escenario Escala** (100–1000 líderes, 10–200 personas/instancia, variable):

| Componente | Coste mensual (por instancia activa) |
|---|---|
| Firestore | $20–80 |
| Vertex AI LLM | $50–200 |
| Vector Search | $10–30 |
| Resto | $10–20 |
| **Total / instancia activa** | **$90–330** |
| **Total (500 instancias)** | **$45.000–165.000** |

A esa escala hay que negociar **discounts** con Google (Committed Use Discounts) — fácilmente -30%.

### 7.2 Pricing del SaaS (propuesta)

3 tiers + add-ons:

| Plan | Precio/mes | Incluye |
|---|---|---|
| **Solo Leader** | $49 | 1 instancia, hasta 25 personas, 20 conversaciones IA/mes, vistas read-only ilimitadas |
| **Team** | $149 | 1 instancia, hasta 100 personas, 100 conversaciones IA/mes, 5 vistas custom |
| **Org** | $499 | Hasta 5 instancias (líderes), 200 personas/inst, 500 conv IA, SSO, audit logs |
| **Enterprise** | custom | Multi-cuenta, SLA, DPO, IaC custom |

Margen estimado: 50–70% en Solo/Team, 40–60% en Org. Sostenible.

**Add-ons opcionales**:
- Conversaciones IA extra: $0.50/conversación
- Almacenamiento extra (>5GB): $0.20/GB-mes
- Vistas compartidas con dominio propio: $20/mes

---

## 8. Riesgos técnicos identificados

| # | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| 1 | Anthropic API directo NO ofrece EU residency en mayo 2026 | Cierta | Alto | **Decisión taken**: usar Claude vía Vertex AI o Bedrock |
| 2 | Precios de cloud y modelos cambian rápido | Alta | Medio | Anclar precios en CHANGELOG con fecha; revisar trimestral |
| 3 | Coste por instancia en Escala (>100 líderes) puede dispararse | Media | Alto | Plan: revisar arquitectura cuando se llegue a 100 instancias activas |
| 4 | Datos GRETA pueden caer en GDPR Art. 9 | Alta | Alto | DPIA obligatoria + flujo de consentimiento explícito de personas evaluadas |
| 5 | Firestore CMEK es database-level, no field-level | Cierta | Medio | Envelope encryption en cliente para campos especialmente sensibles |
| 6 | Vertex AI Vector Search requiere namespace correcto para aislamiento | Cierta | Crítico | Gates de revisión en pipeline + tests automatizados de aislamiento |
| 7 | Multi-tenant via "una database por instancia" tiene límites de quotas Firebase | Media | Medio | Verificar quotas (databases por proyecto) en signup; migrar a múltiples proyectos si necesario |
| 8 | Vendor lock-in con Firebase | Cierta | Medio | Diseñar capa de repositorio/DAL que abstraiga Firestore; coste de salida ~6 meses de desarrollo |
| 9 | LLM CV parsing en español puede tener variabilidad de calidad | Media | Bajo | Test con corpus real al principio; fallback a Affinda si <90% precisión |
| 10 | Firebase Auth + GCIP MAU billing puede ser cara si las personas evaluadas también son MAUs | Baja para GRETA | Alto | **Decisión a tomar en discovery**: las personas evaluadas NO tienen cuenta — solo el líder. Confirmar. |
| 11 | Notificaciones push iOS limitadas en PWA | Cierta | Bajo (no MVP) | Capacitor wrapper si MVP necesita push; migrar a RN/Flutter si app móvil se vuelve crítica |
| 12 | Vertex AI EU regions tienen menor catálogo de modelos que US | Media | Bajo | Comprobar `europe-west1` y `europe-west4` específicamente al integrar |
| 13 | RC3 — Pipeline dual (assessments + 9 guardarraíles) multiplica la huella LLM por ~1.2-1.5× | Cierta | Bajo en coste, Medio en latencia | Usar Haiku para guardarraíles, lógica determinista para 5/7/8, evaluar onSave en lugar de inline, ejecución asincrónica con feedback toast |
| 14 | RC3 — Hipótesis de contribución y experimentos tienen ciclos de vida (defined / refuted / confirmed / consolidated) que el modelo de datos previo no contemplaba | Cierta | Medio | Discovery y architecture añaden las entidades `Challenge`, `EarlySignal`, `Experiment` y refactor de `Contribution` con `hypothesis_reason` y `status` |

---

## 9. Decisiones de producto y preguntas abiertas que el research NO ha podido responder

Estas se elevan al **prompt 2 (Discovery)** para resolver:

1. **¿Las personas evaluadas tienen cuenta en GRETA?** Muy probablemente no — el documento dice "visible solo por la persona propietaria de la instancia". Confirmar implica: GRETA NO necesita gestión de identidad de las personas evaluadas, solo del líder y de los observadores compartidos. Crítico para el modelo de costes (MAU) y el flujo de consentimiento GDPR.
2. **¿Cómo se obtiene el consentimiento Art. 9 de las personas evaluadas?** No lo resuelve el documento. Opciones: el líder es responsable contractualmente (con guidance de la app), GRETA proporciona un flujo de invitación + consentimiento (mucho más complejo), o se opera bajo Art. 6(1)(f) interés legítimo + DPIA documentada (riesgo legal).
3. **¿Las "vistas read-only compartidas" requieren autenticación?** El documento dice "sin acceso a la app completa" — pero no especifica si el destinatario tiene credenciales o si es link firmado/temporal sin auth.
4. **¿El historial de assessments tiene retención indefinida?** El documento dice "el diagnóstico caduca" pero no define política de retención.
5. **¿La importación desde LinkedIn está prevista para MVP?** Significativo legalmente (LinkedIn ToS) y técnicamente.
6. **¿La aplicación móvil es realmente necesaria para MVP o puede esperar a Beta?** El criterio de elección PWA vs nativa depende.
7. **¿Los datos de assessment pueden salir de la EU bajo ningún supuesto?** Decisión binaria que afecta a la elección de región y a la lista de subprocesadores.
8. **¿Los guardarraíles 1–3 se ejecutan inline (mientras el usuario escribe) o solo onSave (al guardar)?** Recomendación del research: **onSave + asincrónico con toast** por coste y UX. Confirmar en discovery la latencia aceptable y si algún guardarraíl crítico debe bloquear visualmente hasta tener veredicto.
9. **¿Los guardarraíles 5, 7 y 8 deben usar LLM o son puramente determinísticos?** Recomendación del research: lógica determinista (umbrales + reglas) + Haiku solo para generar la narrativa cuando se dispara. Confirmar.

---

## 10. Resumen de decisiones tomadas en este research

| Decisión | Resolución |
|---|---|
| Stack principal | **GCP + Firebase/Firestore + Vertex AI** |
| LLM principal | **Claude Sonnet 4.6 vía Vertex AI** en `europe-west1` |
| LLM económico (CV) | **Claude Haiku 4.5 vía Vertex** o **Gemini 2.5 Flash-Lite** |
| Vector store | **Vertex AI Vector Search** con namespace por instancia |
| Grafo (MVP) | **Firestore con junction collections** |
| Grafo (post-MVP, si necesario) | **Neo4j Aura Professional** |
| Auth | **Google Identity Platform (GCIP)** con multi-tenancy |
| Cifrado en reposo | **CMEK por database** + **envelope encryption** para campos sensibles |
| Frontend | **Astro + Lit + JS vanilla (JSDoc/.d.ts)** sobre el catálogo `@manufosela/*`; CSS vanilla con custom properties + tematización por tenant; Signals + `onSnapshot` para estado |
| Componentes UI | **`@manufosela/*` como única fuente para componentes del dashboard.** `radar-chart` ya existe; el resto se inventaría en Discovery. Componentes nuevos → publicar primero al catálogo, luego importar. Básicos no reactivos → Astro nativo, sin forzar Lit |
| Móvil | **PWA + Capacitor** para MVP/Beta |
| CV parsing | **LLM directo (Claude Haiku)** en lugar de Affinda/Sovren |
| Pipeline dual IA (RC3) | **Assessments con Sonnet + Guardarraíles 1–4 / 6 / 9 con Haiku + Lógica determinista para 5 / 7 / 8.** Evaluación **onSave + asincrónica** (no inline). Coste adicional: $1–6/instancia/mes según escenario |
| Pricing del SaaS | 3 tiers ($49 / $149 / $499) + add-ons |

---

*Fin del Research. Siguiente fase: Discovery (Prompt 2).*

## Sources

- [Identity Platform pricing | Google Cloud](https://cloud.google.com/identity-platform/pricing)
- [Firebase Pricing](https://firebase.google.com/pricing)
- [Firebase Authentication Pricing 2026 | RapidNative](https://www.rapidnative.com/blogs/firebase-authentication-pricing)
- [Anthropic Claude EU residency | Privacy Center](https://privacy.claude.com/en/articles/7996890-where-are-your-servers-located-do-you-host-your-models-on-eu-servers)
- [Anthropic Claude on Vertex AI](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/partner-models/claude)
- [Anthropic Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- [Neo4j Aura Pricing](https://neo4j.com/pricing/)
- [Neo4j Aura billing dimensions](https://neo4j.com/docs/aura/billing/billing-dimensions/)
- [Amazon Neptune Pricing](https://aws.amazon.com/neptune/pricing/)
- [AWS Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/)
- [Vertex AI Pricing | Google Cloud](https://cloud.google.com/vertex-ai/generative-ai/pricing)
- [Supabase Pricing](https://supabase.com/pricing)
- [Qdrant Cloud Pricing](https://qdrant.tech/pricing/)
- [Affinda Resume Parser Pricing](https://www.affinda.com/recruitment-ai-pricing)
- [Customer-managed encryption keys (CMEK) | Firestore | Firebase](https://firebase.google.com/docs/firestore/cmek)
- [GCIP multi-tenancy GA blog](https://cloud.google.com/blog/products/identity-security/multi-tenancy-support-identity-platform-now-generally-available)
- [GCIP multi-tenancy quickstart](https://docs.cloud.google.com/identity-platform/docs/multi-tenancy-quickstart)
- [GDPR Article 9 — Special category data | ICO](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/lawful-basis/special-category-data/what-are-the-rules-on-special-category-data/)
- [GDPR Article 9 — gdpr-info.eu](https://gdpr-info.eu/art-9-gdpr/)
- [Firestore many-to-many modeling | fireship](https://fireship.io/lessons/firestore-nosql-data-modeling-by-example/)
- [PWA vs Native vs Hybrid 2026 comparison](https://natively.dev/articles/native-hybrid-pwa-comparison)
- [Expo + Web + Native](https://www.appik-studio.ch/en/blog/pwa-vs-native-app-expo-best-choice/)
