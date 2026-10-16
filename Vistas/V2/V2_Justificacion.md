# V2. Mapa de Capacidades Empresariales

## 1. ¿A qué responde esta vista?
* Presenta un mapa **completo del negocio** (todo el hospital), asegurando que el diseño no se limite únicamente a las funcionalidades de la aplicación de software que vamos a construir.
* Conecta directamente con las metas estratégicas, como reducir las pérdidas por vencimiento de medicamentos, mostrando con precisión en qué partes de la organización debe intervenir la solución.
* Define estrictamente **qué hace el negocio**, omitiendo de manera intencional quién lo hace (actores) o con qué tecnología lo hace (sistemas y datos), para mantener un enfoque puramente estratégico.

## 2. ¿Por qué se usan estos elementos?
Se utilizan elementos de la capa **Estrategia** de ArchiMate (*Capability*), organizados jerárquicamente en 3 niveles:

* **Nivel 1 (Bloques principales):** Dirección y Gobierno, Atención Clínica, Gestión Farmacéutica y Soporte. Muestran que la farmacia opera como un eje central interconectado con todo el hospital.
* **Nivel 2 (Agrupaciones):** Dividen la farmacia siguiendo el ciclo de vida del medicamento: Planificación, Abastecimiento, Inventario, Dispensación y Control.
* **Nivel 3 (Capacidades específicas):** Las acciones concretas que la organización sabe ejecutar en su día a día.

**Uso de colores (Decisión de diseño del equipo):**
* **Naranja:** Capacidades que **son el núcleo de la solución y realizan directamente los requerimientos** ("Predicción de demanda", "Gestión de inventario por lote y vencimiento", "Alertas FEFO").
* **Amarillo:** Capacidades relacionadas que interactúan con las naranjas ("Recepción técnica", "Contabilidad") pero que **no** amplían el alcance de construcción de la aplicación.
* **Crema claro:** Capacidades de contexto ("Hospitalización y UCI", "Facturación") que existen en el hospital **pero que la solución no transforma**.

## 3. ¿Por qué se usan estas relaciones?
* **Composición (Anidación visual):** Utilizamos cajas dentro de cajas en lugar de trazar decenas de flechas cruzadas. Esto expresa la jerarquía formal (el bloque contiene a la agrupación, y esta a la capacidad) logrando que el mapa se entienda de un solo vistazo.
* **Criterio sobre Recursos y Cursos de Acción:** Decidimos omitir intencionalmente recursos de TI y actores en este diagrama para no mezclar sistemas (como ERP o Historia Clínica) en un nivel puramente estratégico. Esos elementos se abordan en detalle en la Cadena de Valor (V3) y en la Interoperabilidad (V9).
* **Trazabilidad hacia la Motivación:** En la V2 la conexión con las metas y requerimientos de la V1 es conceptual y se documenta en las propiedades; el hilo formal de realización se consolida en la vista de Trazabilidad Vertical (V10).
