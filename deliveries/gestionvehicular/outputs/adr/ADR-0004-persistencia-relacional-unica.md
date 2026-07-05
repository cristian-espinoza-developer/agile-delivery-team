# ADR-0004 · Persistencia relacional única y compartida

**Estado:** aceptado
**Fecha:** 2026-07-05

## Contexto y fuerza

El dolor más citado del mecánico en `personas.md` es de integridad de datos,
no de volumen: "El registro de materiales y costos se lleva en Excel; existe
riesgo de olvido y **ha habido errores de suma con consecuencias para la
institución**" (`mecanico.md`). **R-01** exige registrar la orden de trabajo
con cálculo automático de costos, y **US-06** (`stories.md`) lo convierte en
un criterio de aceptación verificable: "el sistema calcula y muestra el costo
total automáticamente antes de que el mecánico confirme el cierre".

Al mismo tiempo, el historial (US-07, US-08) y el panel gerencial (US-10)
hacen consultas que **cruzan entidades relacionadas**: vehículo ↔
mantenimiento programado ↔ orden de trabajo ↔ ítems de materiales, y
mantenimientos ↔ solicitudes de repuestos por centro. Ninguna historia ni
requisito del inbox evidencia volumen de datos, escritura masiva concurrente
ni necesidad de esquema flexible que justifique otra cosa.

## Decisión

Usar una **base de datos relacional (SQL) única**, con un esquema compartido
por los cuatro módulos del monolito (`ADR-0003`), y usar transacciones de
base de datos para las operaciones donde la integridad importa según el
propio dolor documentado: el cierre de una orden de trabajo con su cálculo de
costo total (US-06) y el cambio de estado de una solicitud de repuestos
(US-11).

## Alternativas consideradas

- **Base de datos NoSQL orientada a documentos** — descartada: no hay
  requisito de esquema flexible ni de escala horizontal en el discovery, y
  complicaría precisamente las consultas relacionales y las sumas agregadas
  que el historial (US-07, US-08) y el panel (US-10, "acumulado de costos del
  período") necesitan resolver de forma consistente.
- **Una base de datos por módulo (persistencia poliglota)** — descartada: es
  coherente con microservicios, que ya se descartó en `ADR-0003`; además
  dificultaría exactamente la consulta que compone el panel gerencial
  (estado de mantenimientos + solicitudes de repuestos) sin una capa de
  agregación adicional que el MVP no necesita todavía.

## Consecuencias

- Integridad referencial y transaccional garantizada donde el propio
  discovery documenta el riesgo (cálculo de costos, cambios de estado de
  aprobación), atacando directamente el dolor de "errores de suma con
  consecuencias para la institución" (`mecanico.md`).
- Un solo motor de base de datos reduce el costo operativo del piloto.
- Si en el futuro el sistema crece hacia inventario de bodega (R-07/R-08,
  fuera de alcance del MVP según `mvp-canvas.md`) o hacia microservicios
  independientes, el esquema compartido deberá revisarse — declarado como
  decisión diferida en `architecture.md`, no resuelto aquí.
