## V2. Mapa de capacidades

### 1. ¿A qué responde esta vista?
* Presenta un mapa **completo del negocio** (el hospital entero), asegurando que el diseño no se limite únicamente a las funcionalidades de la aplicación de software.
* Conecta las metas estratégicas, como reducir las pérdidas por vencimiento, mostrando exactamente *dónde* debe intervenir la solución.
* Define estrictamente **qué hace el negocio**, omitiendo de manera intencional quién lo hace (actores) o con qué lo hace (sistemas y datos), para mantener el enfoque puramente estratégico.

### 2. ¿Por qué se usan estos elementos?
Se utilizan elementos de tipo **Capability** (capa de Estrategia de ArchiMate), organizados jerárquicamente en 3 niveles:

* **Nivel 1 (Bloques):** Dirección y Gobierno, Atención Clínica, Gestión Farmacéutica y Soporte. Demuestran que la farmacia opera como un "hub" interconectado con todo el hospital.
* **Nivel 2 (Agrupaciones):** Dividen la farmacia siguiendo el ciclo de vida del medicamento: Planificación, Abastecimiento, Inventario, Dispensación y Control.
* **Nivel 3 (Capacidades):** Las acciones específicas que la organización sabe ejecutar.

**Uso de colores (Decisión de diseño del equipo):**
* **Naranja:** Capacidades que son el núcleo de la solución y realizan directamente los requerimientos (ej. "Predicción de demanda", "Gestión de inventario por lote y vencimiento", "Alertas FEFO").
* **Amarillo:** Capacidades relacionadas que interactúan con las naranjas (ej. "Recepción técnica", "Contabilidad") pero que **no** amplían el alcance de construcción de la aplicación.
* **Crema claro:** Capacidades de contexto (ej. "Hospitalización y UCI", "Facturación") que existen en el hospital pero que la solución no transforma.

### 3. ¿Por qué se usan estas relaciones?
* **Composición (Anidación):** Se utiliza la anidación visual (cajas dentro de cajas) en lugar de dibujar múltiples flechas. Esto expresa jerarquía (el bloque contiene a la agrupación, y esta a la capacidad) permitiendo que el mapa se lea de un solo vistazo y sin cruces visuales.
* **Por qué no hay Recursos ni Cursos de Acción:** Se omitieron intencionalmente para evitar mezclar sistemas (ERP, Historia Clínica) y actores dentro de un mapa estratégico. Los sistemas externos y actores se abordan en las vistas de Interoperabilidad (V9) y Cadena de Valor (V3).
* **Realización de la Motivación:** En la V2 la trazabilidad hacia los requerimientos y metas es conceptual y se documenta en las propiedades; la unión formal se consolida en la vista de trazabilidad vertical (V10).
