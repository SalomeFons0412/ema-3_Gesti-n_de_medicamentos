## 1. Contexto de la vista V10 - Trazabilidad Vertical

Esta vista demuestra la **trazabilidad vertical** del diseño: el camino completo que une una **meta estratégica del negocio** con el **componente de aplicación** concreto que contribuye a cumplirla, atravesando las capas de Motivación, Negocio y Aplicación. Su propósito es evidenciar que el software propuesto no se diseñó de forma aislada, sino que cada elemento de aplicación puede justificarse hacia arriba hasta una necesidad real del hospital.

Se seleccionó como hilo de trazabilidad el relacionado con **alertas de vencimiento**, por ser el núcleo del problema de negocio (reducir pérdidas por vencimiento de medicamentos) y por su coherencia con las vistas V9 (Interoperabilidad) y el dominio Core "Gestión de vencimientos y priorización" definido en la V5.

## 2. Justificación de las Relaciones (Meta → Componente de Aplicación)

El hilo de trazabilidad modelado es el siguiente, de abajo hacia arriba:

*   **"Gestión de alertas de vencimiento" (Application Component) — Assignment → "Notificación de lotes críticos" (Application Function):** el componente de aplicación tiene asignada la ejecución de la función que genera las notificaciones de lotes críticos; es decir, es el componente responsable de realizar ese comportamiento.
*   **"Notificación de lotes críticos" (Application Function) — Serving → "SBN-14 Generar alertas de vencimiento" (Business Service):** la función de aplicación sirve/soporta directamente al servicio de negocio encargado de generar las alertas de vencimiento ante el usuario del negocio (farmacia).
*   **"SBN-14 Generar alertas de vencimiento" (Business Service) — Realization → "RE-02 Generar alertas de medicamentos próximos a vencer" (Requirement):** el servicio de negocio es la forma concreta en que se realiza/satisface el requisito funcional definido desde el levantamiento de información con el experto.
*   **"RE-02" (Requirement) — Realization → "GO-01 Reducir las pérdidas por vencimiento" (Goal):** el requisito, al cumplirse, realiza y contribuye directamente al logro de la meta estratégica de negocio. Esta relación ya existía en el modelo compartido, definida originalmente en la V1 (Motivación).

Con esto, cualquier persona que revise el modelo puede partir de la meta GO-01 y llegar, sin saltos, hasta el componente de software concreto que la soporta — y viceversa, partir de un componente y justificar por qué existe.

## 3. Decisiones de Arquitectura ArchiMate

*   Se utilizó la relación **Realization** (línea punteada con triángulo hueco) para las dos transiciones donde un elemento de un nivel "concreta" o "satisface" a uno más abstracto del nivel superior (Servicio de negocio → Requisito, y Requisito → Meta), que es el uso estándar de esta relación en ArchiMate para expresar que un elemento alcanza el propósito definido por otro.
*   Se utilizó la relación **Serving** (línea continua con flecha abierta) entre la función de aplicación y el servicio de negocio, ya que representa que un elemento de una capa inferior (Aplicación) provee valor directo a un elemento de una capa superior (Negocio), sin que uno "realice" literalmente al otro.
*   Se utilizó la relación **Assignment** (línea continua con círculo relleno en el origen) entre el componente de aplicación y la función que ejecuta, siguiendo la convención de ArchiMate de representar con Assignment la asociación entre una estructura activa (el componente) y el comportamiento que realiza (la función).
*   No se incluyó un nivel de **Capacidad (Capability)** en este hilo porque, al momento de construir esta vista, la carpeta de Estrategia del modelo compartido aún no contenía los elementos de capacidad definidos en la V2; se optó por mantener la trazabilidad directa Motivación → Negocio → Aplicación para no introducir elementos no validados por el resto del equipo.
*   Se reutilizaron en su totalidad elementos ya existentes en el modelo compartido (GO-01, RE-02, SBN-14, Notificación de lotes críticos y Gestión de alertas de vencimiento), sin crear elementos nuevos, para garantizar consistencia con el trabajo ya realizado en las vistas V1, V4 y V5.
