# V4 — Descomposición de servicios

## 1. Información general

- **Vista:** V4 — Descomposición de servicios.
- **Herramienta de modelado:** Archi (ArchiMate Modeler).
- **Notación:** ArchiMate.
- **Elemento principal:** Business Service (Servicio de negocio).

## 2. Objetivo de la vista

La vista V4 presenta la descomposición funcional del proceso de gestión de medicamentos hospitalarios en cuatro capas de servicios. Su propósito es identificar las responsabilidades que conforman la solución, organizar los servicios desde una perspectiva general hasta operaciones específicas y establecer las relaciones necesarias para comprender su colaboración.

La descomposición permite identificar los servicios que participan en la planificación de la demanda, la gestión de inventario, el control de vencimientos, la disponibilidad de medicamentos y la generación de requerimientos de compra.

Además, esta vista sirve como base para documentar los contratos de cada servicio mediante el enfoque Contract First, definiendo sus entradas, salidas, reglas de negocio y condiciones excepcionales antes de concretar su implementación.


## 3. Estructura de las cuatro capas

### Capa 1 — Servicio E2E

Esta capa representa el servicio de negocio de mayor nivel, que agrupa las responsabilidades principales de la solución.

| Código | Servicio | Responsabilidad |
|---|---|---|
| SBN-01 | Gestión integral de medicamentos hospitalarios | Coordinar las capacidades de planificación, disponibilidad, inventario y abastecimiento de medicamentos. |

### Capa 2 — Servicios principales

Esta capa divide la responsabilidad integral en áreas funcionales de negocio.

| Código | Servicio | Responsabilidad |
|---|---|---|
| SBN-02 | Planificación de demanda y abastecimiento | Estimar las necesidades y apoyar la planificación del abastecimiento. |
| SBN-03 | Gestión de disponibilidad para dispensación y consumo | Consultar la disponibilidad de medicamentos aptos para atender necesidades asistenciales. |
| SBN-04 | Gestión de inventario y vencimientos | Mantener la información del inventario, los lotes y sus fechas de vencimiento. |
| SBN-05 | Gestión de requerimientos de compra | Gestionar la preparación y consulta de requerimientos de compra. |

### Capa 3 — Servicios especializados

Esta capa descompone las responsabilidades principales en servicios especializados.

| Código | Servicio | Responsabilidad |
|---|---|---|
| SBN-06 | Estimación de demanda hospitalaria | Estimar la demanda utilizando la información disponible. |
| SBN-07 | Cálculo de necesidades de reposición | Determinar cantidades sugeridas para reponer medicamentos. |
| SBN-08 | Gestión de lotes próximos a vencer y priorización FEFO | Identificar lotes próximos a vencer y priorizar lotes aptos. |
| SBN-09 | Administración de movimientos y existencias | Gestionar los movimientos que afectan las existencias. |
| SBN-10 | Elaboración y seguimiento de requerimientos | Preparar requerimientos y consultar su estado. |

### Capa 4 — Servicios operativos implementables

Esta capa contiene las operaciones específicas que materializan las responsabilidades especializadas.

| Código | Servicio | Responsabilidad |
|---|---|---|
| SBN-11 | Calcular y consultar demanda prevista | Obtener la demanda estimada para un medicamento y periodo determinados. |
| SBN-12 | Calcular cantidades sugeridas de reposición | Calcular cantidades de reposición según los parámetros definidos. |
| SBN-13 | Consultar existencias utilizables por lote | Consultar las cantidades utilizables y el estado de los lotes. |
| SBN-14 | Generar alertas de vencimiento | Identificar lotes que cumplen las condiciones configuradas de alerta. |
| SBN-15 | Registrar entradas y movimientos de inventario | Registrar movimientos y actualizar las existencias correspondientes. |
| SBN-16 | Crear y consultar requerimientos de compra | Crear requerimientos y consultar su información y estado. |

Los nombres anteriores corresponden al catálogo funcional utilizado para documentar la vista. Las relaciones definitivas deben conservar la coherencia con el modelo de Archi y con el proceso E2E.

## 5. Relaciones entre servicios

Las relaciones representadas en el diagrama expresan cómo se organiza y colabora la descomposición de servicios.

- **Relaciones verticales:** representan la descomposición desde el servicio integral hacia responsabilidades más específicas y operaciones implementables.
- **Relaciones horizontales:** representan colaboraciones entre servicios que participan en una misma responsabilidad o necesitan información de otros servicios.

## 6. Tabla de inclusión y exclusión

### 6.1. Criterio de delimitación

La inclusión o exclusión de una responsabilidad se determina según su contribución a los objetivos del proyecto, su relación con el proceso de gestión de medicamentos y la responsabilidad de los sistemas o actores existentes.

La exclusión de una capacidad no significa necesariamente que no exista en el hospital. Significa que su implementación integral no forma parte del alcance definido para esta solución.

### 6.2. Responsabilidades incluidas

| ID | Responsabilidad | Justificación |
|---|---|---|
| IN-01 | Estimación y planificación de la demanda | Permite anticipar las necesidades de medicamentos y apoyar las decisiones de abastecimiento. |
| IN-02 | Gestión del inventario por lote y vencimiento | Permite conocer las existencias y conservar la trazabilidad de los medicamentos. |
| IN-03 | Generación de alertas de vencimiento | Facilita la identificación oportuna de lotes próximos a vencer. |
| IN-04 | Priorización FEFO | Ayuda a priorizar lotes aptos cuyo vencimiento sea más próximo. |
| IN-05 | Consulta de existencias utilizables | Proporciona información sobre la disponibilidad real de medicamentos. |
| IN-06 | Registro de entradas y movimientos | Permite mantener las existencias actualizadas y conservar el historial de movimientos. |
| IN-07 | Cálculo de cantidades sugeridas de reposición | Apoya la determinación de necesidades de abastecimiento. |
| IN-08 | Creación y consulta de requerimientos de compra | Permite formalizar las necesidades de reposición para su revisión y autorización. |

### 6.3. Responsabilidades excluidas

| ID | Responsabilidad excluida | Justificación |
|---|---|---|
| EX-01 | Ejecución integral de cotizaciones y órdenes de compra | La solución prepara requerimientos, pero no sustituye el proceso institucional de compras. |
| EX-02 | Gestión contable, pagos y conciliación financiera | Estas responsabilidades corresponden a los procesos y sistemas financieros institucionales. |
| EX-03 | Gestión clínica integral e historias clínicas | La solución no reemplaza los sistemas clínicos ni sus responsabilidades asistenciales. |
| EX-04 | Diagnóstico, prescripción y autorización clínica | Estas decisiones corresponden a los profesionales autorizados y a los procesos clínicos. |
| EX-05 | Fabricación y distribución por parte de proveedores | Son actividades externas que no forman parte de la operación de la solución. |
| EX-06 | Decisiones regulatorias y gestión integral de farmacovigilancia | Las decisiones regulatorias pertenecen a las autoridades competentes. La solución puede apoyar la trazabilidad y las alertas pertinentes. |

### 6.4. Consideraciones sobre las exclusiones

La solución puede necesitar intercambiar información con sistemas externos para cumplir sus objetivos. Esta necesidad no implica asumir la responsabilidad funcional de esos sistemas.

Los intercambios, su dirección y los mecanismos técnicos correspondientes deben documentarse en la vista específica de interoperabilidad y validarse con los responsables del proceso.

### 7. Catálogo de contratos

| Contrato | Servicio asociado | Propósito |
|---|---|---|
| CON-01 | SBN-01 | Coordinar la gestión integral de medicamentos hospitalarios. |
| CON-02 | SBN-02 | Especificar la planificación de demanda y abastecimiento. |
| CON-03 | SBN-03 | Especificar la consulta de disponibilidad para dispensación y consumo. |
| CON-04 | SBN-04 | Especificar la gestión de inventario y vencimientos. |
| CON-05 | SBN-05 | Especificar la gestión de requerimientos de compra. |
| CON-06 | SBN-06 | Definir las entradas y salidas de la estimación de demanda. |
| CON-07 | SBN-07 | Definir el cálculo de necesidades de reposición. |
| CON-08 | SBN-08 | Definir la identificación de lotes próximos a vencer y la priorización FEFO. |
| CON-09 | SBN-09 | Definir la administración de movimientos y existencias. |
| CON-10 | SBN-10 | Definir la elaboración y el seguimiento de requerimientos. |
| CON-11 | SBN-11 | Especificar el cálculo y la consulta de demanda prevista. |
| CON-12 | SBN-12 | Especificar el cálculo de cantidades sugeridas de reposición. |
| CON-13 | SBN-13 | Especificar la consulta de existencias utilizables por lote. |
| CON-14 | SBN-14 | Especificar la generación de alertas de vencimiento. |
| CON-15 | SBN-15 | Especificar el registro de entradas y movimientos de inventario. |
| CON-16 | SBN-16 | Especificar la creación y consulta de requerimientos de compra. |

### 7.4. Contratos detallados

#### CON-01 — Gestión integral de medicamentos hospitalarios

**Servicio asociado:** SBN-01.

**Entradas**
- Demanda programada y no programada.
- Información histórica de consumo.
- Existencias, lotes y fechas de vencimiento.
- Necesidades de reposición y requerimientos de compra.

**Salidas**
- Información consolidada de demanda y disponibilidad.
- Alertas de vencimiento y necesidades de reposición.
- Requerimientos preparados por los servicios correspondientes.

**Reglas**
- Mantener la trazabilidad de la información.
- Diferenciar una recomendación de compra de una compra autorizada.
- Considerar la aptitud de los lotes para determinar la disponibilidad.

**Excepciones:** señalar resultados incompletos cuando falten datos críticos.

#### CON-02 — Planificación de demanda y abastecimiento

**Servicio asociado:** SBN-02.

**Entradas:** demanda prevista, consumo histórico, necesidades programadas y no programadas, y existencias disponibles.

**Salidas:** necesidades estimadas de abastecimiento, prioridades y necesidades de reposición.

**Reglas:** distinguir los tipos de demanda, considerar la disponibilidad y documentar los datos faltantes.

**Excepciones:** informar cuando el historial o los datos disponibles sean insuficientes.

#### CON-03 — Gestión de disponibilidad para dispensación y consumo

**Servicio asociado:** SBN-03.

**Entradas:** necesidades asistenciales autorizadas, existencias por lote, vencimientos y restricciones de uso.

**Salidas:** disponibilidad consultable, lotes candidatos y faltantes identificados.

**Reglas:** excluir lotes vencidos, bloqueados o no aptos y respetar las validaciones clínicas y farmacéuticas aplicables.

**Excepciones:** informar cuando no existan lotes aptos o falten datos para verificar la disponibilidad.

#### CON-04 — Gestión de inventario y vencimientos

**Servicio asociado:** SBN-04.

**Entradas:** registros de lotes, fechas de vencimiento, recepciones, movimientos, bloqueos y ajustes autorizados.

**Salidas:** existencias actualizadas, alertas y diferencias de inventario identificadas.

**Reglas:** conservar la trazabilidad de los movimientos y distinguir lotes disponibles, bloqueados y vencidos.

**Excepciones:** señalar registros inválidos e inconsistencias que requieran revisión.

#### CON-05 — Gestión de requerimientos de compra

**Servicio asociado:** SBN-05.

**Entradas:** cantidades sugeridas, faltantes, existencias utilizables y datos de los medicamentos.

**Salidas:** requerimientos preparados para revisión y autorización.

**Reglas:** justificar las cantidades, diferenciar el requerimiento de la orden de compra y conservar la aprobación humana cuando corresponda.

**Excepciones:** señalar requerimientos incompletos, duplicados o pendientes de revisión.

#### CON-06 — Estimación de demanda hospitalaria

**Servicio asociado:** SBN-06.

**Entradas:** programación quirúrgica, necesidades hospitalarias, consumo histórico y demanda no programada disponible.

**Salidas:** demanda estimada por medicamento y horizonte temporal.

**Reglas:** diferenciar demanda programada y no programada; identificar supuestos y limitaciones.

**Excepciones:** señalar datos insuficientes o inconsistentes.

#### CON-07 — Cálculo de necesidades de reposición

**Servicio asociado:** SBN-07.

**Entradas:** demanda prevista, existencias utilizables, necesidades pendientes y parámetros de reposición.

**Salidas:** cantidades sugeridas de reposición.

**Reglas:** considerar las existencias utilizables y evitar cantidades negativas; documentar los parámetros utilizados.

**Excepciones:** no presentar una cantidad definitiva si faltan parámetros esenciales.

#### CON-08 — Gestión de lotes próximos a vencer y priorización FEFO

**Servicio asociado:** SBN-08.

**Entradas:** medicamento, lote, fecha de vencimiento, cantidad, estado y demanda próxima.

**Salidas:** lotes priorizados, alertas y recomendaciones.

**Reglas:** priorizar los lotes aptos con vencimiento más próximo; excluir los vencidos o bloqueados.

**Excepciones:** señalar fechas inválidas o ausencia de lotes aptos.

#### CON-09 — Administración de movimientos y existencias

**Servicio asociado:** SBN-09.

**Entradas:** medicamento, lote, tipo de movimiento, cantidad, fecha y soporte.

**Salidas:** movimiento registrado y existencias resultantes.

**Reglas:** validar el lote y el movimiento; conservar el historial y evitar duplicados.

**Excepciones:** rechazar movimientos inválidos y señalar diferencias que requieran conciliación.

#### CON-10 — Elaboración y seguimiento de requerimientos

**Servicio asociado:** SBN-10.

**Entradas:** necesidades de reposición, medicamentos, cantidades sugeridas y estado del requerimiento.

**Salidas:** requerimientos documentados y estado de seguimiento.

**Reglas:** permitir la identificación y consulta del requerimiento y respetar el proceso de autorización definido.

**Excepciones:** informar si el requerimiento no existe, está incompleto o presenta un cambio de estado no permitido.

#### CON-11 — Calcular y consultar demanda prevista

**Servicio asociado:** SBN-11.

**Entradas:** medicamento, periodo de consulta, demanda programada e información histórica disponible.

**Salidas:** demanda prevista para el medicamento y periodo solicitado.

**Reglas:** identificar el horizonte estimado y los datos utilizados; señalar limitaciones.

**Excepciones:** informar parámetros inválidos o información insuficiente.

#### CON-12 — Calcular cantidades sugeridas de reposición

**Servicio asociado:** SBN-12.

**Entradas:** demanda prevista, existencias utilizables, necesidades pendientes y parámetros de reposición.

**Salidas:** cantidad sugerida de reposición por medicamento.

**Reglas:** aplicar los parámetros definidos, evitar cantidades negativas y diferenciar la sugerencia de la compra autorizada.

**Excepciones:** indicar los datos o parámetros faltantes.

#### CON-13 — Consultar existencias utilizables por lote

**Servicio asociado:** SBN-13.

**Entradas:** identificador del medicamento y criterios de consulta, como el lote.

**Salidas:** lotes encontrados, cantidades utilizables, vencimientos y estados registrados.

**Reglas:** excluir del total utilizable los lotes vencidos, bloqueados o no aptos.

**Excepciones:** devolver un resultado vacío cuando no haya coincidencias e informar datos incompletos.

#### CON-14 — Generar alertas de vencimiento

**Servicio asociado:** SBN-14.

**Entradas:** lotes, fechas de vencimiento, cantidades y umbral de alerta configurado.

**Salidas:** alertas con medicamento, lote, vencimiento y prioridad definida.

**Reglas:** aplicar el umbral configurado y evitar duplicados mientras una alerta siga vigente.

**Excepciones:** señalar fechas inválidas o ausencia de un umbral configurado.

#### CON-15 — Registrar entradas y movimientos de inventario

**Servicio asociado:** SBN-15.

**Entradas:** medicamento, lote, cantidad, tipo de movimiento, fecha y soporte.

**Salidas:** confirmación del registro, identificador del movimiento y existencias resultantes.

**Reglas:** validar los campos obligatorios, impedir duplicados y conservar la trazabilidad.

**Excepciones:** rechazar registros incompletos o inválidos sin confirmar una operación que no se realizó.

#### CON-16 — Crear y consultar requerimientos de compra

**Servicio asociado:** SBN-16.

**Entradas:** medicamento, cantidad solicitada o sugerida, justificación e identificación del requerimiento.

**Salidas:** requerimiento creado o detalle del requerimiento consultado, incluido su estado.

**Reglas:** validar los campos obligatorios, asignar un identificador único y diferenciar el requerimiento de una compra ejecutada.

**Excepciones:** informar datos inválidos, requerimientos inexistentes o duplicidades detectadas.
**Estado final de la documentación:** la estructura del README y las fichas propuestas están preparadas. La validación de las relaciones, la asociación efectiva de los contratos en Archi y la confirmación de las reglas pendientes son necesarias antes de declarar cerrada la vista.