# ADR-0001 · Frontend web único responsive (mobile-first), sin apps nativas separadas

**Estado:** aceptado
**Fecha:** 2026-07-05

## Contexto y fuerza

El sistema tiene cuatro roles con contextos de uso distintos: el mecánico y el
encargado operan desde el taller (probablemente escritorio o tablet fija), el
policía y el gerente necesitan acceso desde su propio celular.

Dos fuerzas del inbox tiran en direcciones que hay que reconciliar en una sola
decisión de interfaz:

- **R-17** (`requisitos.md`): "La interfaz del gerente debe ser completamente
  funcional en dispositivos móviles (smartphones); los reportes deben
  visualizarse correctamente sin depender de Excel." Esto lo confirma el dolor
  `gerente#reportes-excel-celular` (`evidence-map.json`, `personas.md`): "Los
  reportes llegan casi a diario en Excel y no se visualizan correctamente en
  dispositivos móviles."
- El riesgo **S-2 · ALTO** del árbol de supuestos (`mvp-canvas.md`): "El
  mecánico usará la interfaz digital sin fricción operativa", motivado por el
  perfil real del mecánico descrito en `personas.md`: "sin estudios
  superiores y con muchos años en el proceso manual".
- US-10 (`stories.md`) fija además un criterio de aceptación explícito: "la
  interfaz es legible y operable sin zoom ni descarga de archivos Excel".

No hay evidencia en el discovery de que el mecánico o el encargado necesiten
una app instalable ni de uso desconectado (offline); tampoco hay evidencia de
volumen de usuarios que justifique invertir en dos frontends distintos.

## Decisión

Construir **un único frontend web responsive**, con diseño mobile-first,
que sirve las cuatro vistas por rol (encargado, mecánico, policía, gerente)
desde la misma base de componentes. La adaptación a celular vs. escritorio se
resuelve con diseño responsive (breakpoints), no con aplicaciones separadas.
Para el riesgo S-2 (mecánico con baja fricción esperada frente a interfaces
digitales), la vista de mecánico se diseña con formularios cortos, campos
mínimos por pantalla y controles de tamaño grande — una guía de UX, no un
componente de arquitectura distinto.

## Alternativas consideradas

- **Apps nativas separadas (iOS/Android) para el gerente + aplicación web de
  escritorio para el taller** — descartada: duplica el esfuerzo de
  construcción y despliegue (tiendas de aplicaciones, ciclos de revisión) para
  un MVP de 42 pts que se valida en un solo centro piloto (`mvp-canvas.md`,
  métrica de éxito); ningún requisito exige características nativas
  (notificaciones push, cámara, uso offline).
- **Aplicación de escritorio no responsive, con acceso al panel gerencial vía
  reporte exportado** — descartada: viola directamente R-17 y el criterio de
  aceptación explícito de US-10 sobre operar "sin zoom ni descarga de
  archivos Excel".
- **Progressive Web App (PWA) instalable** — no descartada de fondo, pero no
  se decide ahora: ninguna historia del backlog exige instalación en pantalla
  de inicio ni funcionamiento offline; queda como posible evolución del mismo
  frontend responsive, sin requerir una decisión de arquitectura distinta hoy.

## Consecuencias

- Un solo codebase de frontend reduce el costo de construir y mantener las
  4 épicas, lo cual es proporcional al tamaño real del MVP.
- Satisface R-17 sin inversión adicional en plataformas nativas.
- El riesgo S-2 no se resuelve solo con arquitectura: requiere que el equipo
  de construcción valide la usabilidad de la vista de mecánico con el propio
  mecánico (mismo riesgo que el discovery marcó como ALTO); este ADR reduce el
  costo de iterar sobre esa interfaz (un solo lugar donde ajustarla), pero no
  elimina el riesgo de adopción por sí solo.
- Se renuncia, por ahora, a capacidades nativas (notificaciones push del
  sistema operativo, uso sin conexión); si en el futuro se evidencia esa
  necesidad, requerirá un ADR propio que reemplace o extienda este.
