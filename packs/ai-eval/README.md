# ai-eval

> **Alpha — principios, no implementación.** Este directorio todavía no es un pack: no tiene
> `pack.json`, roles ni skills. Contiene las reglas que cualquier implementación futura deberá respetar.

> English version: [README.en.md](README.en.md)

## Para qué

Certificación de sistemas que usan modelos: RAG y aplicaciones agénticas. La pregunta no es «¿funciona?»
sino «¿puedo demostrar, con evidencia y umbrales, que cumple lo que promete?».

## Principios

1. **No determinismo.** Una ejecución no demuestra nada. Se mide una distribución, se repite y se declara
   la variabilidad. Un resultado aislado es una anécdota con formato de informe.
2. **El conjunto de evaluación no es un caso de prueba.** Es un dataset con regla de selección, versión y
   fecha. Se cita por identificador, igual que un `DATA`, y nunca se incrusta el valor en el caso.
3. **Umbrales aprobados antes de ejecutar.** Un umbral elegido después de ver el número no es un criterio,
   es una racionalización.
4. **Recuperación y generación se miden por separado.** ¿Trajo los documentos correctos? ¿Respondió fiel a
   lo que trajo? Mezclarlas oculta cuál de las dos falla.
5. **Un modelo como juez exige calibración.** Sin comparación contra etiquetas humanas y una métrica de
   acuerdo entre jueces, el juicio automático es una opinión con formato.
6. **En sistemas agénticos se evalúa la trayectoria, no solo la respuesta final.** Herramientas elegidas,
   argumentos, pasos de más, abandono de la tarea.
7. **Reproducibilidad.** Versión de modelo, prompt, temperatura y dataset fijados y registrados. Sin eso no
   hay comparación entre ejecuciones, solo recuerdos.

## Lo que falta decidir

- El rol de análisis: se intuye propio (`eval-analyst`), porque la aceptación no es pasa/no pasa.
- Los artefactos: conjunto de evaluación, rúbrica, baseline de puntuación.
- Los umbrales por familia: RAG frente a agénticos.

Se decidirá con el criterio real de quien certifica, no por imitación.

## Enlaces

- Bundle de calidad: <https://github.com/cedaniel200/atlas-qa>
- Motor: <https://github.com/cedaniel200/atlasctl>
