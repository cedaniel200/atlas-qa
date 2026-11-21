# Contrato del ciclo de calidad

> Fragmento del contrato base. No es un segundo `AGENTS.md`: se integra en el contrato del proyecto
> cuando el bundle `qa` está activo. Lo que no esté decidido aquí se pregunta; no se inventa.

## 1 · Propósito

Este bundle cubre planificar, diseñar, automatizar y documentar pruebas de software, y cerrar el ciclo
con un veredicto sostenido por evidencia. No cubre evaluación de sistemas de IA: eso es el pack
`ai-eval`.

## 2 · Contexto del proyecto

- El stack, las superficies del sistema y los entornos se leen del contexto del proyecto y del perfil
  activo. **No se infieren.**
- Si el contexto no declara una superficie, un entorno o un comando, se registra como bloqueo y se
  pregunta; no se completa con una suposición plausible.
- Los comandos de build, test y lint son los que declare el perfil. No se inventan comandos.

## 3 · Flujo

```text
Humano (insumos) → test-analyst → [APROBACIÓN] → test-automation → [VALIDACIÓN] → doc-writer
```

1. **Entrada humana:** requisitos, historias, criterios de aceptación, contratos, diagramas, accesos y
   restricciones. Ningún agente rellena esos vacíos por su cuenta.
2. El `test-analyst` revisa los insumos, formula preguntas y diseña. No automatiza ni documenta.
3. **Punto de control obligatorio:** el humano aprueba el paquete de análisis. Sin esa aprobación el
   flujo se detiene; el `test-automation` no inicia nada.
4. El `test-automation` consume solo casos `Aprobado` y devuelve resultados con evidencia.
5. **Punto de control obligatorio:** el humano valida resultados y evidencias antes de consolidar.
6. El `doc-writer` consolida lo aprobado en la documentación oficial.
7. Si falta información, el agente registra el bloqueo y formula preguntas concretas.
8. Un cambio en requisitos obliga a revisar casos, automatización y documentos; el flujo se reanuda en
   el paso 3.

## 4 · Traspaso al doc-writer

- **La aprobación dispara la consolidación.** No se depende de que alguien se acuerde de invocarlo.
- Todo insumo que espera consolidación declara versión, fuentes y preguntas abiertas. Sin eso no hay
  traspaso.
- Un documento oficial marcado como `Consolidado` indica de qué versión del insumo proviene. Si el
  insumo cambia, la marca desaparece.
- El insumo no describe su destino: describe su contenido. El estado y la versión bastan.

## 5 · Artefactos e identificadores

| Tipo | Patrón | Quién lo asigna |
|---|---|---|
| Requisito | `REQ-###` | `test-analyst` |
| Caso de prueba | `TC-###` | `test-analyst` |
| Dato de referencia | `DATA-###` | `test-analyst` |
| Ejecución | `RUN-YYYYMMDD-##` | `test-automation` |
| Hallazgo | `BUG-###` | `test-automation` |

- Estado de artefacto: `Borrador`, `En revisión`, `Aprobado`, `No aplicable`, `Obsoleto`.
  **Solo el humano marca `Aprobado`.**
- Resultado de ejecución: `Éxito`, `Fallo`, `Bloqueado`, `Omitido`, `No ejecutado`.
- Todo caso enlaza al menos un requisito identificable. Si no existe identificador, se propone uno
  provisional y se marca como tal.
- La matriz recorre requisito → dato → caso → ejecución/evidencia → hallazgo cuando esos datos existan.

## 6 · Datos de referencia

- El caso describe comportamiento y cita el dato por su identificador. **No cita el valor.**
- El `DATA-###` declara una **regla de selección** y, aparte, los valores vigentes con su fecha.
- El valor concreto se escribe solo cuando el valor **es la condición que se prueba**.
- Los datos de prueba son controlados y aislados. Nunca datos personales o productivos sin
  autorización.

## 7 · Evidencia y veredicto

- Resultado esperado y resultado obtenido van separados. Sin evidencia no hay `Éxito`.
- Ningún informe se cierra con hallazgos bloqueantes abiertos.
- Un veredicto declara aprobación, aprobación con sugerencias o no aprobación, y se sostiene en la
  tabla de hallazgos: si hay un bloqueante abierto, el veredicto no puede ser favorable.
- No se declara certificación ni cumplimiento por ausencia de defectos.

## 8 · Autonomía

| Rol | Puede hacer sin preguntar | Requiere aprobación humana |
|---|---|---|
| `test-analyst` | Leer requisitos, identificar vacíos, proponer estrategia, plan, casos y matriz en borrador | Aprobar alcance, resolver ambigüedades de negocio, cambiar requisitos |
| `test-automation` | Inspeccionar el repositorio, proponer arquitectura, crear pruebas en el área autorizada y ejecutar pruebas no destructivas en entorno autorizado | Modificar código de producción, cambiar infraestructura compartida, usar datos reales, ejecutar pruebas destructivas o contra producción |
| `doc-writer` (skill `docs-quality`) | Leer insumos y actualizar borradores sin cambiar el significado | Aprobar certificaciones, declarar cumplimiento, descartar hallazgos |

- Si el permiso del entorno no está claro, el agente se detiene y pide autorización.
- Ningún agente aprueba sus propios entregables.

## 9 · Guardas de operación

Estas guardas no se duplican aquí: se heredan del contrato del proyecto y del motor.

- **Rama y commits:** el trabajo va en una rama, nunca directamente sobre la base; un cambio lógico es
  un commit, con prefijo convencional.
- **Secretos:** no se versionan claves, tokens, credenciales ni datos reales.
- **Verificación:** ninguna librería, API, comando o versión se afirma de memoria; se verifica en la
  documentación vigente o se declara como no verificado.
- **Memoria persistente:** si el proyecto la tiene configurada, se consulta al empezar y se escribe al
  decidir. Complementa los documentos; no los sustituye.
- **No se inventa nada:** sin evidencia en requisitos, contratos, código o documentación no se crean
  endpoints, selectores, comandos, métricas ni resultados.

## 10 · Precedencia

1. La especificación aprobada del producto y sus criterios de aceptación son la fuente de verdad
   funcional.
2. Este contrato define el ciclo de calidad.
3. Las skills definen la técnica; los roles, el procedimiento.
4. Ante contradicción, no se elige en silencio: se registra el conflicto y se pide decisión.
