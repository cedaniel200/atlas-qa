---
name: review-test-design
description: Use when reviewing a test design before it is approved — checks traceability, oracle quality, data coupling, technique fit, feasibility and evidence. Trigger: "revisa el diseño de pruebas", "revisa los casos", "¿esto está listo para aprobar?".
---

# Revisión de un diseño de pruebas

## Overview

Revisa el paquete de análisis como lo haría un par antes de que el humano lo apruebe. No rediseña: dice
qué está roto, con qué severidad y por qué.

## When to Use

- Cuando el `test-analyst` declara el paquete listo y antes de la aprobación humana.
- Cuando cambia un requisito y hay que revisar el impacto sobre casos y matriz.

No usar para revisar código de automatización ni documentación consolidada.

## Qué se revisa

| Dimensión | Pregunta |
|---|---|
| **Trazabilidad** | ¿Todo caso enlaza un requisito y todo requisito tiene al menos un caso o una razón declarada? |
| **Oráculo** | ¿El resultado esperado es observable y verificable, o dice «funciona correctamente»? |
| **Datos** | ¿El caso cita el valor en lugar del identificador del dato? ¿La regla de selección está declarada? |
| **Técnica** | ¿La técnica elegida corresponde al tipo de riesgo, o se aplican todas por inercia? |
| **Cobertura** | ¿Hay requisitos sin cubrir o casos huérfanos? ¿Se afirma cobertura sin evidencia? |
| **Viabilidad** | ¿El caso se puede ejecutar con los entornos y datos que existen? |
| **Umbrales** | ¿Los criterios de salida están justificados o son cifras inventadas? |
| **Evidencia** | ¿Se distinguen hechos de supuestos? ¿Hay afirmaciones sin fuente? |
| **Duplicación** | ¿Se valida lo mismo en varias capas sin justificación? |

## Severidad

- **Bloqueante:** impide aprobar. Sin requisito, oráculo no verificable, dato incrustado, supuesto
  convertido en criterio.
- **Importante:** afecta cobertura o mantenibilidad. Técnica mal elegida, duplicación, umbral sin
  justificar.
- **Sugerencia:** mejora la claridad sin cambiar el resultado.

## Formato de salida

Tabla de resumen, obligatoria y al principio:

| # | Hallazgo | Severidad | Estado | Acción propuesta |
|---|---|---|---|---|

- `Estado`: `abierto`, `cerrado` o `aceptado` (con motivo).
- Cierra con un veredicto que se derive de la tabla: si hay un bloqueante abierto, no hay aprobación.
- No reescribas los casos: describe el defecto y dónde está.

## Reglas

1. Una pasada completa, no una por sección.
2. No propongas un diseño alternativo salvo que el defecto lo exija; entonces, una sola alternativa.
3. No cierres un hallazgo propio sin evidencia de que se corrigió.
4. Si el paquete no tiene bloqueantes, dilo explícitamente y pasa al humano.
