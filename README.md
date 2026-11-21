# atlas-qa

> **Alpha.** Bundle de la suite ATLAS para el ciclo de calidad. El motor `atlasctl` llega en 0.6.0 y el
> enganche de bundles en 0.6.1. Todavía no hay nada instalable.

> English version: [README.en.md](README.en.md)

Este repositorio es **contenido**, no producto: roles, skills y un fragmento de contrato que el motor
aplica sobre un proyecto. No se clona para usarlo.

## Estructura

```text
bundle.json          metadatos del bundle: id, dependencias, roles y skills
contract/qa.md       fragmento del contrato base (no es un AGENTS.md propio)
roles/               procedimiento de cada rol exclusivo de este bundle
skills/              técnicas, cargadas por el rol que las necesite
packs/               especializaciones opcionales (ai-eval, pendiente)
```

`bundle.json` declara `requires: ["core"]`. Los roles compartidos (`analyst`, `researcher`, `reviewer`,
`doc-writer`) vienen del bundle `core`, y este bundle los **especializa con skills** en vez de
redefinirlos. Una sola copia de cada rol y ninguna deriva entre productos.

## Qué es un rol, una skill, un pack o un perfil

La clasificación no es cosmética: cada una falla de una manera distinta.

| Tipo | Prueba | Si se usa mal |
|---|---|---|
| **Rol** | Mandato propio, puntos de parada propios, límites de escritura | Dos roles con el mismo mandato divergen |
| **Skill** | Mismo mandato, distinta lista de comprobación | Se convierte en roles que se contradicen |
| **Pack** | Artefactos o modelo de aceptación distintos, mismos gates | Un bundle que en realidad es una técnica |
| **Perfil** | Stack, lenguaje, comandos, rutas | El contenido deja de ser agnóstico |

Aplicado a este bundle:

| Pieza | Tipo | Por qué |
|---|---|---|
| `test-analyst` | rol | Diseña, se detiene para aprobación, nunca automatiza |
| `test-automation` | rol | Escribe código, ejecuta, nunca diseña |
| Revisión del diseño | skill de `reviewer` | Mismo mandato que revisar código, distinta lista |
| Revisión de la automatización | skill de `reviewer` | Ídem |
| `researcher` | rol del `core` + `quality-research` | El rol ya existe; la técnica es local |
| `doc-writer` | rol del `core` + `docs-quality` | Ídem |
| `performance-testing` | skill | Técnica: sin flujo propio ni gates propios |
| `ai-eval` (RAG, agentes) | pack | Artefactos y modelo de aceptación distintos, mismos gates |

## Roles

| Rol | Prompt | Responsabilidad | Escribe en |
|---|---|---|---|
| `test-analyst` | `roles/test-analyst.md` | Estrategia, plan, casos, datos y trazabilidad | `artifacts/analysis/` |
| `test-automation` | `roles/test-automation.md` | Automatizabilidad, arquitectura y código de pruebas | área de pruebas del proyecto |

## Skill Registry

> Los disparadores pueden estar en cualquier idioma; cada skill declara ejemplos en su `description`.

| Skill | Ruta | Disparador |
|---|---|---|
| `docs-quality` | `skills/docs-quality/` | Consolidar insumos en documentación oficial |
| `review-test-design` | `skills/review-test-design/` | Revisar un diseño de pruebas antes de aprobarlo |
| `review-test-automation` | `skills/review-test-automation/` | Revisar la automatización antes de validarla |
| `quality-research` | `skills/quality-research/` | Verificar un hecho externo que cambia el diseño |
| `performance-testing` | `skills/performance-testing/` | Requisito de latencia, concurrencia o capacidad |

## Ciclo

```text
Humano → test-analyst → [aprobación] → test-automation → [validación] → doc-writer
```

Dos puntos de control humanos. El detalle está en `contract/qa.md`.

## Pendiente para 0.6.1

- **`atlas-core`**: el bundle `core` con el contrato base y los seis roles compartidos. Hoy vive dentro de
  `atlas-dev`; se separa cuando el motor ya esté publicado, para no duplicar la migración.
- **Pack `ai-eval`**: certificación de sistemas RAG y agénticos. Mismo flujo, técnicas distintas: conjunto
  de evaluación, rúbricas, baselines de puntuación y umbrales estadísticos. Necesita su propio rol de
  análisis porque el modelo de aceptación no es pasa/no pasa.
- **Templates**: plan, casos, matriz e informe, enlazados desde las skills.

## Enlaces

- Motor: <https://github.com/cedaniel200/atlasctl>
- Bundle de desarrollo: <https://github.com/cedaniel200/atlas-dev>
