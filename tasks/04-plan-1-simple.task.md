# MVP Foundations de Equipazgo — auth + organizaciones + gestión de equipos/personas

Necesito que descompongas en HUs ejecutables el primer slice del SaaS **Equipazgo** (basado en el framework GRETA).

## Qué construimos en este Plan 1

El esqueleto operativo: un líder se registra, se le crea una organización privada, invita a co-líderes opcionales, da de alta equipos y personas, las asigna entre sí, gestiona campos personalizados, y puede ejercer sus derechos GDPR.

**NO incluye en este plan**: subir CVs, IA / RAG, assessments, visualizaciones, vista de grafo ni vistas compartidas. Eso son los Planes 2-4.

## Stack técnico ya decidido

- **Frontend**: Astro 5 + Lit 3 + JS vanilla con JSDoc (sin TypeScript, sin Tailwind). CSS vanilla con custom properties para theming por tenant. Componentes UI desde el catálogo `@manufosela/*` (radar-chart, app-modal, slide-notification, multi-select, nav-list, element-card, circle-percent, circle-picture, photo-collage, theme-toggle, capture-image, before-after, form-validators).
- **Backend**: Cloud Functions Gen 2 + Firestore Native + Cloud Storage + Cloud KMS, todo en `europe-west1` por GDPR.
- **Auth**: Google Identity Platform (GCIP) con multi-tenancy — un sub-tenant GCIP por organización Equipazgo.
- **Multi-tenancy físico**: una database Firestore nombrada `instance-<orgId>` por cada organización. Cifrado con CMEK del cliente.
- **Pagos**: Stripe (planes Solo, Team, Org).
- **Hosting**: Firebase Hosting.

## Funcionalidades que tienes que descomponer en HUs

### Cuenta + organización
1. Líder se registra con email+password o OAuth Google/Microsoft.
2. Al registrarse, una Cloud Function `signupLeader` crea atómicamente: tenant GCIP, database Firestore con CMEK, Storage bucket con CMEK, KMS keyring/key, namespace en Vector Search (vacío en este plan, se usa en Plan 3). Si algún paso falla, rollback completo.
3. Login + recuperar contraseña (magic-link).
4. Configurar perfil del líder (nombre, locale, timezone, avatar opcional).
5. Configurar tema/branding de la organización (colores `--equipazgo-*`, logo).
6. Suscripción y plan vía Stripe (con webhooks idempotentes).
7. Trial de 14 días con features de Team al signup; downgrade auto a Solo si no upgrade.
8. Configurar política de retención (defaults: documentos 18m, assessments 5a, sugerencias rechazadas 90d).
9. Ver dashboard básico de uso/coste de la organización.
10. Borrar cuenta y todos los datos (GDPR derecho al olvido) — con cascada que deshabilita KMS keys, borra Firestore database, borra Storage bucket, audit log irrevocable.
11. Exportar todos los datos de la organización (GDPR portabilidad).

### Co-líderes (plan Team o superior)
12. Owner invita a un co-líder por email.
13. El invitado acepta la invitación (cuenta GCIP creada/vinculada).
14. Listar líderes de la organización con su rol (owner / co_leader).
15. Owner revoca acceso de un co-líder.
16. Owner transfiere ownership a un co-líder (atómico).

### Equipos
17. Crear equipo (nombre, área, descripción).
18. Editar equipo.
19. Archivar equipo (no se borra, queda con `status=archived`).
20. Listar equipos de la organización.

### Personas (sin cuenta — son objeto de evaluación, no usuarios)
21. Dar de alta persona (nombre, email opcional, rol_actual, foto opcional vía `capture-image`).
22. Editar persona.
23. Archivar persona.
24. Importar personas desde CSV (con preview previo a confirmación, batch atómico).
25. Asignar persona a equipo (relación M:N — `TeamMembership` con `role_in_team`, `start_date`, `end_date`, `is_current`).
26. Quitar persona de equipo (poner `end_date`).
27. Registrar consentimiento de persona (opcional, `ConsentRecord` con texto + archivo cifrado, vía `consent-recorder` que hay que crear).

### Proyectos
28. Crear proyecto (nombre, cliente, fechas).
29. Asignar persona a proyecto (relación M:N — `ProjectContribution`).

### Campos personalizados
30. Definir campos personalizados a nivel organización (max 20, tipos: short_text, long_text, number, date, single_choice, multi_choice, file).
31. Rellenar campos personalizados de una persona (con cifrado a nivel campo si la sensibilidad es "sensible" o "muy sensible").

## Reglas que TODAS las HUs deben respetar

- **Aislamiento entre organizaciones**: nunca cross-tenant. Tests automatizados en CI.
- **Auth obligatoria**: cada Cloud Function valida `request.auth.uid` y pertenencia a la organización.
- **Owner vs co_leader**: solo el owner gestiona suscripción, ownership y borrado de la organización. Los co_leaders pueden todo lo demás.
- **Audit log inmutable** para acciones GDPR, billing y gestión de membresía.
- **Cifrado**: todo en reposo con CMEK. Campos sensibles (consent record, futuro reasoning IA) con envelope encryption en cliente.
- **Componentes UI**: cualquier componente nuevo se publica primero al catálogo `@manufosela/*` y luego se importa. Los componentes básicos no reactivos van en `.astro` puro.

## Spikes técnicos a incluir como HUs separadas

- Setup proyecto GCP + Firebase + GCIP multi-tenancy + KMS.
- Setup Stripe (planes, webhooks idempotentes).
- Diseñar y publicar al catálogo `@manufosela/*` los componentes nuevos necesarios: `file-uploader`, `data-table`, `chip`, `consent-recorder`.
- Setup CI/CD GitHub Actions.
- Plantilla DPIA + privacy notice + RoPA (paralelo, no bloqueante).

## Lo que necesito que generes

Una lista de HUs **atómicas y ejecutables individualmente con `kj run`** (cada una es una unidad de trabajo de un coder). Para cada HU:

- ID con prefijo de épica: `[AUTH-001]`, `[PEOPLE-005]`, `[INFRA-003]`, `[SUBS-002]`, etc.
- Título breve.
- Historia "como [actor], quiero [acción], para [valor]".
- 3-8 criterios de aceptación verificables.
- Dependencias entre HUs (las que deben completarse antes).
- Estimación de esfuerzo XS / S / M / L / XL.
- Notas técnicas relevantes según el stack.
- Subset MVP (las HUs mínimas para que un líder se registre + cree equipo + dé de alta personas).
- Plan de sprints de 2 semanas con HUs agrupadas (asume equipo 2-3 devs + 1 designer).
- Checklist de dependencias externas pre-requisito.

**Importante**: NO escribas un único documento markdown describiendo las HUs. Genera HUs reales como entradas separadas en el plan, cada una ejecutable por separado. El idioma de las HUs en español.
