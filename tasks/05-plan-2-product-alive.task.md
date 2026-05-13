# Plan 2 de GRETA — Producto vivo: ficha, assessments, IA, impacto y guardarraíles

Necesito que descompongas en HUs ejecutables el segundo slice del SaaS **GRETA** (basado en el framework GRETA v1.0-RC3).

## Qué construimos en este Plan 2

Sobre el Plan 1 (auth, organizaciones, equipos, personas — ya en disco), añadimos las cinco épicas que dan vida al producto y permiten una demo end-to-end: ficha completa de persona con CV procesado por IA, assessments en las cuatro dimensiones GRETA con sus siete niveles, pipeline RAG que sugiere niveles a partir de transcripciones O2O, capa de impacto (reto del equipo → outcomes → señales tempranas → hipótesis de contribución → evidencias → experimentos) y los nueve guardarraíles que avisan sin bloquear.

**Asumido del Plan 1**: existen `Instance`, `LeaderProfile`, `Team`, `Person`, `TeamMembership`, `Project`, `ProjectContribution`, `CustomField`. Las HUs de este Plan 2 dependen de esas entidades.

**NO incluye en este plan**: visualizaciones avanzadas (radar, dashboard de equipo, mapa de calor), vista de grafo, vistas compartidas read-only. Eso es Plan 3.

## Stack técnico ya decidido (mismo Plan 1)

- **Frontend**: Astro 5 + Lit 3 + JS vanilla con JSDoc. CSS vanilla con custom properties. Componentes del catálogo `@manufosela/*` exclusivamente; componentes nuevos se publican al catálogo antes de usarse aquí.
- **Backend**: Cloud Functions Gen 2 + Firestore Native + Cloud Storage + Cloud KMS, todo en `europe-west1` por GDPR.
- **IA**: Claude Sonnet 4.6 vía Vertex AI para análisis de transcripciones; Claude Haiku 4.5 vía Vertex AI para CV parsing, guardarraíles 1-4 / 6 / 9 y narrativa de los deterministas. Embeddings con `text-embedding-005`. Vector store: Firestore Vector Search con namespace por instancia.
- **Cifrado de campos sensibles**: envelope encryption con DEK derivado de la KMS key de la instancia para `transcripciones`, `observation_text` de evidencias y `reasoning` de sugerencias IA sobre dimensión emocional.

## Funcionalidades que tienes que descomponer en HUs

### Épica PROFILE — ficha de persona

1. Subir CV (PDF/DOCX, <10MB) cifrado en Storage con envelope encryption.
2. Cloud Function `processCV` triggerizada por evento Storage: descifrar → pdf-parse/mammoth → Haiku con JSON Schema → `DocumentExtraction` + `AiSuggestion(status=pending)` con citas + embeddings de párrafos relevantes.
3. UI de revisión de extracción: por cada campo sugerido, ver cita textual del CV, botones Aceptar / Editar / Rechazar con motivo opcional.
4. Aceptación enriquece `Person`; rechazo se mantiene en histórico 90 días.
5. Añadir logros manualmente, ver historial de documentos, listar sugerencias de preguntas IA por persona.
6. Atender derechos GDPR a nivel persona: export y borrado en cascada.

### Épica ASSESSMENT — diagnóstico manual

7. Crear assessment manual de las 4 dimensiones (rol Belbin, conocimiento, seniority, emocional) con los 7 niveles (Tiro, Novicius, Peritus, Expertus, Veteranus, Primus, Magister) y sus colores.
8. Justificar cada nivel con comportamientos concretos (texto libre + tags reutilizables).
9. Editar assessment genera nueva versión sin sobrescribir (`Assessment.version` incremental).
10. Ver historial de assessments por persona; marcar assessment como obsoleto cuando caduca.
11. Comparar dos assessments de la misma persona en el tiempo (preparado para el componente `before-after` de visualización Plan 3).

### Épica AI — procesamiento con IA

12. Subir transcripción O2O o resumen escrito por el líder; cifrado y envío a pipeline.
13. Cloud Function `processConversation`: descifrar → chunking semántico ~800 tokens → embeddings → Vector Search top-k 5-10 filtrado por persona → construcción contexto RAG (GRETA framework estático + chunks históricos + texto nuevo) → Claude Sonnet con system prompt que devuelve sugerencias por dimensión con razonamiento + 2-5 citas literales + confidence + preguntas para la próxima conversación.
14. Validar output del LLM con Valibot antes de persistir; rechazar si el schema falla y reintentar.
15. UI presenta las 4 dimensiones con la sugerencia, razonamiento, citas resaltadas en el texto fuente, botones Aceptar / Ajustar (slider) / Rechazar.
16. Cada acción crea `AssessmentValidation` y actualiza el Assessment de la persona.
17. Sugerencias rechazadas quedan en histórico 90 días con motivo.
18. Reprocesar una transcripción cuando se mejora el modelo (versionado del prompt + modelo en la sugerencia).
19. Generar sugerencias de preguntas para la próxima O2O por persona, on-demand y por cron semanal.

### Épica IMPACT — capa de impacto (RC3 §12)

20. Definir reto específico del equipo (`Challenge` con `is_current=true`; el anterior se retira con `retired_at`).
21. Definir 3-5 outcomes por equipo, cada uno vinculado al Challenge activo; evaluación asincrónica por guardarraíl 1.
22. Definir 1-2 señales tempranas (`EarlySignal`) por outcome, con métrica observable y registro de lecturas (campo `history[]`).
23. Definir hipótesis de contribución individual con formato literal RC3: *"Creemos que si [persona] [acción], contribuirá a [outcome], porque [razón GRETA]"* con `dimension_link` (1..4) y `status=active`; evaluada por guardarraíl 2.
24. Revisar hipótesis en O2O y actualizar `status` a `confirmed` / `refuted` / `consolidated` con `last_reviewed_at` automático.
25. Registrar evidencias (`Evidence`) con texto observable, fecha, vínculo a una o varias contribuciones y a sus outcomes; evaluada por guardarraíl 3 y cifrada con envelope.
26. Diseñar experimentos de equipo acotados (`Experiment` con qué se prueba, qué se observa, cuánto tiempo, qué se decide); vinculados a un outcome y opcionalmente a una contribución.
27. Cerrar experimento al vencer la duración (recordatorio automático); registrar resultado + decisión.
28. Auto-abandono de experimentos que excedan `duration_days + 30` sin cerrar.
29. Detectar `Gap`s automáticamente desde el diagnóstico (bus factor, cobertura roles, etc.) y proponer `Intervention`s con `value_score` y `risk_score`.
30. Marcar intervenciones con `priority_decision` (`act_first` / `act_with_experiment` / `monitor` / `defer`) según la matriz valor × riesgo RC3.

### Épica GUARD — guardarraíles de IA (RC3 §13)

> Regla maestra: los guardarraíles AVISAN, nunca BLOQUEAN. Cada detección crea `GuardrailWarning` persistida y visible en historial.

31. Guardarraíl 1 — evaluar outcome contra los 3 criterios (cambio real / dentro de control / observable) con Haiku 4.5 onSave asincrónico.
32. Guardarraíl 2 — evaluar contribución individual con los mismos 3 criterios aplicados a la hipótesis.
33. Guardarraíl 3 — evaluar evidencia (observación concreta vs valoración subjetiva) con pregunta socrática como sugerencia.
34. Guardarraíl 4 — detector inline durante el pipeline `processConversation` que señala si en la transcripción hubo evaluación en lugar de alineamiento o se habló de niveles explícitamente.
35. Guardarraíl 5 — advertir cuando se asigna un nivel sin suficiente base observacional (umbral configurable; lógica determinista + narrativa Haiku).
36. Guardarraíl 6 — revisar contenido de vistas compartidas antes de generar link (advierte si revela niveles comparativos entre personas).
37. Guardarraíl 7 — contextualizar bajadas de nivel en evolución temporal (determinista + narrativa Haiku cacheada).
38. Guardarraíl 8 — cron diario por instancia que detecta personas sin actividad reciente (>60 días); crea warnings con sugerencia de acción.
39. Guardarraíl 9 — detectar patrones UI de comparación inter-personas (filtros/sort por nivel entre personas) y redirigir con banner contextual.
40. Listar todas las advertencias filtrables por guardarraíl, entidad, severidad, estado.
41. Marcar advertencia como `acknowledged` o ignorar con motivo opcional (queda en histórico).
42. Reintentos de guardarraíles que fallan (Pub/Sub `guardrail-retry-queue` con backoff, max 3 reintentos, fail-open).

## Reglas que TODAS las HUs deben respetar

- **Aislamiento entre instancias**: nunca cross-tenant; cada Cloud Function valida `request.auth.uid` y pertenencia.
- **IA AVISA, nunca BLOQUEA**: ningún guardarraíl impide guardar. Toda detección crea `GuardrailWarning` con sugerencia opcional.
- **Capa de impacto es ALINEAMIENTO, no EVALUACIÓN**: ningún dashboard ranquea personas por evidencias o puntuación agregada. Agrupación por outcome, no por persona.
- **Tono del LLM en guardarraíles**: colega que pregunta honestamente; nunca juzga ni dice "esto está mal".
- **System prompts versionados**: `prompts/guardrail-N.md` con SemVer; `model_used` + `prompt_version` se persisten en cada warning para trazabilidad.
- **Cifrado envelope** para `transcripciones`, `observation_text` de evidencias y `reasoning` IA sobre dim emocional.
- **Validación Valibot** del output de cada llamada LLM antes de persistir.
- **Audit log inmutable** de cada llamada LLM (instance_id, modelo, coste, resultado) y de cada lectura de campo cifrado.
- **Componentes UI**: dashboard y editores se montan con composiciones sobre el catálogo `@manufosela/*`. Componentes nuevos del catálogo necesarios para este plan: `tree-view`, `quadrant-matrix`, `inline-banner`, `rich-text-viewer`, `heatmap-chart`, `level-selector` y `file-uploader` (si no entraron en Plan 1).

## Spikes técnicos a incluir como HUs separadas

- Provisioning del namespace de Vector Search por instancia (si quedó como stub en Plan 1, completarlo aquí).
- Setup del versionado de prompts (`prompts/*.md` con SemVer + tests determinísticos).
- Setup del job runner de cron por instancia (Cloud Scheduler + topic Pub/Sub por instancia o por guardarraíl).
- Setup del retry-queue de guardarraíles (Pub/Sub + dead-letter topic + cron de procesado).
- Diseñar y publicar al catálogo `@manufosela/*` los componentes nuevos pendientes: `tree-view`, `quadrant-matrix`, `inline-banner`, `rich-text-viewer`, `heatmap-chart`, `level-selector`.
- Plantilla de DPIA actualizada para el tratamiento por IA y guardarraíles (sigue siendo paralelo, no bloqueante).

## Lo que necesito que generes

Una lista de HUs **atómicas y ejecutables individualmente con `kj run`** (cada una es una unidad de trabajo de un coder). Para cada HU:

- ID con prefijo de épica: `[PROFILE-001]`, `[ASSESS-003]`, `[AI-002]`, `[IMPACT-005]`, `[GUARD-001]`, etc.
- Título breve.
- Historia "como [actor], quiero [acción], para [valor]".
- 3-8 criterios de aceptación verificables.
- Dependencias entre HUs (las que deben completarse antes), incluyendo dependencias hacia HUs del Plan 1 cuando aplique (referenciar como `Plan1:AUTH-001`, `Plan1:PEOPLE-001`, etc.).
- Estimación de esfuerzo XS / S / M / L / XL.
- Notas técnicas relevantes según el stack.
- Subset MVP del Plan 2 (las HUs mínimas para que un líder suba un CV, asigne niveles iniciales, procese una conversación y defina un outcome con guardarraíl).
- Plan de sprints de 2 semanas con HUs agrupadas (asume equipo 2-3 devs + 1 designer).
- Checklist de dependencias externas pre-requisito.

**Importante**: NO escribas un único documento markdown describiendo las HUs. Genera HUs reales como entradas separadas en el plan, cada una ejecutable por separado. El idioma de las HUs en español.
