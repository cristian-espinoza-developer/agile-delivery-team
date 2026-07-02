# Requisitos Candidatos — Gestión Vehicular Policial

Origen de la evidencia: 4 entrevistas (`gerente.md`, `mecanico.md`, `encargado.md`, `policia.md`).

---

## Gestión de mantenimientos

- **[R-01]** El sistema debe permitir registrar digitalmente una orden de trabajo, incluyendo datos del vehículo (número de motor, chasis, kilometraje), trabajos realizados, materiales usados y costos, antes de cerrar cada intervención.
  - Tipo: funcional
  - Origen: `mecanico.md` · Mecánico

- **[R-02]** El sistema debe mantener un historial de mantenimientos por vehículo consultable en cualquier momento, con fechas, tipo de trabajo y costos acumulados.
  - Tipo: funcional
  - Origen: `mecanico.md`, `policia.md` · Mecánico, Policía

- **[R-03]** El sistema debe mostrar una agenda de mantenimientos programados (vista diaria, semanal y mensual) para el mecánico y el encargado.
  - Tipo: funcional
  - Origen: `mecanico.md`, `encargado.md` · Mecánico, Encargado

- **[R-04]** El sistema debe permitir al policía agendar, cancelar o reagendar un turno de mantenimiento de forma digital, sin necesidad de llamar por teléfono.
  - Tipo: funcional
  - Origen: `mecanico.md`, `policia.md` · Mecánico, Policía

- **[R-05]** El sistema debe enviar un recordatorio automático al policía al menos dos días antes del mantenimiento programado.
  - Tipo: funcional
  - Origen: `mecanico.md`, `policia.md` · Mecánico, Policía

- **[R-06]** El sistema debe permitir al policía consultar el historial de mantenimientos y novedades del vehículo que se le asigna.
  - Tipo: funcional
  - Origen: `policia.md` · Policía

---

## Inventario de repuestos y consumibles

- **[R-07]** El sistema debe registrar el consumo de repuestos y consumibles por orden de trabajo, vinculando cada uso al vehículo y al mecánico que lo ejecutó.
  - Tipo: funcional
  - Origen: `mecanico.md`, `encargado.md` · Mecánico, Encargado

- **[R-08]** El sistema debe mostrar el stock actual de repuestos y consumibles y emitir una alerta cuando un ítem esté por debajo de un umbral mínimo configurable.
  - Tipo: funcional
  - Origen: `mecanico.md`, `encargado.md` · Mecánico, Encargado

---

## Solicitudes de repuestos y aprobaciones

- **[R-09]** El sistema debe permitir al encargado generar una solicitud de repuestos y enviarla digitalmente al gerente, y notificarle en el sistema (no solo por correo) cuando se apruebe o rechace.
  - Tipo: funcional
  - Origen: `encargado.md` · Encargado

- **[R-10]** El sistema debe permitir al gerente ver, aprobar o rechazar solicitudes de repuestos desde una bandeja centralizada, accesible desde dispositivos móviles.
  - Tipo: funcional
  - Origen: `gerente.md` · Gerente

---

## Autorización de viajes

- **[R-11]** El sistema debe proveer un flujo digital para que el encargado solicite la autorización de un viaje fuera de la ciudad o provincia, con seguimiento del estado (pendiente / aprobado / rechazado).
  - Tipo: funcional
  - Origen: `encargado.md`, `gerente.md` · Encargado, Gerente

---

## Traspasos y novedades

- **[R-12]** El sistema debe registrar digitalmente el traspaso de un vehículo entre policías, incluyendo la inspección visual y la posibilidad de adjuntar fotos del estado de entrega.
  - Tipo: funcional
  - Origen: `encargado.md`, `policia.md` · Encargado, Policía

- **[R-13]** El sistema debe permitir registrar novedades del vehículo (accidentes, rayones, ataques) con descripción textual y adjuntos de fotos o videos, y notificar automáticamente al gerente.
  - Tipo: funcional
  - Origen: `encargado.md` · Encargado

---

## Reportes y visibilidad gerencial

- **[R-14]** El sistema debe generar automáticamente un reporte diario y un consolidado mensual de costos de mantenimiento y materiales usados, sin requerir cálculo manual en Excel.
  - Tipo: funcional
  - Origen: `mecanico.md`, `gerente.md` · Mecánico, Gerente

- **[R-15]** El gerente debe contar con un panel centralizado que muestre en tiempo real los pendientes de aprobación (solicitudes de repuestos, viajes, novedades) y el estado general de los mantenimientos de sus centros.
  - Tipo: funcional
  - Origen: `gerente.md` · Gerente

- **[R-16]** El sistema debe permitir consultar el historial de traspasos, viajes autorizados y novedades de un vehículo en una sola pantalla, sin necesidad de revisar múltiples hojas de Excel.
  - Tipo: funcional
  - Origen: `encargado.md` · Encargado

---

## No funcionales

- **[R-17]** La interfaz del gerente debe ser completamente funcional en dispositivos móviles (smartphones); los reportes deben visualizarse correctamente sin depender de Excel.
  - Tipo: no funcional
  - Origen: `gerente.md` · Gerente

- **[R-18]** El sistema debe enviar notificaciones (por correo electrónico u otro canal digital configurado) ante cambios de estado en aprobaciones, solicitudes de repuestos y recordatorios de mantenimiento.
  - Tipo: no funcional
  - Origen: `mecanico.md`, `encargado.md`, `policia.md` · Mecánico, Encargado, Policía
