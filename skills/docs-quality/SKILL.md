---
name: docs-quality
description: Use when consolidating QA artifacts into official documents — keeps test strategy, plan, cases, traceability matrix, certification report and findings log consistent, versioned and traceable to approved inputs. Trigger: "documenta las pruebas", "actualiza la matriz", "cierra el informe".
---

# Documentación de calidad

## Overview

Convierte insumos aprobados en documentación oficial breve, consistente y rastreable. No diseña
pruebas: consolida lo que ya fue decidido.

## When to Use

- Al cerrar la fase de diseño, cuando el paquete de análisis queda `Aprobado`.
- Cuando el `test-automation` entrega resultados y evidencias.
- Cuando un requisito cambia y hay que reflejar el impacto documental.

No usar para diseñar casos, definir umbrales ni escribir código.

## Documentos

| Documento | Contenido |
|---|---|
| `docs/estrategia-pruebas.md` | Objetivo, alcance, niveles, tipos, enfoque, riesgos, métricas, criterios de entrada y salida |
| `docs/plan-pruebas.md` | Actividades, roles, dependencias, recursos, calendario disponible, riesgos, entregables |
| `docs/casos-prueba.md` | Catálogo con estado, identificador, requisito, prioridad, precondiciones, datos, pasos y resultado esperado |
| `docs/matriz-trazabilidad.md` | Requisito → criterio → dato → caso → ejecución/evidencia → hallazgo |
| `docs/informe-certificacion.md` | Alcance, versión, ambiente, criterios de salida, resumen, defectos abiertos, excepciones, evidencias y decisión |
| `docs/bitacora-hallazgos.md` | Registro cronológico de defectos, bloqueos, discrepancias y decisiones |

Adapta los nombres a la estructura existente del repositorio; no dupliques documentos.

## Convenciones

1. Consolida solo insumos aprobados. No completes vacíos por inferencia.
2. Si dos insumos se contradicen, conserva la discrepancia como pendiente y pide resolución.
3. Distingue estado del artefacto (`Borrador`, `En revisión`, `Aprobado`, `No aplicable`, `Obsoleto`) y
   resultado de ejecución (`Éxito`, `Fallo`, `Bloqueado`, `Omitido`, `No ejecutado`).
4. Conserva los identificadores estables y no los reasignes.
5. Registra procedencia y versión de cada insumo utilizado.
6. Actualiza solo los documentos afectados; no reescribas el conjunto.
7. Marca cada documento consolidado con la versión del insumo del que proviene. Si el insumo cambia, la
   marca desaparece.
8. No declares certificación sin criterios de salida y evidencia suficiente.
9. No elimines hallazgos abiertos ni cambies estados sin evidencia o decisión explícita.
10. Separa hechos de decisión: el informe presenta resultados y evidencia antes que la conclusión.

## Formato de salida

- **Resumen de actualización:** documentos afectados y motivo.
- **Documentos:** contenido Markdown o cambios aplicados al repositorio.
- **Control de consistencia:** identificadores duplicados, vínculos rotos, requisitos sin casos, casos
  huérfanos, referencias faltantes.
- **Pendientes:** conflicto, documento afectado, impacto y responsable sugerido.
- **Procedencia:** versiones y fechas de los insumos.

## Brevedad

- Estrategia y plan: resumen ejecutivo y tablas; amplía solo riesgos o decisiones relevantes.
- Casos: pasos suficientes para reproducir; no copies contexto de la historia.
- Matriz: una fila por relación; sin párrafos.
- Certificación: resultados y evidencia primero.
- Bitácora: una entrada por evento; no borres entradas cerradas.

## Criterios de éxito

- Cada afirmación se rastrea a un insumo o evidencia identificable.
- Identificadores, estados, versiones y referencias cruzadas son consistentes.
- Las discrepancias y vacíos permanecen visibles hasta resolverse.
- No se diseñaron pruebas ni se modificó código.
