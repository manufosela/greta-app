# GRETA — Discovery (Fase 2)

> **Fecha**: 2026-05-10
> **Autor**: Claude Code (sesión interactiva, fase 2 del documento maestro)
> **Inputs**: `master.md`, `context-greta.md`, `outputs/GRETA-research.md` (incluyendo decisiones del autor en el research final).
> **Objetivo**: definir QUÉ se construye — actores, casos de uso, flujos, datos, reglas. NO cómo (eso es Architecture, fase 3).

---

## 0. Resolución previa de las 7 preguntas abiertas del Research

Estas decisiones, **tomadas en este Discovery**, son los cimientos del resto del documento.

| # | Pregunta del Research | Decisión |
|---|---|---|
| 1 | ¿Las personas evaluadas tienen cuenta en GRETA? | **NO**. Sólo los líderes y co-líderes tienen cuenta dentro de la organización (instancia). Las personas evaluadas reciben opcionalmente acceso a su propio radar (Beta). Los observadores externos acceden vía link compartido sin cuenta (MVP) o magic-link (Beta). |
| 2 | ¿Cómo se gestiona GDPR? | **Camino pragmático**: privacy notice estándar entregado UNA vez (template proporcionado), base legal Art. 6(1)(f) interés legítimo del responsable (organización del líder), DPIA documentada. No se requiere consentimiento explícito Art. 9 mientras el contenido se limite a competencias profesionales y comportamientos observados. Si el líder quiere registrar consentimiento explícito (uso muy conservador), `ConsentRecord` opcional disponible. |
| 3 | ¿Vistas read-only requieren autenticación? | **MVP**: link firmado JWT con expiry configurable (default 30 días) y revocable. **Beta**: magic-link con email verificado para trazabilidad. |
| 4 | ¿Retención del historial? | **Configurable por instancia**, defaults: documentos 18 meses, assessments 5 años, sugerencias IA rechazadas 90 días. Derecho al olvido siempre disponible. |
| 5 | ¿LinkedIn import en MVP? | **NO**. Complejidad legal (LinkedIn ToS) + técnica. A reconsiderar en Beta si hay demanda. |
| 6 | ¿Móvil necesario para MVP? | **NO**. PWA web es suficiente. Capacitor para empaquetado iOS/Android en Beta si hay señal. |
| 7 | ¿Datos pueden salir de la EU? | **NO**, sin excepciones. Todo en `europe-west1` o `europe-west4`. |

---

## 1. Actores del sistema

### 1.1 Tipos de usuario

#### 1.1.1 Líder (Owner) — usuario principal de la organización
**Único actor que registra la organización y tiene rol `owner` por defecto.**
- **Puede**: registrarse → crear organización (instancia) automáticamente; gestionar equipos, personas, proyectos; subir CVs y transcripciones; revisar sugerencias de IA y validarlas/ajustarlas/rechazarlas; configurar campos personalizados; configurar tema y branding; generar y revocar vistas compartidas; gestionar suscripción y plan; **invitar co-líderes y revocar accesos**; transferir ownership a otro co-líder; exportar y borrar todos sus datos (GDPR).
- **No puede**: ver datos de otras organizaciones; saltarse el flujo de validación de IA (la IA NUNCA decide directamente); modificar audit logs; eliminar histórico de sugerencias rechazadas antes del periodo de retención (compliance).
- **Ve**: todo lo de su organización + agregados de uso/coste de su plan.
- **No ve**: datos de otras organizaciones; logs internos del SaaS.

#### 1.1.1.b Co-líder (CoLeader) — usuario adicional con permisos de líder
**Cuenta adicional dentro de la misma organización**, invitada por el owner. Útil para hand-over, equipos de liderazgo compartido o respaldo (ej: managers que comparten un equipo).
- **Puede**: todas las acciones del líder owner sobre los datos de la organización (gestionar equipos, personas, validar IA, generar vistas compartidas).
- **No puede por defecto**: gestionar la suscripción, transferir/borrar la organización, expulsar al owner. Estos son privilegios exclusivos del `owner`.
- **Permisos configurables por el owner** (granularidad por épica): un co-líder puede tener acceso completo o limitado a ciertos equipos/personas (configuración avanzada — Beta).
- **Disponible en plan Team o superior**. Plan Solo permite 1 owner solamente.
- **Audit log identifica quién hizo qué** — cada acción registra el `uid` real del actor, no se enmascara.

#### 1.1.2 Observador read-only (Observer) — sin cuenta MVP
- **Puede**: abrir el link compartido y ver SOLO el contenido que el líder seleccionó (vistas agregadas, visualizaciones específicas).
- **No puede**: ver datos sensibles (transcripciones completas, razonamientos detallados de IA sobre dimensión emocional); navegar fuera de la vista compartida; descargar datos sin permiso explícito en la vista.
- **Ve**: lo que el líder configuró + contexto mínimo necesario (nombre del equipo, fecha de generación, "compartido por X").
- **MVP**: sin login, link firmado JWT.
- **Beta**: magic-link con email verificado → audit log enriquecido con identidad del observador.

#### 1.1.3 Persona evaluada (Person) — sin cuenta
- **NO es un usuario del sistema**. Es objeto de evaluación. Sus datos personales (nombre, CV, contribuciones, assessments) se procesan bajo:
  - Consentimiento explícito (`ConsentRecord` registrado por el líder), o
  - Base legal Art. 6(1)(f) interés legítimo del responsable (líder/empresa) + DPIA documentada.
- Tiene **derechos GDPR ejercibles vía el líder o vía solicitud directa al DPO del SaaS**:
  - Acceso a sus datos
  - Rectificación
  - Supresión (derecho al olvido)
  - Portabilidad
  - Oposición al tratamiento
- El SaaS proporciona endpoints/UI para que el líder gestione cualquier solicitud GDPR de una persona.

#### 1.1.4 Administrador del SaaS (Operator/SaaSAdmin) — Geniova/operador
- Cuenta separada en GCIP, NO bajo ninguna instancia de líder.
- **Puede**: gestión de plataforma (provisioning, monitoring, soporte agregado, facturación, métricas globales sin PII).
- **No puede acceder a datos de instancia por defecto**. Acceso a datos de cliente requiere flujo legal documentado (incidencia con consentimiento del líder, request judicial). Cada acceso queda en audit log irrevocable + notificación al líder.
- **Ve**: dashboard de operación (uptime, métricas agregadas, tickets de soporte), facturación.

### 1.2 Sistemas externos

| Sistema | Propósito | Datos que recibe | Datos que devuelve |
|---|---|---|---|
| **GCIP / Firebase Auth** | Auth del líder | email, password/OAuth tokens | UID, sesión, claims |
| **Firestore** | BBDD operacional | todas las entidades de la instancia | docs, snapshots reactivos |
| **Cloud Storage** | Archivos (CVs, transcripciones, exports) | binarios cifrados | URLs firmadas |
| **Vertex AI — Claude Sonnet 4.6** | Análisis cualitativo de transcripciones | contexto RAG + transcripción | sugerencias estructuradas + citas |
| **Vertex AI — Claude Haiku 4.5 / Gemini 2.5 Flash** | CV parsing + sugerencias económicas | texto plano del CV | JSON estructurado |
| **Vertex AI — Embeddings (text-embedding-005)** | Embedding de chunks para RAG | texto plano | vector 768-dim |
| **Vertex AI Vector Search** | Almacén vectorial RAG | vectores + metadata `instance_id` | top-k matches |
| **Cloud KMS** | Claves de cifrado por instancia | requests de cifrado/descifrado | DEKs descifradas |
| **Cloud Functions / Cloud Run** | Pipelines de procesamiento, signup, GDPR | eventos | side effects |
| **Email provider (SendGrid o Postmark)** | Confirmaciones, magic-links, alertas | destinatario + plantilla | confirmación de envío |
| **Stripe** | Suscripciones y facturación | events de pago | webhook events |
| **Cloud Logging / Audit Logs** | Auditoría de accesos a datos sensibles | event payloads | persistencia inmutable |

---

## 2. Mapa de casos de uso por épica

Notación: ID-NNN, descripción, actor primario, precondiciones, postcondiciones.

### 2.1 ÉPICA: ACCOUNT — Cuenta e instancia

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| AC-001 | Registrarse | nuevo líder | email no registrado | LeaderAccount + Instance creadas; KMS key + Vector namespace + Storage bucket provisionados |
| AC-002 | Login | líder | cuenta activa | sesión iniciada; instancia cargada |
| AC-003 | Recuperar contraseña | líder | email registrado | email enviado con magic-link |
| AC-004 | Configurar perfil | líder | sesión iniciada | datos de líder actualizados |
| AC-005 | Configurar tema/branding | líder | sesión iniciada | `Theme` de la instancia actualizado; CSS regenerado |
| AC-006 | Suscripción y plan | líder | sesión iniciada | plan actualizado en Stripe + Firestore |
| AC-007 | Borrar cuenta y datos | líder | confirmación 2-step | instancia → `pending_deletion`; cascada GDPR (ver flujo F-007) |
| AC-008 | Exportar mis datos | líder | sesión iniciada | ZIP firmado (Firestore export + Storage objects descifrados) |
| AC-009 | Configurar política de retención | líder | sesión iniciada | retention policy actualizada |
| AC-010 | Ver dashboard de uso/coste | líder | sesión iniciada | métricas de la instancia (conv IA usadas, storage, etc.) |

### 2.2 ÉPICA: PEOPLE — Equipos y personas

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| PE-001 | Crear equipo | líder | sesión | nuevo `Team` |
| PE-002 | Editar equipo | líder | equipo existe | `Team` actualizado |
| PE-003 | Archivar equipo | líder | equipo sin actividad reciente | `Team.status = archived` |
| PE-004 | Listar equipos | líder | sesión | listado |
| PE-005 | Dar de alta persona | líder | sesión | nueva `Person` |
| PE-006 | Editar persona | líder | persona existe | `Person` actualizada |
| PE-007 | Asignar persona a equipo (M:N) | líder | persona y equipo existen | `TeamMembership` creado |
| PE-008 | Quitar persona de equipo | líder | membership existe | `TeamMembership.end_date` puesto |
| PE-009 | Archivar persona | líder | persona existe | `Person.status = archived` |
| PE-010 | Importar personas desde CSV | líder | CSV válido (template proporcionado) | N personas creadas (atómico — todo o nada) |
| PE-011 | Definir campos personalizados | líder | sesión | `CustomField` creados (max 20 por instancia) |
| PE-012 | Crear proyecto | líder | sesión | nuevo `Project` |
| PE-013 | Asignar persona a proyecto | líder | persona y proyecto existen | `ProjectContribution` creada |
| PE-014 | Registrar consentimiento de persona | líder | persona existe | `ConsentRecord` con soporte (texto/archivo) cifrado |

### 2.3 ÉPICA: PROFILE — Ficha de persona

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| PR-001 | Subir CV | líder | persona existe; archivo PDF/DOCX <10MB | `Document` creado, archivo cifrado en Storage |
| PR-002 | IA extrae datos del CV | sistema (auto al PR-001) | Document creado | `DocumentExtraction` + `AiSuggestion` (proyectos, áreas técnicas, situaciones) |
| PR-003 | Validar/editar/rechazar extracción | líder | AiSuggestion existe | `Person` enriquecida; sugerencia con status final |
| PR-004 | Añadir logros manualmente | líder | persona existe | `Person.achievements` actualizado |
| PR-005 | Rellenar campos personalizados | líder | persona existe; campos definidos | `CustomFieldValue` creados |
| PR-006 | Ver historial de documentos | líder | persona existe | listado de Documents + Extractions |
| PR-007 | Ver sugerencias de preguntas IA | líder | persona tiene historial | listado de preguntas generadas (con timestamp y contexto) |
| PR-008 | Atender derecho de acceso de la persona | líder | request recibida | export de los datos de esa persona |
| PR-009 | Atender derecho al olvido individual | líder | request recibida | borrado en cascada de los datos de esa persona en la instancia |

### 2.4 ÉPICA: ASSESSMENT — Diagnóstico

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| AS-001 | Crear assessment manual | líder | persona existe | `Assessment` con 4 dimensiones, niveles + colores + justificación |
| AS-002 | Editar assessment | líder | assessment existe | nueva versión del Assessment (no se sobreescribe — se mantiene historial) |
| AS-003 | Ver historial de assessments | líder | persona existe | timeline ordenado |
| AS-004 | Comparar dos assessments | líder | 2 assessments existen | diff visual (radar overlay) |
| AS-005 | Justificar nivel con comportamientos | líder | assessment en edición | `Assessment.justifications[dimension]` con texto + tags |
| AS-006 | Marcar assessment como obsoleto (caducidad) | líder | assessment existe | `Assessment.is_stale = true` (visible en UI) |

### 2.5 ÉPICA: AI — Procesamiento con IA

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| AI-001 | Subir transcripción O2O | líder | persona existe; texto pegado o archivo .txt/.md | `Document` cifrado en Storage; pipeline encolado |
| AI-002 | Subir resumen O2O | líder | persona existe | igual que AI-001, marcado como `summary` |
| AI-003 | Pipeline RAG procesa el texto | sistema | Document en cola | chunks → embeddings → retrieve → LLM Claude → JSON output |
| AI-004 | Presentar sugerencias por dimensión | sistema → líder | pipeline completo | UI con sugerencias (nivel, razonamiento, citas), status `pending` |
| AI-005 | Aceptar sugerencia tal cual | líder | sugerencia pending | `AssessmentValidation`; Assessment actualizado |
| AI-006 | Ajustar sugerencia | líder | sugerencia pending | `AssessmentValidation` con `adjusted=true`; Assessment actualizado con valor del líder |
| AI-007 | Rechazar sugerencia | líder | sugerencia pending | `AiSuggestion.status = rejected` con motivo opcional |
| AI-008 | Ver historial sugerencias y validaciones | líder | sesión | analytics de precisión IA por dimensión |
| AI-009 | IA sugiere preguntas para próxima conversación | sistema (offline trigger semanal o on-demand) | persona tiene historial | `PR-007` se rellena |
| AI-010 | Reprocesar transcripción (si se mejora el modelo) | líder | document existe | nuevas sugerencias generadas, marcadas como reprocessed |

### 2.6 ÉPICA: VIZ — Visualización

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| VZ-001 | Ver radar chart de persona | líder | persona tiene assessments | `<radar-chart>` renderizado con 4 ejes y 7 niveles concéntricos coloreados |
| VZ-002 | Ver evolución temporal de persona | líder | persona tiene ≥2 assessments | timeline interactivo |
| VZ-003 | Ver dashboard de equipo | líder | equipo existe | cobertura roles Belbin + bus factor por área + score salud |
| VZ-004 | Ver mapa de calor de conocimiento | líder | equipo existe | matriz personas × áreas técnicas con colores |
| VZ-005 | Comparar equipos | líder | ≥2 equipos | overlay de dashboards |
| VZ-006 | Filtrar por dimensión / nivel / equipo / persona | líder | sesión | vistas filtradas |
| VZ-007 | Exportar visualización (PNG/PDF/SVG) | líder | viz cargada | archivo descargable |

### 2.7 ÉPICA: GRAPH — Vista de grafo

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| GR-001 | Visualizar grafo de la instancia | líder | hay datos | `<graph-view>` con nodos personas/equipos/proyectos |
| GR-002 | Filtrar grafo por dimensión + nivel | líder | grafo cargado | nodos coloreados por la dimensión seleccionada |
| GR-003 | Filtrar por equipo / proyecto | líder | grafo cargado | subgrafo |
| GR-004 | Click en nodo persona → ficha | líder | nodo seleccionado | navega a ficha de persona |
| GR-005 | Layout dinámico (force, hierarchical, circular) | líder | grafo cargado | re-layout |

### 2.8 ÉPICA: SHARE — Compartición read-only

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| SH-001 | Crear vista compartible | líder | sesión | `ShareableView` config (qué se comparte) creada |
| SH-002 | Generar link firmado | líder | ShareableView creada | JWT con `expiry` + `view_id`; URL completa retornada |
| SH-003 | Renovar link (extender expiry) | líder | link existe y no revocado | nueva firma JWT; el anterior queda en blacklist |
| SH-004 | Revocar link | líder | link existe | JWT en blacklist server-side |
| SH-005 | Listar vistas compartidas activas | líder | sesión | listado con expiry, accesos contabilizados |
| SH-006 | Acceso del observador a la vista | observador | link válido y no revocado y no expirado | renderiza la vista en modo `readonly`; loggea acceso |
| SH-007 | Preview de la vista antes de generar link | líder | ShareableView en edición | render del exact-output que verá el observador |
| SH-008 | Magic-link para observador (Beta) | observador | email del observador en allowlist | email enviado, login sin contraseña, sesión limitada al view |

### 2.9 ÉPICA: INFRA — Infraestructura y operaciones

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| IN-001 | Provisioning automático de instancia | sistema (auto en signup) | AC-001 | tenant GCIP + DB Firestore + KMS key + Vector namespace + Storage bucket creados (<5s) |
| IN-002 | Rollback de provisioning fallido | sistema | provisioning falló | recursos parciales eliminados; usuario puede reintentar |
| IN-003 | Backup diario por instancia | sistema (cron) | instancia activa | snapshot Firestore + Storage en bucket de backup, retention 30 días |
| IN-004 | Restore desde backup | admin SaaS (con autorización del líder) | backup existe | instancia restaurada en sandbox antes de overwrite |
| IN-005 | Monitoring + alerting | admin SaaS | sistema activo | métricas en Cloud Monitoring + alertas a oncall |
| IN-006 | Pipeline CI/CD | dev team | repo + tests verde | deploy automático a env staging; promoción manual a prod |
| IN-007 | Borrado completo GDPR | sistema (al recibir AC-007) | request confirmada | cascada: KMS keys disabled → Firestore database delete → Storage delete → Vector delete → KMS keys destroyed (después de 30 días gracia) → audit log irrevocable |
| IN-008 | Audit log de accesos a datos sensibles | sistema | acceso a campo cifrado o documento | log en Cloud Audit Logs con quién, qué, cuándo, IP |
| IN-009 | Exportación GDPR completa de instancia | sistema (al recibir AC-008) | request confirmada | ZIP firmado generado en Storage temp, link de descarga 24h |

### 2.10 ÉPICA: IMPACT — Capa de impacto (nuevo en RC3 §12)

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| IM-001 | Definir reto específico del equipo | líder | equipo existe | `Challenge` activo con `defined_at`, `description`; el anterior queda con `retired_at` |
| IM-002 | Listar histórico de retos del equipo | líder | equipo existe | timeline ordenado de Challenges |
| IM-003 | Definir outcome del equipo (3-5 por equipo) | líder | Challenge activo en el equipo | `Outcome` con título, descripción, status `proposed` → evaluado por GR-001 (guardarraíl 1) |
| IM-004 | Definir señales tempranas de un outcome (1-2 por outcome) | líder | Outcome existe | `EarlySignal` con `description` + `observable_metric`; vinculada al Outcome |
| IM-005 | Registrar lectura periódica de una señal temprana | líder o sistema | EarlySignal existe | nuevo `EarlySignalReading` en `history[]`; tendencia recomputada |
| IM-006 | Definir hipótesis de contribución individual (1-3 por persona) | líder | Persona + Outcome existen | `Contribution` con `action`, `outcome_id`, `hypothesis_reason`, `dimension_link`, `status=active` → evaluada por GR-002 |
| IM-007 | Registrar evidencia de impacto | líder | Contribution existe | `Evidence` con observación concreta + fecha + contribuciones referenciadas → evaluada por GR-003 |
| IM-008 | Revisar hipótesis en O2O | líder | Contribution con ≥1 Evidence reciente | `Contribution.last_reviewed_at` actualizado; status puede pasar a `confirmed`/`refuted`/`consolidated` |
| IM-009 | Diseñar experimento de equipo acotado | líder | intervención incierta sobre un Outcome o Contribution | `Experiment` con `what_is_tested`, `observed_signal_id`, `duration_days`, `decision_criteria`, status `running` |
| IM-010 | Cerrar experimento con resultado | líder | Experimento en estado running pasado `duration_days` | `Experiment.status = decided`, `result`, decisión registrada (consolidar/revisar) |
| IM-011 | Ver dashboard de impacto del equipo | líder | equipo con Outcomes activos | render: outcomes ↔ señales tempranas ↔ contribuciones ↔ personas ↔ dimensiones GRETA |
| IM-012 | Ver matriz valor × riesgo de intervenciones | líder | hay ≥1 Gap detectado y ≥1 intervención candidata | matriz 2×2 con intervenciones agrupadas; click → detalle de gap dimensional asociado |
| IM-013 | Priorizar intervención | líder | matriz visible | `Intervention.priority_decision` registrada con justificación textual |

### 2.11 ÉPICA: GUARD — Guardarraíles de IA (nuevo en RC3 §13)

> Regla maestra: **los guardarraíles AVISAN, nunca BLOQUEAN**. Toda advertencia se persiste como `GuardrailWarning` y queda visible en el historial, incluso si el líder decide ignorarla.

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| GU-001 | Evaluar outcome contra los 3 criterios (cambio real / dentro de control / observable) | sistema (onSave de IM-003) | Outcome guardado | si desviación: `GuardrailWarning(guardrail_id=1, entity=Outcome)` con `suggested_alternative` |
| GU-002 | Evaluar contribución individual con los mismos 3 criterios | sistema (onSave de IM-006) | Contribution guardada | si desviación: `GuardrailWarning(guardrail_id=2, entity=Contribution)` |
| GU-003 | Evaluar evidencia (observación concreta vs valoración subjetiva) | sistema (onSave de IM-007) | Evidence guardada | si valoración: `GuardrailWarning(guardrail_id=3, entity=Evidence)` con pregunta socrática |
| GU-004 | Detectar desvíos del framework en transcripción | sistema (durante AI-003) | pipeline RAG en curso | si hubo "evaluación en lugar de alineamiento" o "se habló de niveles": `GuardrailWarning(guardrail_id=4, entity=Document)` |
| GU-005 | Advertir nivel asignado sin base observacional | sistema (al guardar AS-001 o AI-005/006) | nuevo nivel persistido | si muestras < umbral: `GuardrailWarning(guardrail_id=5, entity=Assessment)` recomendando más observaciones |
| GU-006 | Revisar vista compartida antes de publicar | sistema (preview de SH-007) | preview generado | si revela niveles individuales comparativos: `GuardrailWarning(guardrail_id=6, entity=ShareableView)` |
| GU-007 | Contextualizar bajadas de nivel temporal | sistema (al renderizar VZ-002) | persona con ≥2 assessments | si nivel bajó: narrativa contextualizadora visible junto a la timeline |
| GU-008 | Detectar sesgos de atención (cron diario por instancia) | sistema | hay personas con `last_activity_at` > 60 días | `GuardrailWarning(guardrail_id=8, entity=Person)` por persona afectada + dashboard de "personas sin seguimiento" |
| GU-009 | Redirigir comparativas inter-personas en interpretación de mapa | sistema (detectores UI: filtro/sort por nivel entre personas) | patrón comparativo detectado | banner contextual + `GuardrailWarning(guardrail_id=9, entity=Instance)` |
| GU-010 | Listar todas las advertencias registradas | líder | sesión | listado filtrable por guardarraíl, entidad, severidad, estado (acknowledged/ignored) |
| GU-011 | Marcar advertencia como `acknowledged` | líder | warning existe | `GuardrailWarning.acknowledged_by = uid`, ignored = false |
| GU-012 | Ignorar advertencia con motivo | líder | warning existe | `GuardrailWarning.ignored = true`, `ignored_reason` registrado |

---

## 3. Flujos principales (numerados)

### F-001 — Registro de líder y creación automática de instancia

1. Líder visita `app.greta.io/signup`.
2. Introduce email + password (o auth federado: Google/Microsoft).
3. Frontend llama Cloud Function `signupLeader(email, password, displayName)`.
4. Cloud Function ejecuta en transacción:
   - Crea cuenta GCIP (Firebase Auth).
   - Crea tenant GCIP `tenant_<uid>`.
   - Crea Firestore database `instance-<uid>` con CMEK key recién creada.
   - Crea Cloud Storage bucket `gs://greta-instance-<uid>` con CMEK.
   - Crea namespace en Vertex Vector Search `<uid>`.
   - Crea KMS keyring + key `instance-<uid>-master`.
   - Crea documento `Instance/<uid>` con plan = trial, fecha_inicio, etc.
5. Si cualquier paso falla → rollback de los anteriores; usuario informado.
6. Si todo OK (<5s objetivo p95) → email de bienvenida + redirect a onboarding wizard.
7. Onboarding wizard: idioma, timezone, política de retención (defaults preseleccionados), tema básico.

### F-002 — Alta de equipo y personas

1. Líder logueado entra en "Equipos" → click "Nuevo equipo".
2. Modal con nombre, área, descripción → guarda → `Team` creado.
3. Desde el equipo, "Añadir personas" → opciones:
   - **Añadir manualmente**: form con nombre, email opcional, rol_actual, fecha_alta_equipo. → `Person` + `TeamMembership` creados.
   - **Importar CSV**: descarga template CSV → líder lo rellena → sube → preview con validación → confirma → batch insert atómico.
4. Tras crear personas, líder puede:
   - Subir CV (flujo F-003)
   - Crear assessment manual (flujo F-005)
   - Subir transcripción (flujo F-004)

### F-003 — Subida y procesamiento de CV con IA → sugerencias → validación

1. Líder en ficha de persona → "Documentos" → "Subir CV".
2. Drag&drop PDF/DOCX <10MB. Validación cliente.
3. Frontend cifra archivo (envelope encryption con DEK derivado de la KMS key de la instancia) → sube a Storage.
4. Cloud Function `processCV(documentId)` triggerizada por evento Storage:
   - Descifra archivo en memoria de la function.
   - Extrae texto: pdf-parse o mammoth.
   - Llama Claude Haiku 4.5 con system prompt de extracción + JSON Schema.
   - Output: JSON estructurado con proyectos, áreas técnicas (con nivel inferido), situaciones gestionadas, idiomas, formación.
   - Crea `DocumentExtraction` + `AiSuggestion` con `status=pending`.
   - Genera embeddings de los párrafos relevantes → almacena en Vector Search con metadata `instance_id`, `person_id`, `document_id`, `chunk_type`.
   - Sugiere preguntas para la próxima conversación con la persona (basadas en gaps detectados).
5. Líder recibe notificación en UI ("CV procesado — 8 sugerencias para revisar").
6. Líder abre el detalle:
   - Por cada campo sugerido (proyectos, áreas, etc.), ve la cita textual del CV que respalda la sugerencia.
   - Para cada sugerencia: **Aceptar / Editar / Rechazar** + motivo opcional.
7. Las aceptadas/editadas actualizan `Person`. Las rechazadas se mantienen en histórico (90 días por defecto).

### F-004 — Subida de transcripción O2O → sugerencias con razonamiento → validación

1. Líder en ficha de persona → "Conversaciones" → "Nueva conversación".
2. Opciones:
   - Pegar transcripción en textarea
   - Subir archivo .txt/.md/.docx
   - Pegar resumen escrito por el líder (más corto, más anotado)
3. Frontend cifra y sube como en F-003 paso 3.
4. Cloud Function `processConversation(documentId)`:
   - Descifra.
   - Chunking semántico (~800 tokens, overlap 100).
   - Embeddings → almacenados con metadata.
   - Construye contexto RAG:
     - GRETA framework (estático, ~3K tokens)
     - Top-k chunks del histórico de la persona (k=5–10) recuperados de Vector Search filtrado por `person_id`
     - Transcripción nueva
   - Llama Claude Sonnet 4.6 vía Vertex AI (region `europe-west1`) con system prompt que pide:
     - Por cada dimensión GRETA, sugerir nivel (1-7) con color
     - Razonamiento estructurado
     - 2-5 citas literales del texto que respaldan la sugerencia
     - Preguntas sugeridas para la próxima conversación
     - Confidence score por sugerencia
   - Output: JSON validado con Valibot/JSON Schema antes de persistir.
   - Crea `AiSuggestion` por dimensión, status=pending.
5. Líder recibe notificación. Procesamiento típico <30s p95 para 5K palabras.
6. UI presenta:
   - Las 4 dimensiones con la sugerencia
   - Para cada una: nivel sugerido + color + razonamiento + citas resaltadas en el texto original
   - Botones Aceptar / Ajustar (slider de niveles) / Rechazar
7. Cada acción crea `AssessmentValidation` y/o actualiza `Assessment` de la persona.
8. La transcripción y sus chunks quedan disponibles para futuros RAGs (con cifrado in-place).

### F-005 — Crear assessment manual sin IA

1. Líder en ficha de persona → "Nuevo assessment manual".
2. UI presenta las 4 dimensiones con sliders/selectores de los 7 niveles (con sus colores).
3. Para cada dimensión: nivel + texto libre de justificación + tags de comportamiento (autocompletado de tags previos).
4. Guardar → `Assessment` creado con `source=manual`.
5. Visualizaciones (radar, evolución) se actualizan en tiempo real (onSnapshot).

### F-006 — Generar y compartir vista read-only

1. Líder en "Compartir" → "Nueva vista compartida".
2. Wizard:
   - Seleccionar contenido: equipos, personas (toda o parcial), dimensiones (todas o parciales), visualizaciones (radar, dashboard, grafo).
   - Decidir si se incluyen razonamientos textuales (default: NO; sólo agregados/visualizaciones).
   - Configurar expiry: días (default 30) o "permanente hasta revocación".
3. Líder hace click en "Vista previa" → renderiza exactamente lo que verá el observador.
4. Líder confirma → Cloud Function `generateShareableView(config)`:
   - Crea documento `ShareableView` en Firestore.
   - Genera JWT firmado con clave del backend, claims: `view_id`, `instance_id`, `expiry`, `nonce`.
   - Construye URL: `https://app.greta.io/v/<jwt>`.
5. UI presenta la URL + botón copiar + QR.
6. (Beta) Opción de magic-link: en lugar de URL pública, líder introduce emails de observadores; cada uno recibe un magic-link único; auth previo a ver la vista.

### F-007 — Borrado GDPR completo de la instancia

1. Líder en "Cuenta" → "Borrar mi cuenta y todos mis datos".
2. UI presenta consecuencias + checkbox "He leído", + 2-step confirmation (re-login).
3. Líder confirma → Cloud Function `requestAccountDeletion()`:
   - `Instance.status = pending_deletion`, `deletion_requested_at = now`.
   - Manda email confirmando + link para revertir en las próximas 24h.
4. Cron diario: instancias con `pending_deletion > 24h sin reverse`:
   - Disable KMS keys (datos ya no son descifrables) → en este momento, los datos están "muertos" criptográficamente.
   - Borra Firestore database completa.
   - Borra Storage bucket completo.
   - Borra namespace Vertex Vector Search filtrado por `instance_id`.
   - Borra GCIP tenant.
   - Tras 30 días → KMS keys `destroy` (irrecuperable).
   - Audit log irrevocable: "instance <uid> deleted at <timestamp>, reason: GDPR right to be forgotten".
5. Email final al líder confirmando borrado completo.

### F-008 — Consulta del grafo de relaciones

1. Líder entra en "Vista de grafo".
2. Cloud Function `loadGraph(instanceId)`:
   - Query Firestore: people, teams, projects, memberships, contributions.
   - Construye estructura nodes/edges en memoria.
   - Devuelve JSON serializado.
3. Frontend renderiza con `<graph-view>` (Cytoscape internamente).
4. Líder filtra por dimensión → frontend pide `assessments` agregados por persona y colorea nodos.
5. Click en nodo persona → navega a ficha (`/people/:id`).

### F-009 — Definir reto específico → outcome del equipo → evaluación por guardarraíl 1 (RC3)

1. Líder en dashboard de equipo → "Definir reto del equipo".
2. Si ya existe Challenge activo: aviso de que se retirará el actual al crear uno nuevo.
3. Líder describe el reto concreto del momento (textarea, ~200-500 caracteres).
4. Guarda → `Challenge` creado con `defined_at = now`; el anterior queda con `retired_at`.
5. Sección "Outcomes del equipo (3-5)" se desbloquea. Líder añade un outcome.
6. Para cada outcome: título, descripción, vinculación implícita al Challenge activo.
7. Al guardar (onSave **asincrónico**), Cloud Function `evalGuardrailOutcome(outcomeId)`:
   - Llama Claude Haiku 4.5 con system prompt que evalúa los 3 criterios RC3: ¿cambio real?, ¿dentro de control?, ¿observable?
   - Output JSON: `{passes_all: bool, deviations: [...], suggested_alternatives: [...]}`
   - Si `passes_all = false`: crea `GuardrailWarning(guardrail_id=1, entity=Outcome, severity, suggested_alternative)`. **No bloquea el guardado** (RC3 §13).
   - Si `passes_all = true`: no se crea warning.
8. UI muestra toast/badge en el outcome: "✅ outcome válido" o "⚠ 2 puntos a revisar — ver sugerencia".
9. Líder puede: aceptar la sugerencia (reemplaza el texto), ignorar (queda warning visible), editar manualmente.
10. Una vez el outcome está definido, líder añade 1-2 `EarlySignal`. Sin guardarraíl LLM — solo validación determinista de formato.

### F-010 — Definir hipótesis de contribución individual → evaluación por guardarraíl 2 (RC3)

1. Líder en ficha de persona → "Contribuciones esperadas".
2. UI propone el formato literal RC3: *"Creemos que si [persona] [acción], contribuirá a [outcome], porque [razón GRETA]"*.
3. Líder elige outcome objetivo (de los activos del equipo) → describe acción → describe razón conectada a una de las 4 dimensiones GRETA (selector `dimension_link`).
4. Guarda → `Contribution(status=active, last_reviewed_at=now)`.
5. Cloud Function `evalGuardrailContribution(contributionId)` evalúa los mismos 3 criterios que GU-001 aplicados a la hipótesis.
6. Si desviación: `GuardrailWarning(guardrail_id=2)` + sugerencia.
7. En la próxima O2O, líder revisa la hipótesis con la persona. Puede:
   - Marcarla como `confirmed` (la evidencia la respalda)
   - Marcarla como `refuted` (no se cumplió; ¿la hipótesis era mala o las condiciones del equipo bloquean?)
   - Mantenerla `active` con nuevo `last_reviewed_at`
8. Si `refuted`, GRETA ayuda a diagnosticar el gap dimensional asociado (sección §4.3 nueva del framework).

### F-011 — Registrar evidencia de impacto → evaluación por guardarraíl 3 (RC3)

1. Líder en ficha de persona → "Evidencias de impacto" → "Registrar evidencia".
2. Formulario:
   - Texto libre de la observación (max 1000 caracteres, foco "concreto, observable, fechado").
   - Fecha de la observación.
   - Contribuciones a las que se vincula (multi-select de Contributions activas de la persona).
   - Outcome(s) afectado(s) (derivado de las contribuciones).
3. Guarda → `Evidence` creada.
4. Cloud Function `evalGuardrailEvidence(evidenceId)` (Haiku):
   - Pregunta: ¿es observación concreta o valoración subjetiva?
   - Si valoración: `GuardrailWarning(guardrail_id=3)` con pregunta socrática ("¿puedes describir una situación concreta reciente donde lo hayas visto?").
5. La evidencia queda persistida. El líder puede editarla para concretarla.
6. La evidencia es input para revisión de hipótesis (F-010 paso 7) y para dashboard de impacto (IM-011).

### F-012 — Diseñar y cerrar experimento de equipo acotado (RC3)

1. Líder en dashboard de impacto → "Nuevo experimento".
2. Wizard:
   - **Qué se prueba**: descripción de la intervención acotada.
   - **Qué se observa**: selección de una `EarlySignal` existente (o crear nueva).
   - **Cuánto tiempo**: `duration_days` (default sugerido: 30-90).
   - **Qué se decide**: criterio explícito ("si la señal se mueve a X, consolidar; si no, revisar la hipótesis").
   - Vinculación opcional a una `Contribution` específica si el experimento valida esa hipótesis.
3. Guarda → `Experiment(status=running, started_at=now)`.
4. Sistema agenda recordatorio en `started_at + duration_days` para cerrar.
5. Durante el experimento, líder registra evidencias y lecturas de la señal temprana normalmente.
6. Al vencer la duración: notificación al líder → "Experimento X listo para decisión".
7. Líder cierra: `result` textual + decisión (consolidar / revisar / extender). `Experiment.status = decided`.
8. Si la `Contribution` vinculada se confirma → cambia su `status = confirmed`.

### F-013 — Alerta de sesgo de atención (guardarraíl 8, RC3)

1. Cron diario (Cloud Scheduler) por instancia activa.
2. Cloud Function `detectAttentionBias(instanceId)`:
   - Query Firestore: personas con `last_activity_at > 60 días` (sin O2O, sin Evidence, sin assessment actualizado).
   - Por cada persona afectada: crea o actualiza `GuardrailWarning(guardrail_id=8, entity=Person, severity=info)` con sugerencia de acción (ej. "agendar O2O esta semana").
3. Dashboard del líder muestra widget "Personas sin seguimiento reciente" con N personas.
4. Líder click en una → ficha de persona + botón "Agendar conversación" (deep link a calendario).
5. Al registrar nueva actividad sobre esa persona, la warning se marca `acknowledged_by=uid` y desaparece del widget.

### F-014 — Priorizar intervenciones por matriz valor × riesgo (RC3)

1. Sistema detecta `Gap`s a partir del diagnóstico (bus factor, cobertura roles, dimensión emocional, etc.) y propone `Intervention`s candidatas.
2. Cada Intervention tiene `value_score: low|high` (¿cuánto limita el outcome prioritario?) y `risk_score: low|high` (¿cuán disruptiva?).
3. Líder en dashboard de impacto → tab "Priorización" → matriz 2×2 renderizada.
4. Líder puede ajustar manualmente `value_score`/`risk_score` con justificación.
5. Líder selecciona intervención y la marca como `priority_decision=act_first` / `act_with_experiment` / `monitor` / `defer` (los 4 cuadrantes del framework RC3).
6. `act_with_experiment` deep-linka a F-012 (diseñar experimento acotado).

---

## 4. Modelo de datos conceptual

### 4.1 Entidades principales

Todas viven dentro del scope `instance_<uid>` (Firestore database por instancia).

| Entidad | Atributos clave | Sensibilidad | Cifrado |
|---|---|---|---|
| **Instance** | id, owner_uid, plan, theme, retention_policy, kms_key_ref, created_at | Normal | CMEK database |
| **LeaderProfile** | uid, displayName, email, locale, timezone | Normal | CMEK database |
| **Team** | id, name, area, status, created_at, archived_at | Normal | CMEK |
| **Person** | id, name, email_opt, current_role, status, custom_fields_ref | Normal (datos básicos) | CMEK |
| **TeamMembership** | id, person_id, team_id, role_in_team, start_date, end_date | Normal | CMEK |
| **Project** | id, name, client, start_date, end_date, status | Normal | CMEK |
| **ProjectContribution** | id, person_id, project_id, role, period, description | Normal | CMEK |
| **Assessment** | id, person_id, dimension, level (1-7), color, source (manual\|ai), justification, tags, ai_suggestion_id?, created_at, version | **Sensible** | CMEK + envelope para `justification` si toca dimensión emocional |
| **Document** | id, person_id, type (cv\|transcript\|summary\|other), filename, storage_ref, mime, size, uploaded_at | **Muy sensible** (transcripts) / Normal (CV) | CMEK + envelope para transcripts |
| **DocumentExtraction** | id, document_id, structured_data (JSON), extracted_at, model_used | Sensible (depende del documento) | hereda del Document |
| **AiSuggestion** | id, document_id?, dimension, suggested_level, suggested_color, reasoning, citations[], confidence, status (pending\|accepted\|edited\|rejected), created_at, model_used, model_version | **Sensible** (especialmente reasoning de dim emocional) | envelope para reasoning |
| **AssessmentValidation** | id, ai_suggestion_id, leader_uid, action (accept\|edit\|reject), final_level, final_reasoning, validated_at | Sensible | envelope para final_reasoning |
| **CustomField** | id, name, type, options?, max_length, sensitivity (normal\|sensitive\|very_sensitive) | Normal (definición) | — |
| **CustomFieldValue** | id, person_id, custom_field_id, value, encrypted | depende del CustomField.sensitivity | envelope si sensitivity ≥ sensible |
| **ShareableView** | id, name, content_config (JSON), include_reasoning (bool), expiry_at, jwt_jti, status (active\|revoked), created_at, accessed_count | Sensible (acceso a datos) | CMEK |
| **ConsentRecord** | id, person_id, consent_type, granted_at, support (text\|file_storage_ref), revoked_at? | **Muy sensible** | envelope |
| **AuditEvent** | id, actor (leader_uid\|operator_id), action, resource_type, resource_id, ip, user_agent, timestamp | Normal | append-only, sin cifrado adicional |
| **Challenge** (RC3) | id, team_id, description, defined_at, retired_at? | Normal | CMEK |
| **Outcome** (RC3) | id, team_id, challenge_id (FK), title, description, status (proposed\|active\|achieved\|abandoned), created_at | Normal | CMEK |
| **EarlySignal** (RC3) | id, outcome_id, description, observable_metric, history[] (lecturas con fecha y valor) | Normal | CMEK |
| **Contribution** (RC3, refactor) | id, person_id, team_id, outcome_id, action, hypothesis_reason, dimension_link (1..4), status (active\|confirmed\|refuted\|consolidated), defined_at, last_reviewed_at | Sensible (razón puede tocar dim emocional) | envelope para `hypothesis_reason` si `dimension_link=4` |
| **Evidence** (RC3) | id, person_id, contribution_ids[], outcome_ids[], observation_text, observed_at, registered_by, registered_at | Sensible | envelope para `observation_text` |
| **Experiment** (RC3) | id, team_id, what_is_tested, observed_signal_id (FK→EarlySignal), duration_days, decision_criteria, started_at, ended_at?, outcome_id (FK), contribution_id? (FK), status (running\|decided\|abandoned), result? | Normal | CMEK |
| **Gap** (RC3) | id, team_id, dimension (1..4), description, severity, detected_at, source (auto\|manual) | Normal | CMEK |
| **Intervention** (RC3) | id, gap_id, palanca_tipo (contratación\|redistribución\|formación\|acompañamiento\|cambio_rol\|baja), value_score (low\|high), risk_score (low\|high), priority_decision (act_first\|act_with_experiment\|monitor\|defer), notes | Normal | CMEK |
| **GuardrailWarning** (RC3) | id, guardrail_id (1..9), entity_type (Outcome\|Contribution\|Evidence\|Document\|Assessment\|ShareableView\|Person\|Instance), entity_id, severity (info\|warn\|critical), message, suggested_alternative?, created_at, acknowledged_by?, ignored (bool), ignored_reason? | Sensible (mensaje puede citar contenido sensible) | envelope si entity_type ∈ {Document, Assessment} |

### 4.2 Entidades del grafo (cuando se materialice — MVP las infiere de Firestore)

#### Nodos

| Nodo | Propiedades clave |
|---|---|
| `:Person` | id, name, current_role, profile_shape (I/T/π/Comb) |
| `:Team` | id, name, area |
| `:Project` | id, name, client, period |
| `:Assessment` | id, dimension, level, color, date |
| `:Dimension` | id (1..4), name |

#### Aristas

| Arista | Dirección | Propiedades |
|---|---|---|
| `:MEMBER_OF` | Person → Team | role_in_team, start_date, end_date, is_current |
| `:CONTRIBUTED_TO` | Person → Project | role, period, contribution_summary |
| `:HAS_ASSESSMENT` | Person → Assessment | (ninguna) |
| `:IN_DIMENSION` | Assessment → Dimension | (ninguna) |
| `:LINKED_TO` | Team → Project | (period si aplica) |
| `:WORKED_WITH` | Person → Person | derivada (proyectos en común), score (#proyectos compartidos) |

#### Consultas típicas

1. *"Personas del equipo X con nivel Verde+ en dimensión Conocimiento"* → Firestore directo (1 query con filtro compuesto).
2. *"Cobertura de roles Belbin en equipo X"* → agregación Firestore con group-by sobre Person.role_belbin filtrado por TeamMembership.
3. *"Bus factor por área en equipo X"* → conteo de Person con dominio ≥ "sólido" en cada área.
4. *"Personas que han trabajado con persona A"* → derivado de ProjectContributions compartidas (1 join).
5. *"Score de salud del equipo X"* → fórmula compuesta (cobertura roles + bus factor + diversidad de niveles + niveles dimensión emocional). Calculada server-side, cacheada.

**Decisión técnica diferida a Architecture**: para el MVP, queries 1-4 se resuelven con Firestore + junction collections + algunos índices compuestos. Si la query 4 (paths arbitrarios entre personas) o un futuro "shortest path multi-hop" se vuelven dominantes, migrar a Neo4j Aura.

---

## 5. Reglas de negocio críticas

### 5.1 Aislamiento entre instancias (no negociable)

- **Toda lectura/escritura** de Firestore lleva `instance_id` en el path o es rechazada por security rules.
- **Toda Cloud Function** verifica `request.auth.uid == instance.owner_uid` antes de cualquier operación, salvo signup y endpoints públicos firmados (vistas compartidas).
- **Vector Search**: cada query incluye filtro obligatorio `instance_id == request.user.instance_id`. Sin filtro → la function rechaza.
- **KMS**: cada instancia tiene su propia key. IAM impide que la SA de una instancia descifre con la key de otra.
- **Tests automatizados** de aislamiento en CI: simular acceso cross-tenant en cada deploy y fallar si pasa.

### 5.2 Rol del líder y rol de la IA

- El líder es el único actor con permisos de escritura sobre los datos de la instancia.
- La IA **propone**, NUNCA actualiza directamente el assessment de una persona. Siempre pasa por `AssessmentValidation` del líder.
- Toda sugerencia se persiste con su input completo + razonamiento + citas, incluso si se rechaza (auditoría + mejora del modelo).

### 5.3 Compartición de vistas

- Sólo el líder puede crear/modificar/revocar vistas compartidas.
- El observador NUNCA accede a datos sensibles textuales (transcripciones, razonamientos detallados de IA sobre dimensión emocional). Sólo a agregados/visualizaciones que el líder marcó.
- Preview obligatorio antes de generar el link (regla de UI defensiva).
- Revocación instantánea efectiva: el JWT entra en una blacklist server-side comprobada en cada request.
- Expiry máximo configurable a nivel de instancia (default global: 90 días).

### 5.4 Validación de assessments

- Quien crea un assessment manual es el líder, registrando su uid.
- Las versiones anteriores no se sobreescriben — `Assessment.version` se incrementa.
- El historial completo es inmutable salvo por borrado GDPR.

### 5.5 Retención y borrado (GDPR)

- Política configurable por instancia, defaults:
  - Documentos sensibles (transcripciones): 18 meses
  - Assessments: 5 años
  - Sugerencias rechazadas: 90 días
  - Sugerencias aceptadas: vinculadas al Assessment, con su retention
  - Audit logs: 7 años (compliance)
- **Derecho al olvido a nivel persona**: borrado en cascada de Person + sus Documents, Extractions, Suggestions, Validations, Assessments. ConsentRecord se conserva 1 año (evidencia de cumplimiento) anonimizado.
- **Derecho al olvido a nivel instancia**: F-007.
- **Derecho de acceso**: el líder puede generar export en cualquier momento (ZIP firmado).

### 5.6 Cifrado por tipo de dato

| Tipo | Cifrado |
|---|---|
| Datos básicos (nombre, equipo, proyecto) | CMEK database |
| Assessments (nivel, color, justificación de dim no-emocional) | CMEK database |
| Transcripciones, razonamientos de dim emocional, ConsentRecord | CMEK database **+** envelope encryption a nivel campo |
| Audit logs | append-only, sin cifrado adicional (no contienen PII directa) |

### 5.7 Campos personalizados

- Hasta **20 campos personalizados** por instancia.
- Tipos: texto corto (max 200), texto largo (max 5000), número, fecha, selección única, selección múltiple, archivo (max 5MB).
- El líder define la sensibilidad: normal / sensible / muy sensible. Esto determina el cifrado aplicado.
- Los campos pueden marcarse como "procesable por IA" → se incluyen en el contexto RAG.

### 5.8 Auditoría

- Cada acceso a un campo cifrado por envelope encryption queda en `AuditEvent` con: actor (uid), action (`decrypt`/`read`), resource (Document.id, Suggestion.id), timestamp, IP, user-agent.
- Cada modificación de un Assessment queda en `AuditEvent` con before/after.
- El acceso del operador del SaaS a datos de instancia (escenario excepcional) genera `AuditEvent` + notificación email automática al líder.

### 5.9 Provisioning

- Atómico: signup completo o rollback total.
- Objetivo: <5s p95.
- Idempotente: si el usuario reintenta tras un rollback, no quedan recursos zombies.

### 5.10 Vendor lock-in y exportación

- El líder puede solicitar export completo en formato JSON estándar + archivos cifrados desempaquetados (con su clave).
- Los componentes del catálogo `@manufosela/*` son OSS-friendly — no hay lock-in técnico al SaaS.

### 5.11 Guardarraíles de IA — la IA AVISA, NUNCA BLOQUEA (RC3 §13)

- Ningún guardarraíl impide guardar. Toda escritura de Outcome, Contribution, Evidence, ShareableView, etc. persiste el documento aunque el guardarraíl detecte desvío.
- Toda detección crea `GuardrailWarning` asociada a la entidad. El líder decide: aceptar la sugerencia, ignorar (con motivo opcional) o editar manualmente.
- Las advertencias `ignored` quedan en histórico — no se borran. Esto permite revisar a posteriori si el patrón se repite.
- El tono del mensaje generado por la IA es siempre el de un colega que pregunta honestamente (system prompt explícito): "¿lo que has guardado describe lo que cambia o lo que se hace?" en lugar de "Esto es incorrecto, corrígelo".
- Latencia objetivo: GuardrailWarning visible <3s tras el save (asincrónico). Si la evaluación falla (timeout, error LLM), el documento queda guardado sin warning y el sistema lo reintenta en cron diferido.

### 5.12 Ciclo de vida de hipótesis y experimentos (RC3 §12)

- Una `Contribution` siempre nace con `status=active` y `last_reviewed_at = defined_at`.
- En cada O2O donde se revisa, el líder actualiza `last_reviewed_at`. Una hipótesis sin revisar en >90 días aparece como warning UI (no genera GuardrailWarning persistida — es responsabilidad operativa del líder).
- Transición a `confirmed`/`refuted`/`consolidated`: solo el líder. La IA puede sugerir pero nunca cambiar el status.
- Un `Experiment` con `status=running` que excede su `duration_days + 30` se marca `status=abandoned` automáticamente. El líder puede reabrirlo o cerrarlo con resultado.

### 5.13 Capa de impacto como herramienta de alineamiento, NO de evaluación (RC3 §12)

- La UI de los outcomes, contribuciones y evidencias NUNCA muestra ranking entre personas, ni puntuación numérica agregada por persona ("Marta = 7 evidencias / Luis = 3"). El framework lo prohíbe explícitamente.
- El dashboard de impacto del equipo muestra: outcome → señales tempranas → contribuciones agrupadas por outcome (no por persona). El detalle por persona solo accesible desde la ficha individual.
- Si el líder intenta exportar/compartir una vista que ranquea personas por evidencias, el guardarraíl 6 dispara una advertencia obligatoria de revisión (no bloqueante, pero registrada).

---

## 6. Requisitos no funcionales

| Categoría | Requisito |
|---|---|
| **Latencia objetivo** (p95, network EU) | Dashboard inicial <2s; radar chart <1s; grafo (50 nodos) <2s; procesamiento conversación IA (5K palabras) <30s; CV parsing <15s |
| **Disponibilidad** | MVP: 99.5% uptime; Beta: 99.9% (excluyendo ventanas de mantenimiento anunciadas) |
| **Throughput pico** | 10 conversaciones IA simultáneas por instancia (cola con backoff si más); 100 lecturas Firestore/s/instancia (límite cliente) |
| **Volumen por instancia** | Pequeña: 5–25 personas, ~50 docs, <50MB; Mediana: 25–100, ~200 docs, ~200MB; Grande: 100–500, ~1000 docs, ~1GB |
| **Auditoría** | Todos los accesos a datos cifrados con envelope encryption en `AuditEvent`; retention 7 años |
| **Backup y recuperación** | Snapshot diario por instancia; retention 30 días; RPO ≤24h; RTO ≤4h para restore granular |
| **Accesibilidad** | WCAG 2.1 AA mínimo. Componentes del catálogo `@manufosela/*` deben cumplir esto antes de aceptarse en GRETA. Contraste ≥4.5:1 en texto, navegación por teclado completa, aria-* en componentes interactivos |
| **i18n** | MVP: español. Beta: añadir inglés. Componentes del catálogo deben ser i18n-ready (slots/atributos para textos, no strings hardcoded) |
| **Compatibilidad navegadores** | Últimas 2 versiones de Chrome, Firefox, Safari, Edge. Sin soporte IE/legacy |
| **Compatibilidad móvil (PWA)** | iOS Safari 17+, Android Chrome 110+ |
| **Coste por instancia (target)** | Early adopter: <$15/mes; Crecimiento: <$70/mes; Escala: <$330/mes (ver Research §7) |
| **Tiempo de carga inicial PWA** | <3s en 3G simulada con shell precacheado |

---

## 7. Mapa componente GRETA ↔ catálogo `@manufosela/*` (inventario real)

> Inventario realizado el 2026-05-10 sobre `npm search @manufosela`. 18 paquetes publicados.

### 7.1 Componentes que YA existen en el catálogo (importar y usar)

| Componente GRETA | Paquete npm | Versión | Uso en GRETA |
|---|---|---|---|
| Radar 4 dimensiones × 7 niveles | `@manufosela/radar-chart` | 2.0.0 | El radar de cada persona. Verificar que admite 7 niveles concéntricos coloreados; si no, ampliar en el catálogo. |
| Evolución temporal | `@manufosela/historical-line` | 2.0.0 | Timeline de assessments por persona. |
| Modales | `@manufosela/app-modal` | 2.3.0 | Confirmaciones, edición rápida, wizards. |
| Notificaciones toast | `@manufosela/slide-notification` | 2.1.0 | Feedback de acciones (guardado, error, éxito). |
| Selector múltiple | `@manufosela/multi-select` | 2.1.0 | Filtros (dimensiones, equipos), tags. |
| Navegación principal | `@manufosela/nav-list` | 3.0.0 | Tabs y menú lateral del dashboard. |
| Cards de elemento | `@manufosela/element-card` | 2.3.9 | Cards de equipos, proyectos, personas. |
| Indicador circular % | `@manufosela/circle-percent` | 3.0.1 | Bus factor por área, score salud equipo. |
| Foto perfil persona | `@manufosela/circle-picture` | 2.2.1 | Avatares en fichas y dashboard. |
| Vista mosaico fotos equipo | `@manufosela/photo-collage` | 1.1.0 | Vista visual del equipo completo. |
| Toggle tema light/dark | `@manufosela/theme-toggle` | 2.0.1 | Cambio de tema en header. |
| Captura imagen | `@manufosela/capture-image` | 1.1.0 | Foto perfil opcional al alta de persona. |
| Comparación antes/después | `@manufosela/before-after` | 1.0.7 | Comparar dos assessments de la misma persona en el tiempo. |
| Validación de formularios | `@manufosela/form-validators` + `automatic_form_validation` | 1.0.0 / 1.7.0 | Inputs, emails, NIF/NIE, etc. en signup, alta persona, perfil. |

### 7.2 Componentes a crear y publicar al catálogo (gaps)

> Estos son los nuevos componentes Lit que **se diseñarán y publicarán a `@manufosela/*` ANTES de usarlos en GRETA**. Son lo suficientemente genéricos para reutilizar en otros proyectos.

| Componente nuevo | Paquete propuesto | Para qué |
|---|---|---|
| Vista de grafo interactiva | `@manufosela/graph-view` | Wrapper Cytoscape para personas/equipos/proyectos con filtros + colores por nivel |
| Mapa de calor | `@manufosela/heatmap-chart` | Matriz personas × áreas técnicas con escalado de colores |
| Selector de niveles con colores | `@manufosela/level-selector` | Genérico: lista ordenada de niveles con etiquetas y colores configurables |
| Drag&drop de archivos | `@manufosela/file-uploader` | PDF/DOCX/imágenes con preview, validación tamaño/tipo |
| Tabla de datos | `@manufosela/data-table` | Listado con sorting, paginación, filtros, selección |
| Visor de texto enriquecido con resaltados | `@manufosela/rich-text-viewer` | Mostrar razonamiento IA con citas highlightadas en el texto fuente |
| Chips / badges | `@manufosela/chip` | Tags de comportamiento, etiquetas de equipo, categorías |
| Captura de consentimiento | `@manufosela/consent-recorder` | Form con texto del consentimiento + checkbox + opcional firma/upload |
| Árbol jerárquico colapsable | `@manufosela/tree-view` | Render outcome → señales tempranas → contribuciones → evidencias (RC3 IM-011) |
| Matriz 2×2 valor × riesgo | `@manufosela/quadrant-matrix` | Cuadrantes configurables con tarjetas arrastrables (RC3 F-014) |
| Banner contextual ligero | `@manufosela/inline-banner` | Advertencias de guardarraíl no intrusivas (RC3 GU-001..009) con CTA "ver sugerencia" |

### 7.3 Composiciones específicas de GRETA (viven en el repo del SaaS, no en el catálogo)

> Estas son **composiciones de componentes del catálogo** específicas a la lógica del producto. NO se publican al catálogo (no serían reutilizables fuera de GRETA).

| Composición | Componentes del catálogo que usa |
|---|---|
| `<assessment-editor>` (editar las 4 dimensiones de una persona) | `level-selector` × 4 + `app-modal` + `chip` + `slide-notification` |
| `<ai-suggestion-card>` (sugerencia IA con reasoning + citas + botones acción) | `rich-text-viewer` + `app-modal` + botones nativos |
| `<share-view-builder>` (wizard creación vista compartida) | `multi-select` + `app-modal` + `slide-notification` |
| `<team-dashboard>` (cobertura roles + bus factor + score salud) | `element-card` × N + `circle-percent` × N + `radar-chart` |
| `<knowledge-heatmap>` (mapa de calor de conocimiento del equipo) | `heatmap-chart` + filtros con `multi-select` |
| `<outcome-editor>` (RC3 IM-003) — definir outcome con feedback inline del guardarraíl 1 | textarea + `inline-banner` + `slide-notification` |
| `<contribution-hypothesis-form>` (RC3 IM-006) — formulario hipótesis con plantilla literal y `dimension_link` | `app-modal` + selector dimensión + `inline-banner` |
| `<evidence-recorder>` (RC3 IM-007) — registro evidencia con guardarraíl 3 | textarea + `multi-select` (contribuciones) + `inline-banner` |
| `<experiment-designer>` (RC3 IM-009) — wizard 4 campos del experimento | `app-modal` con pasos + selector señal temprana |
| `<impact-dashboard>` (RC3 IM-011) — outcomes → señales → contribuciones → personas | `tree-view` + `circle-percent` (progreso por señal temprana) + `element-card` |
| `<value-risk-matrix>` (RC3 IM-012) — intervenciones priorizadas en cuadrantes | `quadrant-matrix` + drag&drop |
| `<guardrail-warning-card>` (RC3 GU-010..012) — listado de advertencias con acciones | `element-card` + `inline-banner` + acciones acknowledge/ignore |
| `<attention-bias-widget>` (RC3 GU-008) — personas sin seguimiento reciente | listado compacto + `element-card` + deep-link |

### 7.4 Convención

- Si el componente es **suficientemente genérico** para ser reutilizable fuera de GRETA → publicar a `@manufosela/*` PRIMERO, luego importar.
- Si es **composición específica de la lógica del producto** → vive en el repo de GRETA, pero **compuesta exclusivamente de componentes del catálogo**.
- Componentes básicos no reactivos (header, footer, layouts, landing) → Astro nativo `.astro`, sin forzar Lit.

---

## 8. Decisiones de producto que requieren validación antes de Architecture

> Estas decisiones afectan al diseño técnico. Deben confirmarse **antes** de cerrar Architecture.

| # | Decisión | Propuesta de Discovery | Por validar |
|---|---|---|---|
| 8.1 | ¿La política de consentimiento Art. 9 propuesta (responsabilidad del líder + plantillas + ConsentRecord opcional) es legal y operativamente aceptable? | Sí | Asesor legal / DPO antes de MVP |
| 8.2 | Al borrar una persona, ¿se conservan agregados anonimizados para que el equipo mantenga continuidad histórica? | NO en MVP — borrado total. Reconsiderar en Beta. | Producto |
| 8.3 | ¿El líder puede invitar a un "co-líder" a la misma organización (handover, liderazgo compartido)? | **SÍ desde el inicio (decisión del autor)** — disponible en plan Team o superior. Owner controla invitaciones, revocaciones y transferencia de ownership. | — (decidido) |
| 8.4 | ¿Los planes de pricing son finales? Limitan el modelo (max 25 personas en Solo). | Provisionales — confirmar antes de Architecture | Producto + business |
| 8.5 | ¿Hay flujo de trial gratuito antes del paid? | Sí: 14 días con todas las features de Team. Después downgrade automático a Solo si no upgrade. | Producto |
| 8.6 | ¿Las personas evaluadas pueden recibir su radar individual (informe personal) con permiso del líder? | NO en MVP. Considerar en Beta como feature de "feedback formal". | Producto |
| 8.7 | ¿El SaaS ofrece API pública para integración con HRIS (Workday, BambooHR)? | NO en MVP. Roadmap post-Beta. | Producto |
| 8.8 | ¿Multi-database por instancia tiene un límite práctico de Firebase (ej: 100 databases por proyecto)? | Por confirmar en Architecture; alternativa: múltiples proyectos GCP gestionados como pool. | Architecture |

---

## 9. Riesgos de producto identificados

| # | Riesgo | Probabilidad | Impacto | Mitigación |
|---|---|---|---|---|
| 9.1 | Fricción excesiva en validación de sugerencias IA | Alta | Alto | Tests de usabilidad con líderes reales en Sprint 0; defaults inteligentes (aceptar batch, atajos teclado) |
| 9.2 | Curva de aprendizaje del framework GRETA | Alta | Medio | Onboarding con tooltips contextuales, video explicativo (3-5 min), glossary in-app |
| 9.3 | Compartir vistas revela más de lo deseado por accidente | Media | Alto | Preview obligatorio antes de generar link, listado claro de "qué se comparte", auditoría de accesos |
| 9.4 | Líder usa el SaaS sin haber obtenido consentimiento de la persona | Media | Alto (legal) | T&C limitan responsabilidad del SaaS; UI muestra advertencias y plantillas; opcional registro `ConsentRecord` |
| 9.5 | Sugerencias IA con sesgo (cultural, de género, etc.) en dimensión emocional | Media | Alto | Auditoría periódica con dataset balanceado; opción "no sugerir dim emocional" por instancia |
| 9.6 | El radar chart no transmite intuitivamente la lectura GRETA (4 dim son ortogonales, no comparables como áreas) | Media | Medio | Test de comprensión con 5 líderes reales antes de Beta; alternativa: 4 mini-radares u otro formato |
| 9.7 | El catálogo `@manufosela/*` no tiene capacity para crear todos los componentes nuevos a tiempo | Media | Alto | Sprint 0 dedicado a inventario + roadmap del catálogo; priorizar los 3-5 componentes críticos |
| 9.8 | Coste IA por conversación se dispara si el contexto RAG crece con el histórico | Media | Medio | Cap de tokens del RAG (top-k limitado), ventana deslizante del histórico (últimos 12 meses), Haiku para gradación previa |
| 9.9 | DPIA y compliance toman más tiempo que el desarrollo del MVP | Alta | Medio | Iniciar DPIA en Sprint 0 en paralelo al desarrollo |
| 9.10 | Multi-database Firebase tiene quota inesperada en escenario Escala | Media | Alto | Validar en Architecture; tener plan B (pool de proyectos GCP) |

---

## 10. Preguntas abiertas para Architecture (fase 3)

1. ¿Multi-database por tenant en un solo proyecto Firebase, o un proyecto GCP por tenant? Tradeoffs: quotas vs operación.
2. ¿El cifrado envelope se hace en la Cloud Function (server-side trusted) o en el cliente (zero-knowledge real)? Tradeoffs: simplicidad de recovery vs nivel de garantía.
3. ¿Vertex AI Vector Search vs Firestore Vector Search (GA en 2025)? Tradeoffs: piezas + coste vs flexibilidad.
4. ¿Cloud Run vs Cloud Functions para los handlers? Tradeoffs: cold start, latencia, billing.
5. ¿pgvector dentro de Cloud SQL como alternativa a todo el stack vectorial (más simple, una sola pieza)?
6. ¿Cómo gestionar la rotación de claves KMS sin disrupción? ¿Cada cuánto?
7. ¿Stripe webhooks vs polling para sincronización de plan? Idempotencia.
8. ¿Service worker strategy para offline: stale-while-revalidate, network-first, cache-first por ruta? Para el dashboard activo.
9. ¿Estrategia exacta de testing E2E del flujo de IA (mock LLM vs sandbox real)?

---

## 11. Resumen de decisiones tomadas en Discovery

| Decisión | Resolución |
|---|---|
| Quién tiene cuenta | Sólo el líder. Observadores con link firmado / magic-link. Personas evaluadas SIN cuenta. |
| Consentimiento Art. 9 | Responsabilidad contractual del líder + plantillas + `ConsentRecord` opcional |
| Vistas compartidas (MVP) | Link firmado JWT, expiry, revocable, preview obligatorio |
| Vistas compartidas (Beta) | Magic-link con email verificado |
| Retención | Configurable por instancia, defaults: docs 18m, assessments 5a, sugerencias rechazadas 90d |
| LinkedIn import | NO en MVP |
| Móvil | NO MVP (PWA web). Capacitor en Beta si hay señal |
| Datos fuera de EU | Nunca. Todo en `europe-west` |
| Modelo de datos | Firestore con junction collections; grafo materializado solo si lo justifica el uso |
| Composición UI | Catálogo `@manufosela/*` para todo componente del dashboard; Astro nativo para básicos no reactivos |
| Cifrado | CMEK database + envelope encryption en cliente para campos muy sensibles |
| Trial | 14 días con features de Team, downgrade auto a Solo si no upgrade |
| Co-líderes / handover | NO en MVP |
| API pública | NO en MVP |
| Persona ve su propio informe | NO en MVP |
| **Capa de impacto (RC3)** | Entidades nuevas: `Challenge`, `Outcome`, `EarlySignal`, `Contribution` (refactorizada con hypothesis_reason + dimension_link + status), `Evidence`, `Experiment`, `Gap`, `Intervention`. Épica IMPACT (IM-001..013) |
| **Guardarraíles IA (RC3)** | Entidad polimórfica `GuardrailWarning`. 9 guardarraíles con disparadores específicos. Regla maestra: **avisar, nunca bloquear**. Évaluación onSave **asincrónica**. Épica GUARD (GU-001..012) |
| **Hipótesis de contribución** | Formato literal RC3: *"Creemos que si [persona] [acción], contribuirá a [outcome], porque [razón GRETA]"*. Ciclo de vida: active → confirmed/refuted/consolidated |
| **Experimentos acotados** | 4 campos obligatorios (qué se prueba / qué se observa / cuánto tiempo / qué se decide). Auto-abandono si excede duration + 30d. Vinculación opcional a Contribution |
| **Matriz valor × riesgo** | Heurística de priorización 2×2. Solo campos en Intervention (no entidad propia). Decisiones: act_first / act_with_experiment / monitor / defer |
| **Impacto NUNCA como evaluación** | Dashboard agrupa por outcome, no por persona. Sin rankings ni puntuaciones agregadas por persona |

---

*Fin del Discovery. Siguiente fase: Architecture (Prompt 3).*
