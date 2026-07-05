# ADR-0002 · Servicio de notificaciones desacoplado, correo electrónico como canal por defecto

**Estado:** aceptado
**Fecha:** 2026-07-05

## Contexto y fuerza

Cinco historias del backlog (`backlog.json`, `stories.md`) disparan una
notificación como parte de su criterio de aceptación:

- **US-01** (encargado programa mantenimiento) → notifica al policía asignado.
- **US-02** (policía autoagenda) → notifica a mecánico y encargado.
- **US-03** (policía cancela/reagenda) → notifica a mecánico y encargado.
- **US-04** (recordatorio 48h) → notifica al policía.
- **US-09** (encargado solicita repuestos) → notifica al gerente; y al
  encargado cuando el gerente decide.
- **US-11** (gerente aprueba/rechaza) → notifica al encargado con el
  resultado.

**R-18** (`requisitos.md`) deja el canal explícitamente abierto: "El sistema
debe enviar notificaciones (por correo electrónico u otro canal digital
configurado)...". `epics.md` registra esto como pregunta abierta a nivel de
backlog, y `stories.md` la resuelve para US-04 fijando "correo electrónico
(canal por defecto del sistema)" como decisión de diseño delegada al equipo de
construcción — es esa resolución la que este ADR formaliza a nivel de
arquitectura.

Además, el riesgo **S-1 · ALTO** (`mvp-canvas.md`: "los policías adoptarán el
autoagendamiento digital en lugar de ignorarlo, igual que ignoraban el papel
con la fecha del próximo mantenimiento") hace que la notificación no sea un
detalle secundario: es el mecanismo por el cual la métrica de éxito del MVP
(tasa de cumplimiento ≥ 70 %) puede alcanzarse sin que el mecánico tenga que
llamar a reclamar.

## Decisión

Implementar las notificaciones como un **módulo interno desacoplado** dentro
del mismo backend (no un microservicio externo, ver `ADR-0003`): los módulos
de Agenda y Repuestos publican un evento simple ("turno creado", "turno
cancelado", "solicitud aprobada/rechazada") y el programador de recordatorios
(`SCHED`) publica el evento de la ventana de 48 horas; el servicio de
notificaciones interpreta esos eventos y decide el canal y el formato del
mensaje. Para el MVP, el **canal por defecto es correo electrónico**, con una
interfaz de canal que permite en el futuro enrutar por otro medio configurado
sin tocar la lógica de agenda, órdenes o repuestos.

## Alternativas consideradas

- **Cada módulo envía sus propias notificaciones (lógica de correo dentro de
  Agenda y dentro de Repuestos)** — descartada: duplica la lógica de envío en
  al menos dos módulos distintos y hace más costoso cumplir R-18 si se decide
  cambiar el canal, porque habría que tocar cada módulo por separado.
- **Construir de una vez soporte multicanal completo (correo + SMS + push)**
  — descartada: sobre-diseño. Ninguna historia exige más de un canal
  funcionando ya; R-18 solo exige que el canal sea "configurado", no que
  existan múltiples canales disponibles en el MVP. Se declara como decisión
  diferida en `architecture.md`.
- **Servicio de notificaciones como microservicio externo, con su propia cola
  de mensajes** — descartada para este tamaño de MVP por la misma razón que
  `ADR-0003` descarta microservicios en general: la complejidad operativa
  adicional (despliegue independiente, comunicación entre servicios) no está
  justificada por 42 pts y un piloto de un centro.

## Consecuencias

- Un solo punto de cambio si la institución pide otro canal por defecto o
  canal por usuario (R-18 ya lo prevé como posible).
- Los módulos de Agenda y Repuestos quedan más simples: no conocen el detalle
  de cómo se envía una notificación, solo que ocurrió un evento relevante.
- El acoplamiento es interno (llamada directa o evento en proceso), no una
  cola de mensajes distribuida: suficiente para el volumen esperado de un
  piloto de un centro; si el volumen crece, se podrá introducir una cola sin
  cambiar la interfaz que los módulos ya usan.
- Queda pendiente, como detalle de implementación (no de arquitectura), cuál
  proveedor de correo (SMTP propio de la institución vs. externo) se usará
  — declarado como supuesto abierto en `architecture.md`.
