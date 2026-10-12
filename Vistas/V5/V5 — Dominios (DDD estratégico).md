
## 1. Contexto de la vista

Su propósito es identificar y delimitar las principales áreas de conocimiento y responsabilidad del sistema, denominadas dominios o clústeres, y clasificarlas según su importancia estratégica para el negocio.

La solución busca mejorar la gestión de medicamentos hospitalarios mediante el control del inventario por lote y fecha de vencimiento, la priorización de medicamentos aptos para su utilización, la predicción de la demanda y la generación de requerimientos de compra.


## 2. Clasificación estratégica de los dominios

Los dominios se clasifican en dos categorías:

- **Core Domain (dominio central):** representa las áreas que aportan el valor diferenciador de la solución y que responden directamente a los principales problemas identificados.
    
- **Supporting Domain (dominio de soporte):** agrupa las capacidades que contribuyen al funcionamiento del dominio central, pero cuyo propósito principal es facilitar la planificación, coordinación y ejecución de procesos complementarios.
    

En esta propuesta se identifican cuatro dominios: dos centrales y dos de soporte.

## 4. Dominios identificados

### D-01. Gestión de inventario farmacéutico — CORE

**Clasificación:** Core Domain.

**Responsabilidad:** mantener el control y la trazabilidad de los medicamentos hospitalarios mediante la gestión de lotes, existencias, estados y movimientos de inventario.

**Justificación de la clasificación:**

Este dominio es central porque el control del inventario por lote y fecha de vencimiento constituye una de las capacidades fundamentales de la solución. Permite conocer las existencias registradas, identificar los lotes disponibles y mantener la trazabilidad de las entradas y salidas de medicamentos.

Además, proporciona información esencial para reducir las diferencias entre el inventario físico y el registrado, y sirve como fuente de información para los procesos de vencimiento, priorización y abastecimiento.

**Elementos de aplicación representados:**

|Elemento|Tipo ArchiMate|Responsabilidad|
|---|---|---|
|Gestión de medicamentos|Application Component|Agrupa la responsabilidad de administrar la información de los medicamentos.|
|Gestión de lotes de medicamentos|Application Component|Gestiona la información específica de los lotes y su identificación.|
|Control de existencias por lote|Application Function|Consulta y controla las cantidades registradas para cada lote.|
|Gestión del estado del lote|Application Function|Administra los estados que condicionan la disponibilidad de los lotes.|
|Registros de movimientos del inventario|Application Function|Registra los movimientos que modifican las existencias y permiten mantener su trazabilidad.|

### D-02. Gestión de vencimientos y priorización — CORE

**Clasificación:** Core Domain.

**Responsabilidad:** identificar medicamentos próximos a vencer, generar alertas, validar restricciones de uso y priorizar la utilización de lotes aptos mediante el criterio FEFO (_First Expired, First Out_).

**Justificación de la clasificación:**

Este dominio es central porque responde directamente al objetivo de reducir las pérdidas económicas causadas por el vencimiento de medicamentos y mejorar su utilización oportuna.

Aunque depende de la información administrada por el dominio de inventario, tiene una responsabilidad diferenciada: transformar esa información en alertas y decisiones de priorización. El inventario mantiene los datos de los lotes, mientras que este dominio determina qué situaciones requieren atención y qué lotes aptos deben priorizarse.

La regla FEFO permite dar prioridad a los lotes cuya fecha de vencimiento sea más próxima, siempre que cumplan las condiciones necesarias para su utilización. Los lotes vencidos, bloqueados o en cuarentena no deben considerarse aptos para uso.

**Elementos de aplicación representados:**

|Elemento|Tipo ArchiMate|Responsabilidad|
|---|---|---|
|Gestión de alertas de vencimiento|Application Component|Agrupa las responsabilidades relacionadas con la gestión de alertas por vencimiento.|
|Prioridad de uso de lotes (FEFO)|Application Function|Determina el orden de prioridad de los lotes aptos según su fecha de vencimiento.|
|Validación de restricciones de uso|Application Function|Verifica las condiciones que impiden o limitan la utilización de un lote.|
|Notificación de lotes críticos|Application Function|Genera notificaciones sobre lotes que requieren atención prioritaria.|

### D-03. Planificación y predicción de demanda — SUPPORTING

**Clasificación:** Supporting Domain.

**Responsabilidad:** estimar las necesidades futuras de medicamentos a partir del consumo histórico y de la información de programación hospitalaria.

**Justificación de la clasificación:**

Este dominio es de soporte porque proporciona información que ayuda a anticipar las necesidades de abastecimiento, disminuir el riesgo de faltantes y mejorar las decisiones de reposición.

La predicción de demanda contribuye a que la farmacia tome decisiones con anticipación, pero su propósito está orientado a respaldar la gestión del inventario y el abastecimiento. Por ello, se considera una capacidad de apoyo frente al problema central de controlar los medicamentos y reducir las pérdidas por vencimiento.

**Elementos de aplicación representados:**

|Elemento|Tipo ArchiMate|Responsabilidad|
|---|---|---|
|Gestión de consumo histórico|Application Component|Administra la información histórica de consumo utilizada para realizar estimaciones.|
|Gestión de programación hospitalaria|Application Component|Representa la capacidad de gestionar o consultar la programación hospitalaria relevante para estimar la demanda.|
|Predicción de demanda|Application Function|Calcula estimaciones de demanda utilizando los datos disponibles.|
|Cálculo de necesidad estimada de medicamentos|Application Function|Determina las necesidades estimadas de medicamentos a partir de las previsiones obtenidas.|

**Consideración de interoperabilidad:** si la programación hospitalaria pertenece a un sistema externo, este componente debe interpretarse como una capacidad de integración o consulta y no como un módulo interno que administra el sistema hospitalario. La responsabilidad y el límite definitivo deben quedar claros en las vistas de interoperabilidad.

### D-04. Abastecimiento y requerimientos de compra — SUPPORTING

**Clasificación:** Supporting Domain.

**Responsabilidad:** calcular las cantidades requeridas para la reposición de medicamentos, gestionar la autorización técnica y realizar el seguimiento de los requerimientos de compra.

**Justificación de la clasificación:**

Este dominio es de soporte porque transforma las necesidades identificadas por la planificación y el estado del inventario en requerimientos que permiten iniciar el proceso de abastecimiento.

Su función es contribuir a que la farmacia disponga oportunamente de los medicamentos necesarios. Sin embargo, la gestión integral de compras y las decisiones administrativas o financieras que corresponden al área de Compras o al ERP quedan fuera del alcance funcional definido para esta solución.

La autorización técnica se diferencia de la aprobación administrativa o financiera de la compra. Esta separación permite respetar las responsabilidades institucionales y evitar que el sistema asuma funciones que no le corresponden.

**Elementos de aplicación representados:**

|Elemento|Tipo ArchiMate|Responsabilidad|
|---|---|---|
|Gestión de requerimientos de compra|Application Component|Agrupa la gestión de los requerimientos de compra generados por la farmacia.|
|Cálculo de cantidad requerida|Application Function|Calcula las cantidades propuestas para reponer los medicamentos.|
|Gestión de autorización técnica|Application Component|Representa la capacidad de gestionar la autorización técnica de los requerimientos.|
|Seguimiento del abastecimiento|Application Function|Permite consultar y realizar seguimiento al estado de los requerimientos y a la información de abastecimiento disponible.|

## 5. Justificación global de la clasificación

La clasificación de los cuatro dominios se fundamenta en su contribución al problema de negocio y en las responsabilidades definidas para la solución.

|Dominio|Clasificación|Justificación principal|
|---|---|---|
|D-01. Gestión de inventario farmacéutico|Core|Proporciona el control por lote, las existencias y la trazabilidad del inventario.|
|D-02. Gestión de vencimientos y priorización|Core|Permite reducir pérdidas por vencimiento mediante alertas, restricciones y priorización FEFO.|
|D-03. Planificación y predicción de demanda|Supporting|Anticipa las necesidades futuras y aporta información para evitar faltantes.|
|D-04. Abastecimiento y requerimientos de compra|Supporting|Convierte las necesidades identificadas en requerimientos de reposición y facilita su seguimiento.|

Los dos dominios centrales se consideran complementarios: D-01 mantiene la información y el control del inventario, mientras que D-02 utiliza esa información para gestionar los vencimientos y priorizar la utilización de los lotes aptos.

Los dominios de soporte complementan este funcionamiento. D-03 contribuye a anticipar la demanda y D-04 permite traducir las necesidades de reposición en requerimientos de compra.

No se incorporó un dominio genérico independiente en esta versión porque el alcance funcional representado se concentra en estas cuatro responsabilidades. Esta decisión evita añadir agrupaciones que no estén justificadas por los requerimientos identificados.