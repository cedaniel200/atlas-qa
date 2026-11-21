---
name: quality-research
description: Use before designing or approving tests when a claim depends on an external fact — tool behaviour, API limits, standard versions, framework features. Trigger: "¿soporta esto?", "¿cómo funciona X?", "busca cómo se hace", "verifica la versión".
---

# Investigación de calidad

## Overview

Responde con fuentes lo que el diseño da por sabido. Una afirmación que cambia el diseño sin fuente es
un supuesto, y un supuesto se marca como tal.

## When to Use

- Antes de fijar una estrategia o una técnica que depende de cómo funciona una herramienta.
- Antes de prometer cobertura, límites o compatibilidad de versiones.
- Cuando dos documentos del proyecto se contradicen.

No usar para hechos del propio repositorio: eso se lee en el código y en el contrato.

## Barra de evidencia

| Nivel | Qué vale | Qué no vale |
|---|---|---|
| **Verificado** | Documentación oficial de la versión usada, o un experimento reproducible en el proyecto | Un blog, un foro, la memoria del modelo |
| **Supuesto** | Razonable pero no comprobado | Presentarlo como hecho |
| **Bloqueo** | No se puede verificar | Rellenar con lo plausible |

Toda afirmación que cambie el diseño cita fuente o experimento. Si no puede, se marca como supuesto y se
declara qué lo confirmaría.

## Procedimiento

1. **Formular la pregunta** en una frase, acotada a un hecho.
2. **Buscar primero en el proyecto:** contrato, contexto, código, configuración, historial.
3. **Verificar fuera** cuando el hecho sea externo: documentación de la versión exacta que se usa.
4. **Registrar:** qué se preguntó, qué se encontró, qué cambió en el diseño y qué sigue abierto.
5. **Acotar la salida:** máximo diez hallazgos, cada uno con su fuente y su consecuencia práctica.

## Reglas

1. No investigues lo que ya está decidido en el contrato del proyecto; sería ruido.
2. Distingue la versión instalada de la última publicada. Un comportamiento de la última no explica la
   actual.
3. Si la documentación contradice la observación, gana la observación y se registra la discrepancia.
4. Si no hay preguntas abiertas, dilo en una línea. No produzcas una investigación vacía.
5. Sin cifras de costo por hallazgo: el límite es el de la utilidad, no el de la exhaustividad.

## Formato de salida

- **Preguntas:** una línea cada una, con su estado (`resuelta`, `supuesto`, `bloqueada`).
- **Hallazgos:** hecho, fuente, efecto sobre el diseño.
- **Descartes:** qué se comprobó y no cambió nada, en una línea por descarte.
