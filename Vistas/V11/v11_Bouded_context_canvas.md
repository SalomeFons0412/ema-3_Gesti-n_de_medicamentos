## 1. Contexto de la vista V11 - Bounded Context Canvas

Esta vista presenta, en una sola página, el diseño completo del contexto delimitado **"Gestión de Vencimientos y Priorización"** — el dominio clasificado como **Core** en la V5 (DDD Estratégico), responsable de identificar medicamentos próximos a vencer, generar alertas oportunas y priorizar el uso de lotes aptos aplicando la regla FEFO (First Expired, First Out).

Se eligió este contexto para la V11 por su coherencia con las otras dos vistas desarrolladas (V9 - Interoperabilidad y V10 - Trazabilidad vertical), que también giran en torno a las alertas de vencimiento, permitiendo contar una misma historia de arquitectura desde tres ángulos complementarios: cómo se integra con el exterior (V9), cómo se justifica desde la meta de negocio (V10), y cómo se diseña internamente como contexto delimitado (V11).

## 2. Justificación del Contenido del Canvas

Se utilizó la plantilla **Bounded Context Canvas v5** de ddd-crew (github.com/ddd-crew/bounded-context-canvas), la misma presentada en las diapositivas del curso, con sus 9 secciones estándar:

*   **Clasificación Estratégica (Domain / Business Model / Evolution):** se clasificó como **Core** porque resuelve directamente el problema central del negocio (reducir pérdidas por vencimiento); como **Compliance / Cost Reduction** porque su valor es reducir riesgo regulatorio y pérdidas económicas, no generar ingreso directo; y como **Custom Built** porque su lógica (reglas FEFO, umbrales de alerta) es específica del negocio y no un commodity reemplazable por software genérico de mercado.
*   **Domain Roles:** se definió como **Execution Context**, ya que ejecuta comportamiento de negocio en tiempo real (genera alertas, decide prioridades) en lugar de solo almacenar o traducir información.
*   **Inbound Communication:** documenta de dónde recibe información este contexto — principalmente de "Gestión de medicamentos" (datos de lotes y existencias) y, de forma indirecta a través del ACL-01 (visto en la V9), de la Entidad de Vigilancia Farmacéutica.
*   **Ubiquitous Language:** recoge los términos específicos de este contexto (FEFO, lote apto, lote crítico, umbral de alerta), consistentes con el glosario general definido en la V7.
*   **Business Decisions:** las reglas de negocio no negociables que gobiernan el comportamiento del contexto (exclusión de lotes no aptos, prioridad FEFO, no duplicación de alertas).
*   **Outbound Communication:** a quién le entrega información este contexto — al proceso de dispensación (notificaciones y prioridades) y a "Gestión de requerimientos de compra" (anticipación de necesidades de reposición).
*   **Assumptions:** los supuestos bajo los cuales se diseñó el contexto, que deben validarse con el resto del equipo y el negocio.
*   **Verification Metrics:** indicadores que permitirían validar, una vez implementada la solución, si el contexto está cumpliendo su propósito.
*   **Open Questions:** puntos que quedan pendientes de definición y que se marcan explícitamente en lugar de asumir una respuesta no validada.

## 3. Decisiones de Representación en ArchiMate

*   ArchiMate no define un elemento nativo de tipo "Canvas", por lo que, siguiendo la indicación del enunciado de usar **"libre elección del equipo, justificada"**, se optó por representar cada sección del canvas como un elemento **Grouping** (agrupación visual), anidado dentro de un Grouping contenedor que representa el Bounded Context completo.
*   Dentro de cada Grouping se incluyó un elemento **Note** con el contenido textual de la sección, en lugar de usar únicamente el campo de documentación del elemento, ya que la documentación no se renderiza en la vista ni en la imagen exportada — el Note garantiza que el contenido sea visible y legible para cualquier persona que revise la imagen del repositorio, sin necesidad de abrir el archivo .archimate en Archi.
*   Esta solución preserva la esencia del Bounded Context Canvas de DDD estratégico (una sola página con todas las dimensiones de diseño de un contexto) dentro de las limitaciones de la notación ArchiMate, priorizando la legibilidad del entregable final sobre el uso estricto de elementos semánticos de una capa específica.
