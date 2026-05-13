# GRETA SaaS — workspace de Karajan Code

Workspace para construir GRETA SaaS con Karajan Code v2.12.0.

## Estructura

```
greta-app/
├── master.md              # documento maestro original (referencia, no editar)
├── context-greta.md       # Partes 1+2 — contexto que cada prompt necesita
├── prompts/
│   ├── 01-research.md     # PROMPT 1 — kj researcher
│   ├── 02-discovery.md    # PROMPT 2 — kj discover
│   ├── 03-architecture.md # PROMPT 3 — kj architect
│   └── 04-hus.md          # PROMPT 4 — kj plan generate
└── outputs/               # se crea cuando arranque la fase 1
    ├── GRETA-research.md
    ├── GRETA-discovery.md
    ├── GRETA-architecture.md
    ├── GRETA-HUs.md
    └── GRETA-summary.md   # último paso: decisiones clave de cada fase
```

## Orden de ejecución

Reglas estrictas (del documento maestro):

1. No empezar el siguiente prompt hasta que el anterior esté completo y guardado
2. Cada prompt usa el output del anterior como contexto adicional
3. Si surge una ambigüedad bloqueante, parar y preguntar
4. Al terminar los 4 → generar `GRETA-summary.md`

## Comandos

```bash
# 1) Init del proyecto Karajan (una sola vez)
cd ~/ws_firebase/greta-app
kj init

# 2) Fase Research
kj researcher --task-file prompts/01-research.md

# 3) Fase Discovery (sólo cuando Research esté validado)
kj discover --task-file prompts/02-discovery.md

# 4) Fase Architecture (sólo cuando Discovery esté validado)
kj architect --task-file prompts/03-architecture.md

# 5) Fase HUs (sólo cuando Architecture esté validada)
kj plan generate --task-file prompts/04-hus.md

# 6) Ejecución de las HUs (sub-pipeline por HU)
kj run --plan <plan-id>
```

## Notas

- Cada output se revisa **a mano** antes de pasar al siguiente.
- v2.12.0 mostrará **plan adherence** en el `summary.md` de cada `kj run`, midiendo cuánto el coder respetó la HU.
- El proyecto de KJ se llama `GRETA` (abrev. `GRT`); el de Firebase será `greta-app` o como decidas.
