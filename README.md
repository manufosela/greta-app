# greta-app

App SaaS de **GRETA** — herramienta privada para liderazgos de ingeniería que creen que las personas son la estrategia. Diagnostica composición de equipo, mide impacto real (no outputs), procesa transcripciones de 1:1 con IA y construye una red de liderazgo afectivo entre TLs, EMs y CTOs.

> El **framework GRETA** (metodología, escala, dimensiones) vive en un repositorio aparte: `github.com/manufosela/greta`. Aquí está la **implementación** del framework como producto.

---

## Estructura del repo

```
greta-app/
├── README.md
├── .gitignore
│
├── docs/                       # Documentación viva del producto
│   ├── GRETA-v1.0-RC3.md       # (referencia al framework — copia o symlink al repo greta)
│   ├── research.md             # output de la fase Research de Karajan
│   ├── discovery.md            # output de la fase Discovery
│   ├── architecture.md         # output de la fase Architecture
│   └── HUs.md                  # output del plan de Historias de Usuario
│
├── tasks/                      # SPEC versionada del proyecto (input de Karajan)
│   ├── 01-research.task.md
│   ├── 04-plan-1-foundations.task.md
│   ├── 04-plan-1-simple.task.md
│   └── 05-plan-2-product-alive.task.md
│
├── karajan/                    # Inputs de Karajan Code (transitorios)
│   ├── master.md               # documento maestro original (referencia, no editar)
│   ├── context-greta.md        # Partes 1+2 — contexto que cada prompt necesita
│   └── prompts/
│       ├── 01-research.md
│       ├── 02-discovery.md
│       ├── 03-architecture.md
│       └── 04-hus.md
│
├── .karajan/                   # Reglas del equipo / agentes para Karajan
│   ├── coder-rules.md
│   └── review-rules.md
│
└── .claude/commands/           # Slash-commands de Karajan para Claude Code
```

Aún no hay `src/` — se añadirá cuando comience la ejecución de HUs.

## Convenciones

- **`tasks/`** es la spec versionada del proyecto. Karajan **solo lee** de ahí; nunca escribe.
- **`docs/`** es la documentación viva (outputs de las fases de Karajan).
- **`karajan/`** son los inputs efímeros del proceso. Una vez ejecutados y validados los 4 prompts, esta carpeta podrá archivarse.
- Lo que Karajan genera en runtime (`~/.kj/`, `.kj/`, `.agent/`, `.scannerwork/`) va al `.gitignore`.

## Orden de ejecución (reglas estrictas del documento maestro)

1. No empezar el siguiente prompt hasta que el anterior esté completo y guardado.
2. Cada prompt usa el output del anterior como contexto adicional.
3. Si surge una ambigüedad bloqueante, parar y preguntar.
4. Al terminar los 4 → generar `docs/summary.md` con las decisiones clave.

## Comandos

```bash
# 1) Init del proyecto Karajan (una sola vez)
cd ~/ws_firebase/greta-app
kj init

# 2) Fase Research
kj researcher --task-file tasks/01-research.task.md

# 3) Fase Discovery (sólo cuando Research esté validado)
kj discover --task-file karajan/prompts/02-discovery.md

# 4) Fase Architecture (sólo cuando Discovery esté validado)
kj architect --task-file karajan/prompts/03-architecture.md

# 5) Fase HUs (sólo cuando Architecture esté validada)
kj plan generate --task-file karajan/prompts/04-hus.md

# 6) Ejecución de las HUs (sub-pipeline por HU)
kj run --plan <plan-id>
```

## Notas

- Cada output se revisa **a mano** antes de pasar al siguiente.
- v2.12.0 de Karajan mostrará **plan adherence** en el `summary.md` de cada `kj run`, midiendo cuánto el coder respetó la HU.
- El proyecto KJ se llama `GRETA` (abrev. `GRT`); el de Firebase será `greta-app`.
