# User Stories — Gestión Vehicular Policial (MVP)

Cobertura MVP: agendamiento digital de mantenimientos · registro de órdenes de
trabajo · historial por vehículo · solicitud/aprobación de repuestos · panel
del gerente mobile-friendly.

Requisitos cubiertos: R-01, R-02, R-03, R-04, R-05, R-06, R-09, R-10, R-15, R-17, R-18 (parcial).

---

## Agendamiento de mantenimientos

- **[US-01]** Como **encargado**, quiero programar un mantenimiento para un vehículo
  específico (con fecha, hora y tipo de servicio) en el sistema para que el
  mecánico y el policía estén al tanto sin que yo los llame al celular.
  - Criterios de aceptación:
    - Dado que el encargado crea un nuevo mantenimiento programado con todos los
      campos requeridos, cuando lo guarda, entonces aparece en la agenda del mecánico
      y se envía una notificación al policía asignado a ese vehículo.
    - Dado que ya existe un mantenimiento en esa franja horaria para el mismo
      mecánico, cuando el encargado intenta crear otro en el mismo horario, entonces
      el sistema advierte el conflicto antes de guardar.
  - Fuente: `encargado.md`, `mecanico.md`

- **[US-02]** Como **policía**, quiero agendar mi propio turno de mantenimiento
  eligiendo una fecha y hora disponible en el sistema para no tener que llamar al
  mecánico ni ir sin cita.
  - Criterios de aceptación:
    - Dado que el policía accede al sistema con su vehículo asignado, cuando
      selecciona una franja disponible y confirma, entonces el turno queda registrado
      y el mecánico y el encargado reciben una notificación.
    - Dado que el policía intenta agendar en una franja ya ocupada, cuando intenta
      confirmar, entonces el sistema muestra las próximas franjas disponibles en lugar
      de bloquear sin orientación.
  - Fuente: `policia.md`, `mecanico.md`

- **[US-03]** Como **policía**, quiero cancelar o reagendar un turno de mantenimiento
  directamente en el sistema para avisar con anticipación sin tener que llamar.
  - Criterios de aceptación:
    - Dado que el policía tiene un turno programado, cuando lo cancela o selecciona
      una nueva fecha, entonces el mecánico y el encargado reciben inmediatamente
      una notificación con el motivo (si se indicó).
    - Dado que el turno ya pasó sin asistencia, cuando el sistema lo detecta,
      entonces lo marca como "no asistido" y notifica al encargado para seguimiento.
  - Fuente: `mecanico.md`, `policia.md`

- **[US-04]** Como **policía**, quiero recibir un recordatorio automático al menos
  48 horas antes de mi mantenimiento programado para no olvidarme de ir.
  - Criterios de aceptación:
    - Dado que existe un mantenimiento programado dentro de las próximas 48 horas,
      cuando el sistema lo detecta en su ciclo de notificaciones, entonces envía un
      aviso al policía al canal configurado (correo u otro).
    - Dado que el policía ya canceló el turno, cuando el sistema evalúa los
      recordatorios pendientes, entonces no envía el aviso para ese turno cancelado.
  - Fuente: `policia.md`, `mecanico.md`

- **[US-05]** Como **mecánico**, quiero ver la agenda del día con todos los
  mantenimientos asignados para saber qué vehículos me esperan y prepararme.
  - Criterios de aceptación:
    - Dado que el mecánico abre el sistema al inicio del turno, cuando accede a la
      vista del día, entonces ve los mantenimientos ordenados por hora con el tipo de
      servicio, identificador del vehículo y nombre del policía.
    - Dado que un turno fue cancelado, cuando el mecánico consulta la agenda,
      entonces el turno aparece marcado como cancelado (no desaparece silenciosamente).
  - Fuente: `mecanico.md`

---

## Registro de órdenes de trabajo e historial

- **[US-06]** Como **mecánico**, quiero registrar digitalmente una orden de trabajo
  (vehículo, trabajos realizados, materiales usados y costos) para evitar olvidos
  y que el sistema calcule el total sin errores de suma.
  - Criterios de aceptación:
    - Dado que el mecánico completa una intervención, cuando ingresa los ítems de
      material y sus costos en la orden, entonces el sistema calcula y muestra el
      costo total automáticamente antes de que el mecánico confirme el cierre.
    - Dado que el mecánico cierra la orden, cuando la guarda, entonces queda
      vinculada al vehículo y aparece en su historial con fecha, tipo de trabajo,
      materiales y costo total.
  - Fuente: `mecanico.md`

- **[US-07]** Como **mecánico**, quiero consultar el historial de mantenimientos
  del vehículo que voy a atender para conocer el kilometraje anterior y los
  trabajos ya realizados antes de comenzar.
  - Criterios de aceptación:
    - Dado que el mecánico abre una orden de trabajo para un vehículo, cuando accede
      al historial de ese vehículo, entonces ve todas las intervenciones previas
      ordenadas cronológicamente con tipo de trabajo, materiales y costo.
    - Dado que el vehículo no tiene intervenciones anteriores en el sistema, cuando
      el mecánico consulta el historial, entonces el sistema muestra un mensaje
      explícito de "sin historial registrado" en lugar de una pantalla vacía.
  - Fuente: `mecanico.md`

- **[US-08]** Como **policía**, quiero consultar el historial de mantenimientos del
  vehículo que me asignan para saber si tiene problemas recurrentes antes de
  aceptarlo.
  - Criterios de aceptación:
    - Dado que un vehículo es asignado al policía, cuando el policía accede con
      el identificador del vehículo, entonces ve el listado de mantenimientos previos
      con fechas, tipos de trabajo y costos.
    - Dado que el policía consulta el historial, cuando hay mantenimientos
      realizados fuera de fecha (atrasados), entonces estos aparecen destacados para
      que el policía pueda identificar si el vehículo tuvo problemas de seguimiento.
  - Fuente: `policia.md`

---

## Solicitud y aprobación de repuestos

- **[US-09]** Como **encargado**, quiero crear una solicitud de repuestos en el
  sistema para que el gerente la vea en su panel y no se pierda en la bandeja
  de correo.
  - Criterios de aceptación:
    - Dado que el encargado crea una solicitud con lista de ítems y cantidades,
      cuando la envía, entonces aparece en el panel del gerente como pendiente de
      aprobación y el gerente recibe una notificación.
    - Dado que el gerente aprueba o rechaza la solicitud, cuando toma la decisión
      (con motivo opcional en caso de rechazo), entonces el encargado recibe la
      notificación en el sistema con el resultado; no requiere llamada ni nuevo correo.
  - Fuente: `encargado.md`

---

## Panel gerencial

- **[US-10]** Como **gerente**, quiero acceder desde mi celular a un panel que
  muestre el estado de mantenimientos de mis centros y las solicitudes de repuestos
  pendientes para aprobar sin depender de Excel ni del correo.
  - Criterios de aceptación:
    - Dado que el gerente accede al panel desde un smartphone, cuando navega por
      el estado de mantenimientos y solicitudes, entonces la interfaz es legible y
      operable sin zoom ni descarga de archivos Excel.
    - Dado que el gerente selecciona una solicitud de repuestos pendiente, cuando
      la aprueba o rechaza desde el panel, entonces la decisión se registra y el
      encargado del centro recibe la notificación en el sistema.
    - Dado que el gerente filtra por uno de sus centros, cuando aplica el filtro,
      entonces ve únicamente los mantenimientos de ese centro con su estado
      (programado / en curso / completado / no asistido) y el acumulado de costos
      del período.
  - Fuente: `gerente.md`
