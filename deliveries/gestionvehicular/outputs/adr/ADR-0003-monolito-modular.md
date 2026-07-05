# ADR-0003 · Monolito modular (no microservicios) para Agenda / Órdenes / Repuestos / Panel

**Estado:** aceptado
**Fecha:** 2026-07-05

## Contexto y fuerza

Las cuatro épicas del backlog (`epics.md`) suman **42 puntos en 11 historias**
(`stories.md`, tabla de resumen de estimación) y la métrica de éxito del MVP
se mide "sobre el primer mes de uso en **al menos un centro piloto**"
(`mvp-canvas.md`). No hay evidencia en el discovery de múltiples equipos de
desarrollo que necesiten desplegar módulos de forma independiente, ni de
requisitos de escalado diferenciado por épica.

Además, las épicas tienen **alta cohesión de datos**, documentada por el
propio equipo en `epics.md`:

- E-02 "depende operativamente de que existan turnos agendados (E-01) para
  tener algo que registrar".
- E-03 "es condición previa de datos para que el panel gerencial (E-04) tenga
  algo que mostrar en su bandeja de aprobaciones".
- E-04 "depende de que existan solicitudes de repuestos (E-03) y estados de
  mantenimiento (E-01/E-02) para tener contenido real que mostrar".

Es decir: el panel gerencial no genera datos propios, solo compone datos de
los otros tres módulos. Separar estos módulos en servicios independientes
obligaría a resolver consistencia entre servicios (comunicación de red,
posible duplicación de estado) para un beneficio que el discovery no
evidencia como necesario todavía.

## Decisión

Construir un **monolito modular**: un único backend, un único proceso de
despliegue, una única base de datos (ver `ADR-0004`), con separación **lógica**
en cuatro módulos internos (Agenda, Órdenes de trabajo e Historial, Solicitud
y aprobación de repuestos, Composición del panel gerencial) que exponen sus
propios casos de uso pero comparten el mismo runtime y esquema de datos.

## Alternativas consideradas

- **Microservicios independientes por épica** (un servicio de Agenda, uno de
  Órdenes, uno de Repuestos, uno de Panel) — descartada: la complejidad
  operativa (orquestación, comunicación entre servicios, despliegues y
  monitoreo múltiples) no se justifica para 42 pts y un piloto de un centro;
  además el Panel gerencial necesitaría llamar de forma síncrona o
  eventual a los otros tres servicios para componer una sola pantalla,
  agregando latencia y puntos de falla sin beneficio evidenciado.
- **Monolito sin separación modular interna** (todo el código en una sola
  capa sin límites claros entre agenda/órdenes/repuestos) — descartada:
  dificulta el mantenimiento cuando el sistema crezca hacia las
  funcionalidades que `mvp-canvas.md` ya proyecta para "iteración 2"
  (inventario de bodega R-07/R-08, reportes avanzados R-14), que sí
  requerirán módulos nuevos y bien delimitados.

## Consecuencias

- Despliegue simple (un solo artefacto) y costo operativo bajo, proporcional
  al tamaño real del MVP y a que la validación es "en al menos un centro
  piloto" (`mvp-canvas.md`).
- Los cambios que cruzan módulos relacionados (p. ej. una solicitud de
  repuestos aprobada que debe reflejarse en el panel) son atómicos porque
  comparten transacción y base de datos.
- Si el piloto escala a un número de centros o de usuarios concurrentes que el
  discovery no evidencia hoy, esta decisión deberá revisarse — se declara
  explícitamente como decisión diferida en `architecture.md`, no como parte
  de este ADR.
- La separación modular interna (no solo monolito por conveniencia) deja una
  ruta de migración más barata a servicios independientes en el futuro, si
  llegara a ser necesario.
