# Sprint 1 — Que el policía llegue a su cita sin que el mecánico tenga que llamarlo a reclamar, agendando, reagendando y recibiendo recordatorio automático de su mantenimiento directamente en el sistema

**Capacidad:** 20 pts · **Comprometido:** 18 pts

| Historia | Pts | Épica | Prioridad |
|----------|-----|-------|-----------|
| US-01    | 5   | E-01  | 1         |
| US-02    | 5   | E-01  | 2         |
| US-04    | 3   | E-01  | 3         |
| US-03    | 3   | E-01  | 4         |
| US-05    | 2   | E-01  | 5         |

## Verificación de Definition of Ready (previa a la selección)

Se revisaron las 11 historias de `backlog.json` de forma independiente (no se
asumió el "ready" declarado por el Developer):

- `as_a` / `want` / `so_that`: no vacíos en las 11 historias.
- `acceptance_criteria`: ≥ 2 criterios Gherkin en cada una de las 11 historias.
- `estimate`: entero > 0 y ≤ 8 en las 11 (valores: 5, 5, 3, 3, 2, 5, 3, 3, 5, 5, 3).
- `dependencies`: todas apuntan a IDs que existen en el backlog (US-04→US-01,US-02;
  US-03→US-01,US-02; US-05→US-01,US-02; US-07→US-06; US-08→US-06; US-10→US-09;
  US-11→US-09,US-10).
- `open_questions`: vacío en las 11.

Conclusión: **las 11 historias cumplen la Definition of Ready.** Ninguna se
devuelve a refinamiento. La limitación de este sprint es de **capacidad**, no de
calidad del backlog.

## Selección (orden de prioridad: épica E-01 > E-02 > E-03 > E-04; dentro de cada
épica, por su campo `priority`)

Se recorrió el backlog en orden de prioridad acumulando puntos contra la
capacidad de 20 pts:

1. US-01 (5 pts) → acumulado 5
2. US-02 (5 pts) → acumulado 10
3. US-04 (3 pts, depende de US-01 y US-02, ambas ya incluidas) → acumulado 13
4. US-03 (3 pts, depende de US-01 y US-02, ambas ya incluidas) → acumulado 16
5. US-05 (2 pts, depende de US-01 y US-02, ambas ya incluidas) → acumulado 18

Con esto se completa toda la épica **E-01** (18 pts) sin dependencias colgantes.
La siguiente historia en prioridad, **US-06** (E-02, 5 pts), no cabe sin superar
la capacidad (18 + 5 = 23 > 20), así que no se fuerza su entrada saltándose el
orden de prioridad. El sprint queda comprometido en 18 pts, 2 pts por debajo de
la capacidad — se prefiere un sprint con foco entero en un outcome completo y
sin sobre-compromiso a partir una épica a la mitad para llenar los 2 pts
restantes.

## Backlog restante (queda fuera de este sprint, para el siguiente)

| Historia | Pts | Épica | Prioridad | Nota |
|----------|-----|-------|-----------|------|
| US-06    | 5   | E-02  | 1         | Primera candidata del próximo sprint |
| US-07    | 3   | E-02  | 2         | Depende de US-06 |
| US-08    | 3   | E-02  | 3         | Depende de US-06 |
| US-09    | 5   | E-03  | 1         | |
| US-10    | 5   | E-04  | 1         | Depende de US-09 |
| US-11    | 3   | E-04  | 2         | Depende de US-09, US-10 |

Total restante: 24 pts (42 pts totales del backlog − 18 pts comprometidos en
este sprint).
