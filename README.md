# UNIDAD 2: Agile Delivery Team

En la unidad 2 se implementa el segundo paso del proyecto que sería el Agile Delivery Team utilizando modelos de lenguaje avanzado de Claude para generar las historias de usuario, épicas, el sprint plan y la arquitectura del Sistema de Gestión Vehicular para la Policía Nacional. 

Después de la implementación y el descubrimiento realizado por la IA podemos responder las siguientes preguntas:

## ¿Qué cambió en tu plan cuando separaste el trabajo en cuatro roles en vez de pensarlo "todo junto"?

Creo que el cambio principal fue que la arquitectura dejó de centrarse en las funcionalidades técnicas aisladas para enfocarse en la coherencia del flujo de trabajo por cada rol. Al organizar las 4 épicas (E-01 a E-04) según los outcomes de cada usuario (policía, mecánico, encargado, gerente) y no por módulos técnicos, la planeación se realizó con las siguientes características:

* Se pudo establecer que el agendamiento (E-01) era la base que habilitaba todo lo demás, ya que sin turnos programados no hay órdenes que registrar (E-02) ni solicitudes de repuestos que gestionar (E-03).
* Al ver que todos los roles necesitaban interactuar con el sistema, pero con diferentes necesidades, se puede optar por un único aplicativo web responsive en lugar de múltiples apps, permitiendo que la misma base de componentes sirva tanto para el taller como para el celular del gerente o policía.
* Al identificar que el gerente no necesita un sistema de reportes independiente sino un panel de resumen de lo que hacen los otros roles, se puede evitar duplicar el estado de los datos, integrándolo todo en un solo monolito modular.

## ¿Qué historia te costó más dejar lista según INVEST, y por qué?

La historia que presentó mayor complejidad para cumplir con la característica de ser Small (pequeña) según INVEST fue la US-10 (Ver panel de estado de mantenimientos y costos por centro), ya que la historia original del Product Owner abarcaba tres responsabilidades distintas: la interfaz móvil del panel, la lógica de aprobar/rechazar solicitudes y la visualización de estados con el acumulado de costos. 

Esto la situaba en el tope de 8 puntos, lo que la hacía difícil de gestionar y medir. Para mantener la integridad de INVEST, se decidió dividirla en dos historias entregables de forma independiente: US-10 (focalizada en la visualización del panel) y US-11 (focalizada específicamente en la aprobación/rechazo de solicitudes). Esto permitió resolver el problema del alcance y asegurar que cada parte fuera lo suficientemente pequeña y verificable.

## ¿Para qué te serviría un gate de Definition of Ready (DoR) en tu equipo real?

Un gate de Definition of Ready nos serviría para garantizar que el equipo de construcción no reciba trabajo sin terminar o ambiguo, lo cual reduce drásticamente el desperdicio operativo. 

En nuestro caso específico, el DoR utilizado permitió:
* **Evitar devoluciones a refinamiento:** Al validar que cada historia tuviera criterios de aceptación en Gherkin, estimaciones claras y dependencias resueltas, se aseguró que el Sprint 1 tuviera un foco total en el outcome, sin interrupciones por dudas técnicas durante la ejecución.
* **Resolver preguntas abiertas:** Permitió resolver dudas (como el canal de notificación en US-04 o el alcance del acumulado de costos en US-10) antes de comprometer capacidad de desarrollo, asegurando que el diseño se mantuviera simple y dentro de los límites del MVP.
* **Gestión de dependencias:** Aseguró que las historias que dependen de otras (como US-04 que requiere US-01 y US-02) no se ejecutaran de forma aislada, garantizando que el flujo de valor sea funcional desde el primer día.