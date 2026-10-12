

## 1. Información general

- **Vista:** V1 — Motivación.
    
- **Caso de estudio:** Gestión de medicamentos en farmacia hospitalaria.
    
- **Notación:** ArchiMate.
    
- **Propósito:** Representar la motivación que justifica el diseño de la arquitectura, relacionando los problemas identificados con los objetivos, resultados esperados, requisitos, principios, restricciones y actores involucrados.
    

## 2. Objetivo de la vista

La vista V1 — Motivación tiene como objetivo explicar por qué se propone una arquitectura para mejorar la gestión de medicamentos en el entorno hospitalario. Para ello, relaciona los riesgos y necesidades identificados con los objetivos que se pretenden alcanzar, los requisitos que orientan la solución y las restricciones que delimitan su alcance.

La vista aborda cuatro frentes principales:

1. **Reducción de pérdidas por vencimiento:** mejorar la rotación de los medicamentos y priorizar su utilización según la fecha de vencimiento.
    
2. **Anticipación de las necesidades de abastecimiento:** estimar la demanda hospitalaria para mejorar la capacidad de anticipar faltantes.
    
3. **Mejora de la disponibilidad:** facilitar que los medicamentos estén disponibles oportunamente para las necesidades asistenciales.
    
4. **Trazabilidad del inventario:** controlar las existencias por lote y fecha de vencimiento, mejorando la visibilidad del inventario utilizable.
    

La motivación no representa todavía la implementación técnica del sistema. Su función es justificar la necesidad de la arquitectura y establecer las condiciones que deberán orientar las vistas posteriores.

## 3. Contexto y problema identificado

La gestión de medicamentos en una institución hospitalaria requiere mantener un equilibrio entre la disponibilidad de productos, la planificación de las necesidades y el control de las existencias. Las dificultades en estos procesos pueden ocasionar pérdidas económicas por vencimiento, faltantes de medicamentos y diferencias entre el inventario físico y el registrado.

La vista identifica cuatro problemas o necesidades principales:

### 3.1. Pérdidas económicas por vencimiento

El vencimiento de medicamentos constituye un riesgo para la institución cuando no se realiza una rotación oportuna de las existencias. Para abordar esta situación, la arquitectura plantea priorizar el uso de medicamentos aptos según su fecha de vencimiento y generar alertas de los productos próximos a vencer.

### 3.2. Incertidumbre de la demanda hospitalaria

La demanda de medicamentos puede variar de acuerdo con las necesidades asistenciales. La dificultad para anticipar esas necesidades afecta la planificación del abastecimiento y aumenta el riesgo de faltantes.

Por esta razón, se propone estimar la demanda utilizando información histórica y la programación hospitalaria, considerando tanto la demanda programada como la no programada.

### 3.3. Necesidad de disponibilidad oportuna

Los servicios asistenciales requieren medicamentos disponibles cuando los necesitan. Una planificación insuficiente puede dificultar la atención de las necesidades hospitalarias.

La arquitectura busca mejorar la disponibilidad mediante una mayor capacidad para anticipar faltantes y generar requerimientos de compra cuando sean necesarios.

### 3.4. Necesidad de trazabilidad del inventario

El control de las existencias debe permitir identificar los medicamentos por lote y fecha de vencimiento. La vista señala el riesgo de que existan diferencias entre el inventario físico y el registrado.

La trazabilidad busca mejorar el seguimiento de las existencias y facilitar la identificación de los lotes que pueden utilizarse, considerando su vencimiento y las condiciones aplicables.

## 4. Elementos de motivación representados

### 4.1. Drivers o factores de motivación

Los siguientes elementos expresan las necesidades que impulsan la arquitectura:

|Código|Elemento|Descripción|
|---|---|---|
|DR-01|Pérdidas económicas por vencimiento de medicamentos|Necesidad de reducir las pérdidas económicas asociadas al vencimiento.|
|DR-02|Incertidumbre de la demanda hospitalaria|Necesidad de mejorar la anticipación de las necesidades de medicamentos.|
|DR-03|Necesidad de disponibilidad oportuna de medicamentos|Necesidad de contar con existencias suficientes para responder a las necesidades asistenciales.|
|DR-04|Necesidad de trazabilidad del inventario|Necesidad de mejorar el seguimiento de los medicamentos y sus existencias por lote y vencimiento.|

### 4.2. Objetivos

Los objetivos representan los resultados generales que se buscan alcanzar:

|Código|Objetivo|Propósito|
|---|---|---|
|GO-01|Reducir las pérdidas por vencimiento|Disminuir las pérdidas relacionadas con medicamentos vencidos mediante una rotación oportuna.|
|GO-02|Mejorar la disponibilidad de medicamentos|Favorecer la disponibilidad oportuna para responder a las necesidades hospitalarias.|
|GO-03|Anticipar las necesidades de abastecimiento|Mejorar la capacidad de estimar necesidades y prever posibles faltantes.|
|GO-04|Mejorar la trazabilidad del inventario|Facilitar el control y seguimiento de las existencias por lote y fecha de vencimiento.|

### 4.3. Resultados esperados

Los resultados representan los efectos que se espera obtener al avanzar en el cumplimiento de los objetivos:

|Código|Resultado|Objetivo relacionado|
|---|---|---|
|OU-01|Menor cantidad de medicamentos perdidos por vencimiento|GO-01|
|OU-02|Mayor capacidad para anticipar faltantes|GO-03|
|OU-03|Mejor visibilidad de las existencias utilizables por lote|GO-04|

Estos resultados son esperados y no deben interpretarse como indicadores ya medidos ni como mejoras comprobadas en una implementación.

### 4.4. Requisitos

Los requisitos expresan necesidades concretas que la arquitectura debe considerar:

|Código|Requisito|Finalidad|
|---|---|---|
|RE-01|Controlar el inventario por lote y fecha de vencimiento|Facilitar la identificación y trazabilidad de las existencias.|
|RE-02|Generar alertas de medicamentos próximos a vencer|Advertir oportunamente sobre productos que requieren atención.|
|RE-03|Priorizar el uso según vencimiento y demanda próxima|Orientar la rotación de medicamentos aptos de acuerdo con su vencimiento y las necesidades previstas.|
|RE-04|Estimar la demanda con programación e histórico|Utilizar la programación hospitalaria y los datos históricos como insumos para anticipar necesidades.|
|RE-05|Generar requerimientos de compra de medicamentos|Apoyar la identificación y formalización de necesidades de reposición.|

Los requisitos definidos en esta vista sirven como punto de partida para la descomposición posterior de capacidades, procesos y servicios.

### 4.5. Principios

La vista contiene tres principios que orientan las decisiones de diseño:

|Código|Principio|Aplicación|
|---|---|---|
|PR-01|Trazabilidad del medicamento por lote y vencimiento|El control de inventario debe conservar la relación entre el medicamento, su lote y su fecha de vencimiento.|
|PR-02|Priorizar medicamentos aptos según su fecha de vencimiento|La rotación debe favorecer los lotes aptos con vencimiento más próximo, de acuerdo con las reglas de utilización aplicables.|
|PR-03|Separar la recomendación automatizada de la autorización de compra|La generación automática de recomendaciones o requerimientos no equivale a autorizar una compra.|

Estos principios permiten orientar la solución sin confundir el apoyo automatizado a las decisiones con las responsabilidades de autorización institucional.

### 4.6. Restricciones

Las restricciones delimitan el alcance de la arquitectura:

|Código|Restricción|Implicación|
|---|---|---|
|CO-01|Alcance centrado en gestión de medicamentos y reducción de pérdidas por vencimiento|La solución se concentra en los problemas de inventario, vencimiento, disponibilidad y abastecimiento definidos para el caso.|
|CO-02|Considerar demanda programada y no programada|La estimación debe contemplar necesidades previsibles y necesidades no programadas.|
|CO-03|No asumir la gestión integral de compras fuera del alcance definido|La solución puede generar requerimientos, pero no se presume que gestione todo el ciclo de compras.|

En particular, la generación de requerimientos no implica que la arquitectura incluya automáticamente cotizaciones, selección de proveedores, emisión de órdenes de compra, pagos o procesos contables completos.

### 4.7. Riesgos y dificultades

Los elementos de evaluación representados en la vista permiten identificar situaciones que podrían afectar el logro de los objetivos:

|Código|Elemento|Relación con la arquitectura|
|---|---|---|
|AS-01|Riesgo de vencimiento por falta de rotación oportuna|Justifica los controles de vencimiento, las alertas y la priorización de lotes.|
|AS-02|Dificultad para anticipar las necesidades de medicamentos|Justifica la estimación de demanda y la planificación del abastecimiento.|
|AS-03|Riesgo de diferencias entre inventario físico y registrado|Justifica el fortalecimiento de los movimientos y controles de inventario.|
|AS-04|Dependencia de información de otros sistemas|Señala que la calidad de la planificación puede depender de información generada en otros sistemas.|

AS-04 identifica una dependencia de información, pero no demuestra por sí sola que existan integraciones técnicas implementadas. La disponibilidad, los formatos y los mecanismos de intercambio deberán verificarse en el análisis de interoperabilidad.

## 5. Actores y áreas involucradas

La vista identifica los siguientes actores o unidades organizacionales:

|Código|Actor o área|Participación|
|---|---|---|
|ST-01|Dirección del hospital|Se relaciona con el impacto institucional y económico de las pérdidas por vencimiento.|
|ST-02|Farmacia hospitalaria|Se vincula con el control de existencias, lotes, vencimientos y trazabilidad del inventario.|
|ST-03|Área de compras|Se relaciona con las necesidades de abastecimiento y los requerimientos de compra.|
|ST-04|Servicios asistenciales|Representan las necesidades de disponibilidad de medicamentos para la atención hospitalaria.|
|ST-05|Contabilidad y auditoría|Se relaciona con el control y la revisión de las diferencias entre las existencias físicas y los registros.|

La presencia de estas áreas permite reconocer que el problema no es exclusivamente tecnológico: también involucra responsabilidades operativas, necesidades asistenciales y controles institucionales.