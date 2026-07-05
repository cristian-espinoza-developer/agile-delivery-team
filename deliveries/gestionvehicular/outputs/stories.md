# Historias refinadas — Gestión Vehicular Policial (MVP)

Refinamiento del Developer sobre el backlog del PO (`backlog.json`). Cada
historia se revisó contra INVEST: criterios de aceptación en Gherkin
verificables, estimación en puntos, tamaño ≤ 8 pts, y dependencias resueltas
contra IDs existentes. Ninguna historia queda con `open_questions` pendientes:
las dos que llegaron abiertas (US-04 y US-10) se resolvieron con decisiones de
alcance trazadas al `inbox/` (sin inventar hechos nuevos) y se documentan al
final de este archivo.

Agrupadas por épica, en orden de prioridad: E-01, E-02, E-03, E-04.

---

## E-01 · Agendamiento digital de mantenimientos

### US-01 · Programar mantenimiento desde el encargado   ·   épica E-01   ·   5 pts
**Como** encargado, **quiero** programar un mantenimiento para un vehículo específico (con fecha, hora y tipo de servicio) en el sistema, **para** que el mecánico y el policía estén al tanto sin que yo los llame al celular.

Criterios de aceptación (Gherkin):
- Dado que el encargado crea un nuevo mantenimiento programado con todos los campos requeridos, cuando lo guarda, entonces aparece en la agenda del mecánico y se envía una notificación al policía asignado a ese vehículo.
- Dado que ya existe un mantenimiento en esa franja horaria para el mismo mecánico, cuando el encargado intenta crear otro en el mismo horario, entonces el sistema advierte el conflicto antes de guardar.

Origen: `us:US-01`, `req:R-03`, `req:R-04`, `persona:encargado#agendamiento-celular-personal`, `persona:mecanico#sin-agenda-mantenimientos`

---

### US-02 · Autoagendar turno de mantenimiento   ·   épica E-01   ·   5 pts
**Como** policía, **quiero** agendar mi propio turno de mantenimiento eligiendo una fecha y hora disponible en el sistema, **para** no tener que llamar al mecánico ni ir sin cita.

Criterios de aceptación (Gherkin):
- Dado que el policía accede al sistema con su vehículo asignado, cuando selecciona una franja disponible y confirma, entonces el turno queda registrado y el mecánico y el encargado reciben una notificación.
- Dado que el policía intenta agendar en una franja ya ocupada, cuando intenta confirmar, entonces el sistema muestra las próximas franjas disponibles en lugar de bloquear sin orientación.

Origen: `us:US-02`, `req:R-04`, `persona:policia#sin-agendamiento-policia`

---

### US-03 · Cancelar o reagendar un turno   ·   épica E-01   ·   3 pts
**Como** policía, **quiero** cancelar o reagendar un turno de mantenimiento directamente en el sistema, **para** avisar con anticipación sin tener que llamar.

Criterios de aceptación (Gherkin):
- Dado que el policía tiene un turno programado, cuando lo cancela o selecciona una nueva fecha, entonces el mecánico y el encargado reciben inmediatamente una notificación con el motivo (si se indicó).
- Dado que el turno ya pasó sin asistencia, cuando el sistema lo detecta, entonces lo marca como "no asistido" y notifica al encargado para seguimiento.

Dependencias: US-01, US-02

Origen: `us:US-03`, `req:R-04`, `persona:mecanico#sin-cancelacion-turnos`

---

### US-04 · Recordatorio automático 48h antes   ·   épica E-01   ·   3 pts
**Como** policía, **quiero** recibir un recordatorio automático al menos 48 horas antes de mi mantenimiento programado, **para** no olvidarme de ir.

Criterios de aceptación (Gherkin):
- Dado que existe un mantenimiento programado dentro de las próximas 48 horas, cuando el sistema lo detecta en su ciclo de notificaciones, entonces envía un aviso al policía por correo electrónico (canal por defecto del sistema) o, si el policía tiene configurado otro canal digital, por ese canal configurado, conforme a R-18.
- Dado que el policía ya canceló el turno, cuando el sistema evalúa los recordatorios pendientes, entonces no envía el aviso para ese turno cancelado.

Dependencias: US-01, US-02

Origen: `us:US-04`, `req:R-05`, `req:R-18`, `persona:policia#sin-recordatorios-mantenimiento`

---

### US-05 · Ver agenda del día   ·   épica E-01   ·   2 pts
**Como** mecánico, **quiero** ver la agenda del día con todos los mantenimientos asignados, **para** saber qué vehículos me esperan y prepararme.

Criterios de aceptación (Gherkin):
- Dado que el mecánico abre el sistema al inicio del turno, cuando accede a la vista del día, entonces ve los mantenimientos ordenados por hora con el tipo de servicio, identificador del vehículo y nombre del policía.
- Dado que un turno fue cancelado, cuando el mecánico consulta la agenda, entonces el turno aparece marcado como cancelado (no desaparece silenciosamente).

Dependencias: US-01, US-02

Origen: `us:US-05`, `req:R-03`, `persona:mecanico#sin-agenda-mantenimientos`

---

## E-02 · Registro de órdenes de trabajo e historial vehicular

### US-06 · Registrar orden de trabajo digital   ·   épica E-02   ·   5 pts
**Como** mecánico, **quiero** registrar digitalmente una orden de trabajo (vehículo, trabajos realizados, materiales usados y costos), **para** evitar olvidos y que el sistema calcule el total sin errores de suma.

Criterios de aceptación (Gherkin):
- Dado que el mecánico completa una intervención, cuando ingresa los ítems de material y sus costos en la orden, entonces el sistema calcula y muestra el costo total automáticamente antes de que el mecánico confirme el cierre.
- Dado que el mecánico cierra la orden, cuando la guarda, entonces queda vinculada al vehículo y aparece en su historial con fecha, tipo de trabajo, materiales y costo total.

Origen: `us:US-06`, `req:R-01`, `persona:mecanico#registro-manual-trabajos`

---

### US-07 · Consultar historial antes de intervenir   ·   épica E-02   ·   3 pts
**Como** mecánico, **quiero** consultar el historial de mantenimientos del vehículo que voy a atender, **para** conocer el kilometraje anterior y los trabajos ya realizados antes de comenzar.

Criterios de aceptación (Gherkin):
- Dado que el mecánico abre una orden de trabajo para un vehículo, cuando accede al historial de ese vehículo, entonces ve todas las intervenciones previas ordenadas cronológicamente con tipo de trabajo, materiales y costo.
- Dado que el vehículo no tiene intervenciones anteriores en el sistema, cuando el mecánico consulta el historial, entonces el sistema muestra un mensaje explícito de "sin historial registrado" en lugar de una pantalla vacía.

Dependencias: US-06

Origen: `us:US-07`, `req:R-02`, `persona:mecanico#sin-historial-vehiculo`

---

### US-08 · Consultar historial del vehículo asignado   ·   épica E-02   ·   3 pts
**Como** policía, **quiero** consultar el historial de mantenimientos del vehículo que me asignan, **para** saber si tiene problemas recurrentes antes de aceptarlo.

Criterios de aceptación (Gherkin):
- Dado que un vehículo es asignado al policía, cuando el policía accede con el identificador del vehículo, entonces ve el listado de mantenimientos previos con fechas, tipos de trabajo y costos.
- Dado que el policía consulta el historial, cuando hay mantenimientos realizados fuera de fecha (atrasados), entonces estos aparecen destacados para que el policía pueda identificar si el vehículo tuvo problemas de seguimiento.

Dependencias: US-06

Origen: `us:US-08`, `req:R-02`, `req:R-06`, `persona:policia#sin-historial-visible-policia`

---

## E-03 · Solicitud y aprobación digital de repuestos

### US-09 · Crear solicitud de repuestos   ·   épica E-03   ·   5 pts
**Como** encargado, **quiero** crear una solicitud de repuestos en el sistema, **para** que el gerente la vea en su panel y no se pierda en la bandeja de correo.

Criterios de aceptación (Gherkin):
- Dado que el encargado crea una solicitud con lista de ítems y cantidades, cuando la envía, entonces aparece en el panel del gerente como pendiente de aprobación y el gerente recibe una notificación.
- Dado que el gerente aprueba o rechaza la solicitud, cuando toma la decisión (con motivo opcional en caso de rechazo), entonces el encargado recibe la notificación en el sistema con el resultado; no requiere llamada ni nuevo correo.

Origen: `us:US-09`, `req:R-09`, `persona:encargado#solicitud-repuestos-sin-respuesta`

---

## E-04 · Panel gerencial de visibilidad y control

> Nota de refinamiento: la historia original US-10 del PO cubría tres
> responsabilidades distintas (interfaz móvil del panel, aprobar/rechazar
> solicitudes, y ver estado + costos por centro) y ya estaba en el tope de 8
> puntos. Para cumplir la S de INVEST (Small) se dividió en dos historias
> independientemente entregables: **US-10** (ver el panel: estado de
> mantenimientos, filtro por centro, acumulado de costos y bandeja de
> solicitudes pendientes) y **US-11** (aprobar/rechazar una solicitud desde el
> panel). Ambas conservan la trazabilidad de origen de la US-10 del PO.

### US-10 · Ver panel de estado de mantenimientos y costos por centro   ·   épica E-04   ·   5 pts
**Como** gerente, **quiero** acceder desde mi celular a un panel que muestre el estado de mantenimientos de mis centros (filtrable por centro, con el acumulado de costos del período) y las solicitudes de repuestos pendientes de aprobación, **para** no depender de Excel ni del correo para saber qué está pasando en mis centros.

Criterios de aceptación (Gherkin):
- Dado que el gerente accede al panel desde un smartphone, cuando navega por el estado de mantenimientos y las solicitudes pendientes, entonces la interfaz es legible y operable sin zoom ni descarga de archivos Excel.
- Dado que el gerente filtra por uno de sus centros, cuando aplica el filtro, entonces ve únicamente los mantenimientos de ese centro con su estado (programado / en curso / completado / no asistido) y una sumatoria simple del acumulado de costos del período filtrado, mostrada en la misma vista (no es un reporte generado, exportable ni programado; los "reportes avanzados automáticos" de R-14 quedan fuera de este alcance y se habilitan en la iteración 2, según `mvp-canvas.md`).
- Dado que existen solicitudes de repuestos pendientes de aprobación, cuando el gerente abre el panel, entonces las ve listadas junto con el estado de mantenimientos, sin necesidad de abrir el correo.

Dependencias: US-09

Origen: `us:US-10`, `req:R-10`, `req:R-15`, `req:R-17`, `persona:gerente#falta-panel-aprobaciones`, `persona:gerente#reportes-excel-celular`, `persona:gerente#sin-visibilidad-mantenimientos-gerente`

---

### US-11 · Aprobar o rechazar solicitudes desde el panel   ·   épica E-04   ·   3 pts
**Como** gerente, **quiero** aprobar o rechazar una solicitud de repuestos pendiente directamente desde el panel, **para** que el encargado reciba la decisión en el sistema sin que yo tenga que llamarlo ni escribir un correo.

Criterios de aceptación (Gherkin):
- Dado que el gerente selecciona una solicitud de repuestos pendiente en el panel, cuando la aprueba o la rechaza (con motivo opcional en caso de rechazo), entonces la decisión se registra y el encargado del centro recibe la notificación en el sistema.
- Dado que una solicitud ya fue aprobada o rechazada, cuando el gerente vuelve a consultar el panel, entonces la solicitud aparece con su estado final y ya no figura en la bandeja de pendientes.

Dependencias: US-09, US-10

Origen: `us:US-10`, `req:R-10`, `req:R-15`, `persona:gerente#falta-panel-aprobaciones`

---

## Resolución de preguntas abiertas heredadas del PO

- **US-04 — canal del recordatorio.** `inbox/requisitos.md` (R-18) ya autoriza
  explícitamente "correo electrónico u otro canal digital configurado": no es
  una laguna de descubrimiento sino una decisión de diseño delegada al equipo
  de construcción. Se resuelve fijando correo electrónico como canal por
  defecto, con la posibilidad de usar el canal configurado del sistema para
  ese usuario. `open_questions` queda vacío.
- **US-10 — alcance de "acumulado de costos del período".** `inbox/mvp-canvas.md`
  declara "Reportes avanzados automáticos" (R-14) fuera de alcance del MVP
  (se habilita en iteración 2). Se acota el criterio de aceptación para que el
  "acumulado" sea una sumatoria simple mostrada en la misma vista del panel —
  no un reporte generado, exportable o programado — quedando inequívocamente
  distinto de R-14. `open_questions` queda vacío. Adicionalmente, al revisar
  el tamaño de la historia (8 pts, 3 responsabilidades) se dividió en US-10 y
  US-11 (ver nota de refinamiento arriba).

---

## Resumen de estimación

| Épica | Historias | Puntos |
|---|---|---|
| E-01 | US-01, US-02, US-03, US-04, US-05 | 5+5+3+3+2 = 18 |
| E-02 | US-06, US-07, US-08 | 5+3+3 = 11 |
| E-03 | US-09 | 5 |
| E-04 | US-10, US-11 | 5+3 = 8 |
| **Total** | **11 historias** | **42 pts** |

Todas las historias cumplen Definition of Ready: criterios de aceptación en
Gherkin verificables, estimación entera > 0 y ≤ 8 pts, dependencias apuntando
a IDs existentes del propio backlog, y sin preguntas abiertas pendientes.
