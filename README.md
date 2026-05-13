# greta-app

App SaaS de **GRETA** — herramienta privada para liderazgos de ingeniería que creen que las personas son la estrategia. Diagnostica composición de equipo, mide impacto real (no outputs), procesa transcripciones de 1:1 con IA y construye una red de liderazgo afectivo entre TLs, EMs y CTOs.

> El **framework GRETA** (metodología, escala, dimensiones) vive en un repositorio aparte: `github.com/manufosela/greta`. Aquí está la **implementación** del framework como producto. La copia incluida en `docs/GRETA-v1.0-RC3.md` debe sincronizarse a mano cuando el framework cambie.

---

## Estructura del repo

```
greta-app/
├── README.md
├── .gitignore
└── docs/
    ├── GRETA-app.task.md        # Documento maestro: framework + requisitos + lo que Karajan debe construir
    └── GRETA-v1.0-RC3.md        # Copia del framework GRETA (la referencia metodológica)
```

Toda la documentación del proyecto vive en `docs/`. Sin más carpetas: ni `tasks/`, ni `karajan/`, ni outputs intermedios. Lo que Karajan necesite generar lo deja fuera del repo (`~/.kj/`, `.karajan/`, `.claude/`, etc., todos en `.gitignore`).

Cuando empiece el desarrollo del producto aparecerán `src/`, `functions/`, etc. — solo lo necesario del producto, no de las herramientas.

## Generar el plan con Karajan

```bash
cd ~/ws_firebase/greta-app
kj plan generate --task-file docs/GRETA-app.task.md
```

El plan generado vive en `~/.kj/plans/home_manu_ws_firebase_greta-app/` (fuera del repo). El board lo muestra en `http://localhost:4000` tras `kj board start`.

## Ejecutar las HUs del plan

```bash
kj run --plan <plan-id>          # ejecuta todas las HUs del plan
kj run --plan <plan-id> --hu <hu-id>   # una HU concreta
```

Ambos comandos abren un sub-pipeline por HU: coder → reviewer → tester → done/failed. El estado se ve en el board.

## Por qué este repo no tiene carpetas de Karajan

Karajan es una herramienta transparente: lee desde `docs/`, escribe sus artefactos en `~/.kj/` y `.karajan/` (ignorados), y deja el repo limpio para que solo contenga lo del producto. Las reglas locales del equipo viven en `.karajan/coder-rules.md` y `.karajan/review-rules.md`, fuera del control de git.
