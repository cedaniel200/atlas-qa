# atlas-qa

> **Alpha.** Bundle de la suite ATLAS para el ciclo de calidad. El motor `atlasctl` llega en 0.6.0 y el
> enganche de bundles en 0.6.1. Todavía no hay nada instalable.

Este repositorio es **contenido**, no producto: roles, skills y el fragmento de contrato que el motor
aplica sobre un proyecto. No se clona para usarlo.

## Qué contiene

```text
bundle.json          metadatos del bundle: id, dependencias, roles y skills
contract/qa.md       fragmento del contrato base (no es un AGENTS.md propio)
roles/               procedimiento de cada rol exclusivo de este bundle
skills/              técnicas, cargadas por el rol que las necesite
```

`bundle.json` declara `requires: ["core"]`: los roles compartidos (`analyst`, `researcher`, `reviewer`,
`doc-writer`) vienen del bundle `core` y este bundle los **especializa con skills**, no los redefine.
Ventaja: una sola copia de cada rol y ninguna deriva entre productos.

## Roles

| Rol | Prompt | Responsabilidad | Escribe |
|---|---|---|---|
| `test-analyst` | `roles/test-analyst.md` | Estrategia, plan, casos, datos y trazabilidad | `artifacts/analysis/` |
| `test-automation` | `roles/test-automation.md` | Automatizabilidad, arquitectura y código de pruebas | área de pruebas del proyecto |

Los demás roles del ciclo son del `core`. Este bundle les añade técnica:

| Rol del core | Skill que aporta este bundle |
|---|---|
| `doc-writer` | `docs-quality` |
| `reviewer` | `review-test-design` |
| `researcher` | `quality-research` |

## Skill Registry

> Los disparadores pueden estar en cualquier idioma; cada skill declara ejemplos en su `description`.

| Skill | Ruta | Disparador |
|---|---|---|
| `docs-quality` | `skills/docs-quality/` | Consolidar insumos en documentación oficial |
| `review-test-design` | `skills/review-test-design/` | Revisar un diseño de pruebas antes de aprobarlo |
| `quality-research` | `skills/quality-research/` | Verificar un hecho externo que cambia el diseño |
| `performance-testing` | `skills/performance-testing/` | Requisito de latencia, concurrencia o capacidad |

## Ciclo

```text
Humano → test-analyst → [aprobación] → test-automation → [validación] → doc-writer
```

Dos puntos de control humanos. El detalle está en `contract/qa.md`.

## Pendiente para 0.6.1

- **Pack `ai-eval`**: certificación de sistemas RAG y agénticos. Mismo flujo, técnicas distintas:
  conjunto de evaluación, rúbricas, baselines de puntuación y umbrales estadísticos.
- **Idioma**: los prompts del bundle están en español; el arnés declara `language.agent: en` para los
  prompts canónicos. Decidir si se traducen o si el bundle declara su idioma.
- **Templates**: plan, casos, matriz e informe como plantillas enlazadas desde las skills.

## Enlaces

- Motor: <https://github.com/cedaniel200/atlasctl>
- Bundle de desarrollo: <https://github.com/cedaniel200/atlas-dev>
