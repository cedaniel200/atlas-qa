---
name: performance-testing
description: Use when a requirement depends on response time, throughput, concurrency or capacity — defines the load model, the baseline and the acceptance thresholds before any run. Trigger: "prueba de carga", "rendimiento", "cuántos usuarios soporta", "latencia", "estrés", "capacidad".
---

# Pruebas de rendimiento

## Overview

Mide el comportamiento bajo una carga declarada y lo compara contra un umbral aprobado. Sin modelo de
carga y sin umbral acordado, una prueba de rendimiento produce números, no evidencia.

## When to Use

- Cuando un requisito fija tiempos de respuesta, concurrencia o capacidad.
- Antes de un lanzamiento, para establecer una línea base.
- Cuando se sospecha una regresión de rendimiento entre versiones.

No usar para medir el rendimiento de la suite de pruebas ni para comparar herramientas.

## Antes de ejecutar

| Elemento | Sin él |
|---|---|
| **Modelo de carga** | No se sabe qué se está midiendo: usuarios concurrentes, ritmo de llegada, duración, perfil de mezcla |
| **Ambiente declarado** | Los datos no son comparables entre ejecuciones |
| **Umbral** | No hay criterio de aprobación; el número no significa nada |
| **Línea base** | No hay contra qué comparar |
| **Datos controlados** | El resultado depende de qué había en la base |

Si falta uno, la prueba no se ejecuta: se registra como bloqueo.

## Convenciones

1. **Umbrales aprobados, no inventados.** Se proponen con justificación —requisito, acuerdo de servicio,
   línea base previa— y el humano los aprueba antes de la ejecución.
2. **Una variable por ejecución.** Si se cambia la carga y el ambiente a la vez, el resultado no se
   atribuye a nada.
3. **Mide el percentil, no la media.** La media esconde la cola, y la cola es lo que el usuario siente.
   Declara qué percentil se usa y por qué.
4. **No destructivo o autorizado.** Carga, estrés y capacidad se ejecutan solo en el ambiente declarado
   y con autorización explícita.
5. **Datos aislados y reproducibles.** El estado del ambiente se registra antes y después.
6. **Interferencia declarada.** Si otras pruebas corren en el mismo ambiente, se anota y no se compara.
7. **Una regresión no se declara con una sola ejecución.** Se repite y se busca la causa antes de
   abrir un hallazgo.
8. **El resultado es evidencia, no una promesa.** Se adjuntan condiciones, configuración y artefactos.

## Formato de salida

- **Modelo de carga:** usuarios, ritmo, duración, mezcla y datos.
- **Umbral acordado:** métrica, percentil, valor y quién lo aprobó.
- **Ejecuciones:** `RUN-YYYYMMDD-## | Escenario | Carga | Ambiente | Métrica | Valor | Resultado | Evidencia`.
- **Comparación** contra la línea base, cuando exista.
- **Hallazgos:** `BUG-###` con la condición que los reproduce.

## Relación con el ciclo

- El `test-analyst` define modelo de carga, umbral y casos; sin su aprobación no hay ejecución.
- El `test-automation` ejecuta y entrega resultados con evidencia.
- Un rendimiento fuera de umbral es un hallazgo abierto: bloquea el veredicto favorable.
