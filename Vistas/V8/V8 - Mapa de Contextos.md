## 1. Contexto de la vista V8 - Mapa de Contextos e Interoperabilidad

Esta vista representa el Mapa de Contextos Delimitados (Bounded Contexts) según los principios de DDD estratégico. Su propósito es visualizar las dependencias y el flujo de información entre los 4 dominios internos de la solución (definidos en la V5) y los sistemas/entidades externas con las que la Farmacia Hospitalaria debe interoperar.

## 2. Justificación de las Relaciones y Patrones (Interoperabilidad)

Las relaciones y patrones modelados se basan en la entrevista con el experto del negocio:

**Relaciones entre Dominios Internos:**
*   **Inventario hacia Vencimientos (Customer / Supplier):** El dominio de inventario actúa como proveedor (Supplier) de la base de datos de lotes y fechas; vencimientos es el cliente (Customer) que consume esta información para aplicar la regla FEFO y generar alertas.
*   **Inventario hacia Abastecimiento:** El inventario notifica cuando las existencias llegan a su punto de reorden.
*   **Planificación de Demanda hacia Abastecimiento:** Las predicciones calculadas se envían como insumo para ajustar las cantidades sugeridas a comprar.

**Relaciones con Sistemas Externos:**
*   **Programación Quirúrgica hacia Planificación (Customer / Supplier):** El sistema externo provee la agenda de cirugías para estimar las necesidades.
*   **Historia Clínica (HCE) hacia Inventario (Conformist):** Farmacia se "conforma" (Conformist) y adopta el modelo de datos que manda la historia clínica en tiempo real al validar prescripciones, ya que no tiene poder para cambiar ese sistema externo gigante.
*   **Abastecimiento hacia Sistema ERP - Compras (Capa Anticorrupción - ACL):** El dominio de Farmacia traduce sus requerimientos técnicos y los manda limpios al ERP corporativo. El ACL evita que la complejidad financiera y administrativa del ERP contamine el dominio de abastecimiento de farmacia.
*   **Proveedores Externos hacia Inventario:** Entregan físicamente los medicamentos y facturas/remisiones.
*   **Sistema ERP (Compras) hacia Inventario:** Retorna el estado de las Órdenes de Compra.
*   **Vencimientos hacia INVIMA:** Exporta los reportes regulatorios obligatorios.

## 3. Decisiones de Arquitectura ArchiMate
*   Se utilizó **Application Component** para representar tanto los Contextos Delimitados internos como los Sistemas Externos, ya que encapsulan un comportamiento y datos de forma independiente.
*   Se utilizó la relación **Serving** (flecha continua abierta) para modelar la dirección en la que un componente "sirve" o provee información de valor al componente destino, estableciendo claramente quién depende de quién en el ecosistema.