# Plan 1 — MVP Foundations (Equipazgo)

> **Tarea para el planner**: generar el plan completo de Historias de Usuario (HUs) para el **MVP Foundations** de Equipazgo, cubriendo las épicas **INFRA**, **ACCOUNT** y **PEOPLE**. NO incluir IA ni visualizaciones avanzadas (esos planes vienen en Plan 2-4).
>
> **Output esperado**: lista completa de HUs con el formato definido al final del documento, ordenadas por dependencia, con MVP subset, dependencias externas, spikes técnicos identificados.

---

## 1. Contexto del producto

**Equipazgo** es un SaaS basado en el framework **GRETA** (Guía de Referencia de Equipos de Trabajo Afectivo). Permite a líderes de equipos de ingeniería diagnosticar, componer y mantener su equipo a través de 4 dimensiones: **Rol de contribución (Belbin)**, **Conocimiento**, **Seniority**, **Emocional**. Cada persona tiene un nivel independiente en cada dimensión sobre una escala de 7 niveles con colores (🔴 Tiro / 🟠 Novicius / 🟡 Peritus / 🟢 Expertus / 🔵 Veteranus / 🟣 Primus / ⚪ Magister).

**Modelo de negocio**: SaaS multi-tenant, una instancia (organización) por suscripción. Owner + co-líderes opcionales (plan Team+). Persona evaluada NO tiene cuenta. Vistas read-only compartibles con observadores externos vía link firmado / magic-link.

**Privacidad**: GDPR by design. Datos cifrados en reposo (CMEK por instancia) y en tránsito (TLS 1.3). Cifrado a nivel de campo para datos especialmente sensibles. Privacidad total por instancia (ningún cruce de datos).

---

## 2. Stack técnico decidido (relevante para este plan)

- **Frontend**: Astro 5 + Lit 3 + JavaScript vanilla con JSDoc/.d.ts. CSS vanilla con custom properties. Sin TS, sin Tailwind.
- **UI components**: catálogo `@manufosela/*` (Lit web components). Componentes nuevos → publicar primero al catálogo, luego importar. Componentes básicos no reactivos → Astro nativo.
- **Backend**: Cloud Functions Gen 2 + Cloud Run (servicios largos) + Firestore Native + Cloud Storage + Cloud KMS.
- **Auth**: Google Identity Platform (GCIP) con multi-tenancy (un sub-tenant por instancia).
- **BBDD**: una database Firestore nombrada por instancia (`instance-<orgId>`). Pool de proyectos GCP cuando se llegue a ~80 instancias activas.
- **Region**: `europe-west1` o `europe-west4`. Sin excepciones (EU residency obligatoria).
- **Pagos**: Stripe.
- **CI/CD**: GitHub Actions, deploy auto staging, manual prod (canary 10% → 100%).
- **IaC**: Terraform para infra base + Cloud Function `signupLeader` para provisioning de instancias.
- **Hosting**: Firebase Hosting + CDN.
- **Idioma del producto**: español primero (i18n-ready desde el inicio).

---

## 3. Épicas a planificar

### 3.1 ÉPICA INFRA — Infraestructura y operaciones

Provisioning automático, cifrado, backup, monitoring, CI/CD, GDPR.

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| IN-001 | Provisioning automático de instancia | sistema (auto en signup) | usuario completa registro | tenant GCIP + DB Firestore + KMS key + Vector namespace + Storage bucket creados (<5s) |
| IN-002 | Rollback de provisioning fallido | sistema | provisioning falló | recursos parciales eliminados; usuario puede reintentar |
| IN-003 | Backup diario por instancia | sistema (cron) | instancia activa | snapshot Firestore + Storage en bucket de backup, retention 30 días |
| IN-004 | Restore desde backup | admin SaaS (con autorización del líder) | backup existe | instancia restaurada en sandbox antes de overwrite |
| IN-005 | Monitoring + alerting | admin SaaS | sistema activo | métricas en Cloud Monitoring + alertas a oncall |
| IN-006 | Pipeline CI/CD | dev team | repo + tests verde | deploy automático a staging; promoción manual a prod |
| IN-007 | Borrado completo GDPR | sistema (al recibir AC-007) | request confirmada | cascada: KMS keys disabled → Firestore database delete → Storage delete → Vector delete → KMS keys destroyed (después de 30 días gracia) → audit log irrevocable |
| IN-008 | Audit log de accesos a datos sensibles | sistema | acceso a campo cifrado o documento | log en Cloud Audit Logs con quién, qué, cuándo, IP |
| IN-009 | Exportación GDPR completa de instancia | sistema (al recibir AC-008) | request confirmada | ZIP firmado generado en Storage temp, link de descarga 24h |

### 3.2 ÉPICA ACCOUNT — Cuenta, organización y co-líderes

Registro, login, gestión de organización, suscripción, GDPR para el usuario.

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| AC-001 | Registrarse | nuevo líder | email no registrado | LeaderAccount + Organization (Instance) creadas; provisioning IN-001 disparado |
| AC-002 | Login | líder | cuenta activa | sesión iniciada; organización cargada |
| AC-003 | Recuperar contraseña | líder | email registrado | email enviado con magic-link |
| AC-004 | Configurar perfil personal | líder | sesión iniciada | datos del líder actualizados (display_name, locale, timezone, foto avatar opcional) |
| AC-005 | Configurar tema/branding de la organización | owner | sesión iniciada | `Theme` actualizado (colores `--equipazgo-*`, logo opcional); CSS regenerado |
| AC-006 | Suscripción y plan (Stripe) | owner | sesión iniciada | plan actualizado en Stripe + Firestore |
| AC-007 | Borrar cuenta y todos los datos (GDPR) | owner | confirmación 2-step + re-login | organización → `pending_deletion`; cascada IN-007 a las 24h salvo cancelación |
| AC-008 | Exportar mis datos (GDPR portabilidad) | owner | sesión iniciada | ZIP firmado vía IN-009 |
| AC-009 | Configurar política de retención | owner | sesión iniciada | retention policy actualizada |
| AC-010 | Ver dashboard de uso/coste | líder | sesión iniciada | métricas de la organización (storage, conv IA usadas — siempre 0 en este plan, etc.) |
| AC-011 | Invitar co-líder | owner | plan Team o superior | email de invitación enviado; pending hasta aceptación |
| AC-012 | Aceptar invitación de co-líder | invitado (sin cuenta o con cuenta) | invitación válida | cuenta GCIP creada/vinculada; `OrganizationMembership` con role=co_leader |
| AC-013 | Listar líderes de la organización | líder | sesión | listado de miembros con roles |
| AC-014 | Revocar acceso de co-líder | owner | co-líder existe | `OrganizationMembership.revoked_at` puesto; sesiones del co-líder invalidadas |
| AC-015 | Transferir ownership a co-líder | owner | co-líder existe | role swap atómico; antiguo owner pasa a co_leader |
| AC-016 | Configurar permisos granulares de co-líder (opcional, Beta) | owner | co-líder existe | `OrganizationMembership.permissions.scope_teams/scope_dimensions` actualizado |

### 3.3 ÉPICA PEOPLE — Equipos, personas, proyectos, campos personalizados

Estructura del equipo: equipos, personas, proyectos, custom fields. **Sin assessments aún (Plan 2)**.

| ID | Caso de uso | Actor | Precondiciones | Postcondiciones |
|---|---|---|---|---|
| PE-001 | Crear equipo | líder | sesión | nuevo `Team` |
| PE-002 | Editar equipo | líder | equipo existe | `Team` actualizado |
| PE-003 | Archivar equipo | líder | equipo sin actividad reciente | `Team.status = archived` |
| PE-004 | Listar equipos | líder | sesión | listado |
| PE-005 | Dar de alta persona | líder | sesión | nueva `Person` con datos básicos (nombre, email opcional, current_role, foto opcional) |
| PE-006 | Editar persona | líder | persona existe | `Person` actualizada |
| PE-007 | Asignar persona a equipo (relación M:N) | líder | persona y equipo existen | `TeamMembership` creado |
| PE-008 | Quitar persona de equipo | líder | membership existe | `TeamMembership.end_date` puesto, `is_current=false` |
| PE-009 | Archivar persona | líder | persona existe | `Person.status = archived` |
| PE-010 | Importar personas desde CSV | líder | CSV válido (template proporcionado) | N personas creadas (atómico — todo o nada); preview previo a confirmación |
| PE-011 | Definir campos personalizados | líder | sesión | `CustomField` creados (max 20 por instancia, 7 tipos: short_text, long_text, number, date, single_choice, multi_choice, file) |
| PE-012 | Crear proyecto | líder | sesión | nuevo `Project` |
| PE-013 | Asignar persona a proyecto (relación M:N) | líder | persona y proyecto existen | `ProjectContribution` creada |
| PE-014 | Registrar consentimiento de persona (opcional) | líder | persona existe | `ConsentRecord` con soporte (texto/archivo) cifrado |

---

## 4. Detalle técnico clave para este plan

### 4.1 Provisioning automático de organización (ADR-002, ADR-010)

`signupLeader` Cloud Function orquesta secuencialmente:
1. Crea cuenta en GCIP (tenant `tenant-leaders`).
2. Llama `instance-allocator` para decidir proyecto Firebase destino (pool).
3. Crea KMS keyring + key `instance-<orgId>-master`.
4. Crea Firestore database `instance-<orgId>` con CMEK.
5. Crea Storage bucket `gs://greta-instance-<orgId>` con CMEK.
6. Configura Firestore Vector Search index (para Plan 3 — vacío en este plan).
7. Crea documento `Instance` con metadata + `OrganizationMembership` con role=owner.
8. Envía email bienvenida.

**Atomicidad**: marker `provisioning_state` en Firestore (`pending` → `active` o `failed`). Si cualquier paso falla → rollback inverso de los pasos completados. Idempotente: retry seguro tras fallo.

**Failure recovery**: cron horario detecta `provisioning_state=pending` con `>1h` y reintenta o limpia.

**Objetivo**: <5s p95 cuando todos los servicios responden normales.

### 4.2 Modelo de datos relevante (Firestore)

**Database global `default`** (en proyecto Firebase, no por instancia):

```jsonc
// Colección: instances
{
  "id": "org_<nanoid>",
  "owner_uid": "uid_<owner>",
  "org_name": "string",
  "members": ["uid_owner", "uid_co_leader_1", ...],  // denormalizado
  "plan": "trial | solo | team | org | enterprise",
  "plan_expires_at": "Timestamp | null",
  "stripe_customer_id": "string | null",
  "stripe_subscription_id": "string | null",
  "firestore_db_name": "instance-<orgId>",
  "firestore_project_id": "greta-prod-1 | greta-prod-2 | ...",
  "kms_keyring": "instance-<orgId>",
  "kms_key_master": "instance-<orgId>-master",
  "storage_bucket": "greta-instance-<orgId>",
  "vector_namespace": "<orgId>",
  "provisioning_state": "pending | active | failed | pending_deletion | deleted",
  "deletion_requested_at": "Timestamp | null",
  "created_at": "Timestamp",
  "updated_at": "Timestamp"
}

// Colección: leaders
{
  "uid": "string",
  "email": "string (unique)",
  "display_name": "string",
  "avatar_url": "gs://path | null",
  "locale": "es | en",
  "timezone": "string (IANA)",
  "created_at": "Timestamp",
  "last_login_at": "Timestamp"
}

// Colección: organization_memberships
{
  "id": "<orgId>_<uid>",
  "org_id": "string",
  "uid": "string",
  "role": "owner | co_leader",
  "permissions": {
    "manage_subscription": false,
    "transfer_ownership": false,
    "delete_organization": false,
    "manage_co_leaders": false,
    "scope_teams": "all" | ["team_id_1", ...],
    "scope_dimensions": "all" | ["knowledge", ...]
  },
  "invited_by": "uid",
  "invited_at": "Timestamp",
  "accepted_at": "Timestamp | null",
  "revoked_at": "Timestamp | null"
}
```

**Database por instancia `instance-<orgId>`** (sólo las colecciones de este plan):

```jsonc
// settings (1 doc)
{
  "id": "default",
  "theme": {
    "primary": "#color", "secondary": "#color",
    "logo_url": "gs://path | null",
    "level_colors": {
      "tiro": "#e74c3c", "novicius": "#e67e22",
      "peritus": "#f1c40f", "expertus": "#27ae60",
      "veteranus": "#3498db", "primus": "#9b59b6", "magister": "#ecf0f1"
    }
  },
  "retention_policy": {
    "documents_months": 18,
    "assessments_years": 5,
    "rejected_suggestions_days": 90
  },
  "shareable_view_max_expiry_days": 90,
  "custom_fields_count_limit": 20
}

// teams
{ "id", "name", "area", "description", "status", "created_at", "archived_at" }

// people
{ "id", "name", "email", "current_role", "profile_shape", "avatar_url", "status", "created_at", "archived_at" }

// memberships (junction Person-Team)
{ "id": "<personId>_<teamId>", "person_id", "team_id", "role_in_team", "start_date", "end_date", "is_current" }

// projects
{ "id", "name", "client", "start_date", "end_date", "status" }

// contributions (junction Person-Project)
{ "id": "<personId>_<projectId>", "person_id", "project_id", "role", "period", "description" }

// custom_fields (definiciones)
{ "id", "name", "type", "options", "max_length", "sensitivity", "ai_processable", "created_at", "order" }

// custom_field_values
{ "id", "person_id", "custom_field_id", "value_plain", "value_encrypted", "updated_at" }

// consent_records
{ "id", "person_id", "consent_type", "granted_at", "support_text", "support_storage_path_encrypted", "revoked_at", "expires_at" }

// audit_events (inmutable, append-only)
{ "id", "actor_type", "actor_id", "action", "resource_type", "resource_id", "ip", "user_agent", "before", "after", "timestamp" }
```

### 4.3 Índices Firestore críticos para este plan

| Colección | Campos | Tipo | Razón |
|---|---|---|---|
| `instances` (global) | `owner_uid` asc | composite | listar organizaciones del usuario |
| `instances` (global) | `provisioning_state` asc | composite | cron de retry/cleanup |
| `organization_memberships` (global) | `uid` asc, `role` asc | composite | listar organizaciones a las que pertenece un usuario |
| `organization_memberships` (global) | `org_id` asc, `revoked_at` asc | composite | listar miembros activos de una org |
| `memberships` (instance) | `team_id` asc, `is_current` desc | composite | miembros actuales de un equipo |
| `memberships` (instance) | `person_id` asc, `is_current` desc | composite | equipos actuales de una persona |
| `audit_events` (instance) | `actor_id` asc, `timestamp` desc | composite | auditoría por usuario |

### 4.4 Reglas de negocio aplicables a este plan

1. **Aislamiento entre instancias** (no negociable): toda lectura/escritura lleva `instance_id` en path o filtros obligatorios. Tests automatizados de aislamiento cross-tenant en CI.
2. **Roles owner vs co_leader**: owner tiene privilegios exclusivos sobre suscripción, transferencia de ownership, borrado de organización y gestión de co-líderes. Co-leaders pueden gestionar equipos/personas/datos.
3. **Audit log inmutable**: cada cambio sensible (membership, GDPR action, billing) genera `AuditEvent`. No se permite update/delete.
4. **Plan Solo**: 1 owner, 0 co-líderes. Plan Team: hasta 5 co-líderes. Plan Org: hasta 20.
5. **Trial de 14 días** con features Team al signup. Después downgrade automático a Solo si no upgrade.
6. **Custom fields**: max 20 por organización. Sensibilidad determina cifrado aplicado.
7. **Provisioning atómico o nada**: ningún recurso medio-creado.
8. **Derecho al olvido**: cascada en 24h con grace period; KMS keys disabled inmediatamente; destroy a los 30 días.

### 4.5 Infraestructura mínima para este plan

- **Environments**: dev, staging, prod (`greta-prod-1`).
- **CI/CD GitHub Actions**: lint + unit tests + integration tests (Firestore emulator) + accessibility tests + SAST → deploy auto staging → manual prod canary.
- **Monitoring + alerting**: Cloud Monitoring con alertas en `signupLeader` p95 latency, error rate functions, KMS decrypt anomalous rate, cost per instance.
- **Backup**: snapshot diario Firestore por instancia, retention 30d, RPO ≤24h.

---

## 5. Catálogo `@manufosela/*` disponible para este plan

**Existentes (importar y usar)**:
- `@manufosela/app-modal@2.3.0` — modales de confirmación, edición, wizards (CSV import).
- `@manufosela/slide-notification@2.1.0` — toasts (guardado, error, éxito).
- `@manufosela/nav-list@3.0.0` — tabs y menú lateral del dashboard.
- `@manufosela/element-card@2.3.9` — cards de equipos y personas en listados.
- `@manufosela/circle-picture@2.2.1` — avatares.
- `@manufosela/photo-collage@1.1.0` — vista mosaico de equipo.
- `@manufosela/multi-select@2.1.0` — filtros (selección múltiple de equipos, personas).
- `@manufosela/theme-toggle@2.0.1` — light/dark.
- `@manufosela/capture-image@1.1.0` — captura foto perfil.
- `@manufosela/form-validators@1.0.0` + `automatic_form_validation@1.7.0` — validación de inputs (signup, alta persona, perfil).

**A publicar al catálogo (gaps necesarios para este plan)**:
- `@manufosela/file-uploader` — drag&drop de CSV (PE-010), avatar opcional.
- `@manufosela/data-table` — listado de personas, equipos, miembros con sorting/filtros.
- `@manufosela/chip` — tags de equipo, área, custom fields.
- `@manufosela/consent-recorder` — captura ConsentRecord (PE-014).

---

## 6. Spikes técnicos identificados (proponer como HUs separadas)

1. **SPK-001 — Setup proyecto GCP + Firebase + GCIP multi-tenancy + KMS** (1 sprint, M).
2. **SPK-002 — Implementar pool de proyectos Firebase + `instance-allocator`** (1 sprint, M). Necesario para escalar más allá de 80 instancias; en MVP puede vivir simplificado a 1 proyecto.
3. **SPK-003 — Diseñar y publicar componentes nuevos al catálogo `@manufosela/*` (file-uploader, data-table, chip, consent-recorder)** (2 sprints, L). Bloquea HUs que los consumen.
4. **SPK-004 — Setup Stripe (productos, planes, webhooks idempotentes)** (1 sprint, M).
5. **SPK-005 — Setup CI/CD GitHub Actions (lint, tests, deploys, secrets)** (1 sprint, M).
6. **SPK-006 — Plantilla DPIA + privacy notice + RoPA** (paralelo, no bloqueante código).

---

## 7. Dependencias externas a configurar antes de empezar

- Cuenta Google Cloud con billing activo + créditos iniciales aplicados.
- Proyecto GCP `greta-dev`, `greta-staging`, `greta-prod-1` creados.
- Identity Platform habilitado (con multi-tenancy GA).
- Firestore Native habilitado en cada proyecto.
- Cloud KMS habilitado.
- Cuenta Stripe (modo test al inicio, prod al lanzamiento).
- Dominio `equipazgo.app` (o el que se elija) registrado + DNS apuntando a Firebase Hosting.
- Cuenta GitHub con repo `equipazgo` (o el nombre elegido) creado.
- Cuenta de email transaccional (SendGrid o Postmark) con dominio verificado.
- Asesor legal o DPO designado (para revisar plantillas DPIA y privacy notice).

---

## 8. INSTRUCCIONES PARA EL PLANNER (formato de output)

Genera el plan completo de HUs siguiendo estas reglas estrictas:

### 8.1 Formato de cada HU

```
**ID**: [EPIC-NNN] (ej: ACCOUNT-001, PEOPLE-005, INFRA-003)
**Título**: breve y descriptivo
**Historia**: Como [actor], quiero [acción], para [valor/objetivo]
**Criterios de aceptación**:
  - mínimo 3, máximo 8 condiciones verificables (Given/When/Then implícito)
**Dependencias**: lista de IDs de HUs que deben estar completas antes (vacía si ninguna)
**Estimación**: XS / S / M / L / XL (complejidad relativa, no horas)
**Notas técnicas**: consideraciones de implementación según el stack y la arquitectura
**Componentes UI**: lista de componentes del catálogo `@manufosela/*` que usa (existentes o a crear)
**Tablas Firestore involucradas**: colecciones afectadas
```

### 8.2 Reglas de planning

1. **Cubre las 3 épicas completas** (INFRA, ACCOUNT, PEOPLE) con TODOS los casos de uso listados arriba (9 + 16 + 14 = 39 casos de uso → ~39 HUs mínimo, posiblemente más si algún caso se descompone).
2. **Identifica el MVP del MVP**: subset mínimo que permite a un líder registrarse, crear su organización, dar de alta su primer equipo y sus personas. Marca cada HU como `MVP=true` o `MVP=false`.
3. **Identifica spikes**: incluye los SPK-001 a SPK-006 listados arriba como HUs separadas.
4. **Resuelve dependencias internas**: ej PE-007 (asignar a equipo) depende de PE-001 (crear equipo) y PE-005 (alta persona); AC-011 (invitar co-líder) depende de AC-001 + AC-006 (plan Team).
5. **Ordena las HUs en una secuencia ejecutable** respetando dependencias.
6. **Propón sprints de 2 semanas** con HUs agrupadas, asumiendo equipo de 2-3 devs + 1 designer.
7. **Lista riesgos del plan** y proponer mitigación.

### 8.3 Output esperado

- Lista completa de HUs con todos los campos
- Subset MVP con justificación
- Plan de sprints (objetivo de cada sprint + HUs incluidas)
- Lista de spikes técnicos con duración estimada
- Checklist de dependencias externas (sección 7 arriba) marcadas como pre-requisito
- Estimación total de duración hasta completar Plan 1

**IMPORTANTE**:
- NO incluyas HUs de las épicas PROFILE, ASSESSMENT, AI, VIZ, GRAPH, SHARE — esas van en Planes 2-4.
- Las HUs deben ser **atómicas y ejecutables individualmente** por un coder (Karajan Code las ejecutará una a una con `kj run`).
- Cada HU debe tener criterios de aceptación verificables automáticamente cuando sea posible.
- Si detectas algún gap en la información proporcionada → marcalo como "BLOQUEANTE" en una sección final, NO inventes.
