## 1. Contexto de la vista V9 - Interoperabilidad y Anticorrupción

Esta vista representa el mecanismo de **interoperabilidad** entre la Farmacia Hospitalaria y los 6 sistemas/entidades externas con los que debe intercambiar información (Programación Quirúrgica, Historia Clínica Electrónica, Servicio de Urgencias, ERP de Compras y Contabilidad, Proveedor de Medicamentos y la Entidad de Vigilancia Farmacéutica). Su propósito es mostrar cómo un único componente de **Capa Anticorrupción (Anti-Corruption Layer - ACL)** centraliza, traduce y protege al dominio interno de la complejidad y los formatos propios de cada sistema externo, evitando que esos modelos externos "contaminen" el lenguaje ubicuo y los agregados definidos para Farmacia.

## 2. Justificación de las Relaciones y Patrones (Interoperabilidad y Anticorrupción)

Las relaciones modeladas se basan en la información recogida en la entrevista con el experto del negocio y en los contratos de datos ya identificados en la V8 (Mapa de Contextos):

**Entradas al ACL (sistemas externos → ACL-01):**
*   **Programación Quirúrgica → ACL-01:** envía la agenda quirúrgica (OBJ-01) para que Farmacia anticipe la demanda de medicamentos de cirugía.
*   **Historia Clínica Electrónica → ACL-01:** envía la prescripción médica (OBJ-02) que debe validarse y despacharse. Es la relación de mayor volumen y criticidad, por lo que el ACL es indispensable: Farmacia **se adapta** al modelo de datos de la HCE sin exponer sus propios agregados a ese sistema externo.
*   **Servicio de Urgencias → ACL-01:** reporta necesidades asistenciales no programadas (OBJ-03), que requieren atención prioritaria.
*   **ERP de Compras y Contabilidad → ACL-01:** retorna el estado de los requerimientos de compra (OBJ-04) ya colocados.
*   **Proveedor de Medicamentos → ACL-01:** informa los datos de entrega y facturación (OBJ-05) de los medicamentos despachados.
*   **Entidad de Vigilancia Farmacéutica → ACL-01:** notifica alertas regulatorias (OBJ-06) que deben propagarse al dominio de vencimientos/alertas.

**Salidas del ACL (ACL-01 → dominios internos):**
*   **ACL-01 → Gestión de programación hospitalaria:** entrega la agenda quirúrgica ya traducida al lenguaje interno de programación.
*   **ACL-01 → Gestión de medicamentos:** entrega las prescripciones y necesidades asistenciales validadas, listas para descuento de inventario.
*   **ACL-01 → Gestión de requerimientos de compra:** entrega el estado actualizado de las órdenes de compra.
*   **ACL-01 → Gestión de lotes de medicamento:** entrega la confirmación de entregas y lotes recibidos del proveedor.
*   **ACL-01 → Gestión de alertas de vencimiento:** entrega las alertas regulatorias que deben cruzarse con las fechas de vencimiento de los lotes.

Con este diseño, **ningún dominio interno conoce directamente a un sistema externo**: todos pasan primero por ACL-01, que es el único componente autorizado a "hablar" los formatos de Programación Quirúrgica, HCE, Urgencias, el ERP, el proveedor y el ente regulador.

## 3. Decisiones de Arquitectura ArchiMate

*   Se utilizó **Application Component** para representar tanto al ACL-01 como a los 5 componentes internos de destino, ya que todos encapsulan comportamiento y datos propios de forma modular.
*   Los sistemas externos se representaron de forma mixta según su naturaleza: **Application Component** (sistemas de software como Programación Quirúrgica, Historia Clínica, Urgencias, ERP) y **Business Actor** (Proveedor de Medicamentos y Entidad de Vigilancia, al ser organizaciones externas y no sistemas de software).
*   Se utilizó la relación **Flow** (línea punteada con flecha rellena) para representar el intercambio de información dirigido, tanto de los sistemas externos hacia el ACL como del ACL hacia los dominios internos — enfatizando que lo que se mueve entre componentes es información/datos, no simplemente una dependencia estructural.
*   Se utilizó la relación **Access** (línea punteada con flecha abierta) entre cada sistema externo y su objeto de negocio (Business Object) correspondiente, para dejar explícito **qué dato concreto** se produce o consume en cada integración.
*   Se agregó una **Application Interface** (IF-ACL-01) conectada a ACL-01 mediante una relación de **Composition**, representando el punto de acceso único y formal mediante el cual los sistemas externos acceden a los servicios de traducción del adaptador anticorrupción.
*   El patrón aplicado corresponde a **Anti-Corruption Layer (ACL)** de DDD estratégico: un único componente de traducción se interpone entre el dominio interno y la totalidad de los sistemas externos, de modo que los cambios en cualquier sistema externo solo requieren modificar el ACL, sin impactar los dominios internos ni sus agregados.
