# Diseño de Arquitectura: Gestión de Medicamentos en Farmacia Hospitalaria

Este repositorio contiene todo el trabajo de arquitectura empresarial y de software que desarrollamos para resolver el problema de la gestión de medicamentos en la farmacia de un hospital. Lo trabajamos usando **ArchiMate** como lenguaje de modelado y aplicamos los conceptos de **Domain-Driven Design (DDD)** tanto a nivel estratégico como táctico.

---

## Tabla de Aportes del Equipo

Todo el trabajo se construyó de forma colaborativa entre los cuatro integrantes del equipo, lo cual se puede contrastar y verificar directamente en el historial de commits de este repositorio:

| Integrante | Rol / Aportes Principales | Vistas Desarrolladas |
| :--- | :--- | :--- |
| **Salomé Fonseca** | Entendimiento del negocio, Estrategia y Dominios | V2, V3 |
| **Pilar Mantilla** | Entendimiento del negocio, Estrategia y Dominios | V1, V4, V5 |
| **María Fernanda Muñoz** | Diseño Táctico (DDD), Agregados y Mapa de Contextos | V6, V7, V8 |
| **Diego Villegas** | Interoperabilidad, Trazabilidad y Canvas | V9, V10, V11 |

---

## Índice Navegable

1. [Fase 0: Contexto, Entrevista al Experto y Análisis de Hallazgos](#1-fase-0-contexto-entrevista-al-experto-y-análisis-de-hallazgos)
2. [Fase 1: Motivación del Negocio (V1)](#2-fase-1-motivación-del-negocio-v1)
3. [Fase 1: Mapa de Capacidades Empresariales (V2)](#3-fase-1-mapa-de-capacidades-empresariales-v2)
4. [Fase 1: Cadena de Valor y Proceso E2E (V3)](#4-fase-1-cadena-de-valor-y-proceso-e2e-v3)
5. [Fase 2: Descomposición de Servicios y Contratos (V4)](#5-fase-2-descomposición-de-servicios-y-contratos-v4)
6. [Fase 3: DDD Estratégico - Dominios (V5)](#6-fase-3-ddd-estratégico---dominios-v5)
7. [Fase 3: Lenguaje Ubicuo (V7)](#7-fase-3-lenguaje-ubicuo-v7)
8. [Fase 4: DDD Táctico - Agregados, Comandos y Eventos (V6)](#8-fase-4-ddd-táctico---agregados-comandos-y-eventos-v6)
9. [Fase 5: Mapa de Contextos Delimitados (V8)](#9-fase-5-mapa-de-contextos-delimitados-v8)
10. [Fase 5: Interoperabilidad y Capa Anticorrupción (V9)](#10-fase-5-interoperabilidad-y-capa-anticorrupción-v9)
11. [Fase 5: Trazabilidad Vertical (V10)](#11-fase-5-trazabilidad-vertical-v10)
12. [Fase 6: Bounded Context Canvas (V11)](#12-fase-6-bounded-context-canvas-v11)
13. [Decisiones y Supuestos Arquitectónicos](#13-decisiones-y-supuestos-arquitectónicos)

---

## 1. Fase 0: Contexto, Entrevista al Experto y Análisis de Hallazgos

### ¿De qué trata el problema?
En un hospital la farmacia no funciona como una droguería de barrio: atiende dos mundos totalmente distintos. Por un lado está la **demanda programada** (cirugías planeadas y pacientes hospitalizados donde sabemos de antemano qué medicamentos se van a requerir), y por el otro la **demanda no programada** (urgencias que llegan en cualquier momento y donde la respuesta tiene que ser inmediata). El problema principal que nos plantearon es que se están perdiendo medicamentos por vencimiento y no hay una forma clara de predecir cuánto comprar ni de priorizar los lotes que están por caducar.

### ¿Cómo hicimos la entrevista?
Para entender a fondo las reglas reales del hospital, configuramos una IA con el rol de un **Químico Farmacéutico con experiencia en dirección de farmacia hospitalaria**. Le armamos un guion enfocado en entender cómo se mueve el medicamento, qué excepciones ocurren en el día a día y con qué otros sistemas tiene que hablar la farmacia. 

* 📄 **Documento con la conversación y preguntas:** [Ver transcripción completa de la entrevista](Entrevistas/Evidencia_Entrevista_Farmacia_Hospitalaria_Parcial3.docx)

### Análisis de Hallazgos: ¿Qué aceptamos, qué descartamos y cómo nos cambió el diseño?
Aquí fue donde nosotros como equipo tomamos las decisiones de arquitectura, separando lo que nos contó el experto de lo que de verdad le correspondía hacer a nuestro software:

1. **Lo que aceptamos e influyó directo en la solución:**
   * **Regla FEFO obligatoria:** El experto nos dejó claro que medicamento que vence primero, sale primero. No hay excepciones. Si un lote entra a su fecha límite, a las 00:00 horas se debe bloquear en el sistema para que nadie lo pueda despachar por error. Esto nos llevó directo a diseñar el agregado `AlertaVencimiento` en la V6 y a ponerle un semáforo de riesgo a los lotes.
   * **Separación de kits quirúrgicos y dosis unitaria:** En cirugías se preparan "canastas" con anticipación, mientras que en hospitalización se alista la dosis exacta de 24 horas por paciente. Esto nos definió cómo estructurar el proceso de punta a punta (V3) y cómo calcular la demanda (V4).
   * **Recepción técnica y cuarentena:** Si un medicamento llega con problemas de temperatura o empaque roto, no se bota de inmediato: pasa a un estado de `Cuarentena` y queda congelado para que no sume al stock utilizable. Esto lo modelamos en los estados del lote en el dominio de inventario.
   * **Farmacia sugiere, Compras negocia:** La farmacia calcula cuántas unidades hacen falta y genera el requerimiento con el principio activo (DCI), pero la orden de compra y el pago lo hace el departamento de compras en el ERP institucional.

2. **Lo que descartamos (lo que dejamos por fuera del software):**
   * **Cobro y facturación a pacientes:** El experto nos mencionó que cada pastilla se le carga a la cuenta del paciente. Decidimos que nuestro software no se va a meter con liquidaciones, tarifas de EPS ni copagos; la farmacia solo notifica qué se consumió y el sistema de facturación del hospital que se encargue del dinero.
   * **Dispositivos físicos y dispensadores automáticos (IoT):** Aunque hay hospitales con carros inteligentes o dispensadores robóticos, dejamos eso fuera del alcance de software para mantener el sistema enfocado en la lógica de inventario, predicción y alertas.
   * **Prescripción médica:** Los doctores formulan en la Historia Clínica Electrónica. Nosotros no creamos pantallas para que el médico recete, sino que nos conectamos a la historia clínica mediante una capa de traducción (ACL).

---

## 2. Fase 1: Motivación del Negocio (V1)

Esta vista responde a una pregunta muy simple: **¿Por qué el hospital necesita este software y qué gana con él?**

![Vista 1 - Motivación](Vistas/V1/V1_Motivación.png)

### Análisis y Argumentación de la Vista 1:
* **¿Qué estamos resolviendo acá?** El impulsor principal (*Driver*) es el alto costo financiero y el riesgo que representan los medicamentos que se vencen en las estanterías sin usarse, sumado al dolor de cabeza de quedarse sin stock para cirugías.
* **Metas y Requerimientos:** Conectamos la meta principal (`Reducir pérdidas por vencimiento`) con objetivos específicos como calcular la demanda antes de que los pacientes lleguen y semaforizar los lotes críticos. De ahí se desprenden los requerimientos funcionales (`RE-01` a `RE-04`) que guían todo el diseño.
* **Restricciones del mundo real:** Tuvimos muy en cuenta restricciones innegociables como la normativa del INVIMA (farmacovigilancia y registro sanitario) y el hecho de que la farmacia depende de los presupuestos que le apruebe la dirección financiera.
* **Por qué usamos estos elementos:** Usamos elementos de la capa de Motivación de ArchiMate (*Stakeholder, Driver, Goal, Requirement, Constraint*). Esto nos sirvió para que ninguna decisión técnica posterior estuviera desconectada de una necesidad real del hospital.

---

## 3. Fase 1: Mapa de Capacidades Empresariales (V2)

Aquí quisimos ver la foto completa del hospital para entender **dónde está parada la farmacia y que es lo que hace la organización**.

![Vista 2 - Mapa de Capacidades](Vistas/V2/V2_Capacidades.png)

### Análisis y Argumentación de la Vista 2:
* **Todo el hospital:** No nos limitamos a dibujar solo las cajitas del software que íbamos a programar. Mapeamos desde la Dirección y Gobierno, pasando por la Atención Asistencial (Urgencias, Quirófanos, UCI), hasta la Gestión Farmacéutica y el Soporte.
* **Código de colores que definimos como equipo:**
  * **Cajas Naranjas:** Son las capacidades donde nuestro software entra a actuar de lleno (predicción de demanda, control de inventario por lote y alertas FEFO).
  * **Cajas Amarillas:** Son capacidades que tienen que ver con nosotros y con las que nos comunicamos (como recepción técnica o compras), pero que no forman parte de lo que vamos a construir.
  * **Cajas Crema claro:** Es el resto del hospital (facturación, hospitalización) que sabemos que existe en el entorno pero que no intervenimos.
* **Decisión de modelado:** Usamos anidación (cajas dentro de cajas) en vez de llenar el diagrama de flechas que enredaran la lectura. Esto permite entender la jerarquía en tres niveles de un solo vistazo.

---

## 4. Fase 1: Cadena de Valor y Proceso E2E (V3)

En esta vista mostramos el camino completo que recorre el medicamento desde que se planea hasta que se le entrega al paciente o se concilia.

![Vista 3 - Cadena de Valor](Vistas/V3/V3_Cadena%20de%20valor%20y%20proceso%20E2E.png)

### Análisis y Argumentación de la Vista 3:
* **Estructura en carriles (Lanes):** Organizamos el proceso por roles reales del hospital: el Auxiliar de Farmacia (que mueve cajas y hace el conteo físico), el Regente de Farmacia (que supervisa la operación diaria) y el Químico Farmacéutico (que tiene la responsabilidad técnica y legal de validar fórmulas y compras).
* **Los dos flujos de demanda en acción:**
  * Para la **demanda programada**, el proceso arranca con la agenda quirúrgica publicada. Con eso se alistan las canastas de cirugía 24 horas antes.
  * Para la **demanda no programada**, el proceso reacciona cuando Urgencias pide reposición rápida de su carro de paro.
* **Decisión clave:** Dejamos bien señalado con flechas de tipo *Serving* exactamente qué pasos operativos apoya nuestra aplicación (como la sugerencia de lotes FEFO y el cálculo de reposición), diferenciándolos de los pasos puramente manuales o externos (como la compra física en el ERP).

---

## 5. Fase 2: Descomposición de Servicios y Contratos (V4)

Acá bajamos el proceso general a servicios de negocio concretos organizados en **cuatro capas**, aplicando el principio de **Contract First** (diseñar las entradas, salidas y reglas antes de pensar en código).

![Vista 4 - Servicios](Vistas/V4/V4_Servicios.png)

### Las 4 Capas de Servicio:
1. **Capa 1 (Servicio E2E):** `SBN-01 Gestión integral de medicamentos hospitalarios`, que representa el servicio sombrilla que ofrece la farmacia.
2. **Capa 2 (Servicios Principales):** Agrupa las grandes áreas: Planificación de demanda (`SBN-02`), Disponibilidad para consumo (`SBN-03`), Inventario y vencimientos (`SBN-04`) y Requerimientos de compra (`SBN-05`).
3. **Capa 3 (Servicios Especializados):** Tareas específicas como el cálculo de reposición (`SBN-07`) y la priorización FEFO de lotes (`SBN-08`).
4. **Capa 4 (Servicios Operativos Implementables):** Operaciones atómicas que ya se pueden programar como endpoints (consultar existencias utilizables `SBN-13`, generar alertas `SBN-14`, crear requerimiento `SBN-16`).

### Tabla de Inclusión y Exclusión de Servicios

Para que no queden dudas sobre qué hace nuestro sistema y qué no, definimos esta tabla con argumentos claros:

| Código | Servicio de Negocio | Decisión | ¿Por qué tomamos esta decisión? |
|---|---|:---:|---|
| **SBN-01** | Gestión integral de medicamentos | **INCLUIDO** | Es el servicio macro que coordina toda la operación de la farmacia. |
| **SBN-02** | Planificación de demanda y abastecimiento | **INCLUIDO** | Clave para no comprar a ciegas y anticipar lo que van a pedir cirugías y urgencias. |
| **SBN-03** | Gestión de disponibilidad y priorización | **INCLUIDO** | Resuelve el núcleo del problema: saber qué lotes se pueden usar y cuáles vencen primero. |
| **SBN-04** | Gestión de inventario por lote | **INCLUIDO** | Da la trazabilidad exacta de existencias por lote, fecha y registro sanitario. |
| **SBN-05** | Gestión de requerimientos de compra | **INCLUIDO** | Permite que el Químico Farmacéutico apruebe técnicamente qué pedir al área de compras. |
| **SBN-EXT-01** | Negociación y cotización con proveedores | **EXCLUIDO** | Eso es gestión comercial propia del ERP de Compras; a la farmacia solo le interesa qué pedir y cuándo llega. |
| **SBN-EXT-02** | Facturación y cobro a aseguradoras/EPS | **EXCLUIDO** | Es un proceso contable y financiero. El software solo avisa qué se consumió. |
| **SBN-EXT-03** | Prescripción médica en cama o consulta | **EXCLUIDO** | Pertenece a la Historia Clínica Electrónica. La farmacia recibe la prescripción ya hecha. |
| **SBN-EXT-04** | Control de dispensadores robotizados (Hardware) | **EXCLUIDO** | Son tecnologías externas específicas; el software maneja la lógica de negocio, no el hardware. |

### Contratos de Servicios (Contract First)
Cada servicio incluido cuenta con su contrato definido mediante objetos de negocio pasivos (`CON-01` a `CON-16`), especificando parámetros de entrada (ej. código DCI, periodo de estimación, rango de fechas), salidas estructuradas (lista de lotes disponibles ordenados por FEFO, nivel de alerta verde/amarillo/rojo) e invariantes de negocio (no permitir existencias negativas ni sugerir lotes en cuarentena).

---

## 6. Fase 3: DDD Estratégico - Dominios (V5)

Aplicando Domain-Driven Design, agrupamos las capacidades en clústeres o dominios según el valor que aportan al negocio.

![Vista 5 - Dominios](Vistas/V5/V5_Dominios.png)

### Clasificación y Argumentación de Dominios:

1. **Dominios Centrales (Core Domains):**
   * **D-01 Gestión de inventario farmacéutico:** Es el corazón de los datos. Sin control estricto por lote, fecha de vencimiento y estado (disponible/cuarentena), todo lo demás se cae.
   * **D-02 Gestión de vencimientos y priorización:** Es la razón de ser del proyecto. Transforma los datos fríos del inventario en decisiones inteligentes de rotación aplicando la regla FEFO y evitando que los medicamentos se dañen.

2. **Dominios de Soporte (Supporting Domains):**
   * **D-03 Planificación y predicción de demanda:** Ayuda al dominio central pronosticando consumos basados en agendas de cirugías y temporadas pasadas, pero su lógica es de soporte al abastecimiento.
   * **D-04 Abastecimiento y requerimientos de compra:** Genera las requisiciones sugeridas para que el hospital no se quede sin existencias.

3. **Dominios Genéricos (Generic Domains):**
   *(Son capacidades estándar que el hospital ya tiene resueltas con herramientas de mercado o corporativas y que nosotros no reinventamos)*:
   * **D-GEN-01 Gestión de Identidad y Accesos (IAM):** Autenticación y permisos de usuarios (médicos, farmacéuticos, auxiliares) usando el Active Directory del hospital.
   * **D-GEN-02 Servicio de Notificaciones:** Envío de correos y alertas push a los teléfonos de los encargados cuando un lote pasa a estado crítico.
   * **D-GEN-03 Maestros Hospitalarios:** Datos generales de camas, quirófanos y pacientes gobernados por el sistema central del hospital.

---

## 7. Fase 3: Lenguaje Ubicuo (V7)

Para que los desarrolladores y los farmacéuticos habláramos exactamente el mismo idioma y no hubiera malentendidos en el código, armamos este glosario formal:

![Vista 7 - Lenguaje Ubicuo](Vistas/V7/V7%20-%20Lenguaje%20Ubicuo.png)

### Glosario del Negocio (Términos, Dominios y Ejemplos):

| Término | Dominio | ¿Qué significa en la farmacia? | Ejemplo real |
|---|---|---|---|
| **FEFO** | D-02 Vencimientos | *First Expired, First Out*. Regla de oro: el lote que vence antes, se gasta primero. | Si tengo un lote de Insulina que vence en 20 días y otro en 6 meses, el sistema me manda a sacar primero el de 20 días. |
| **Cuarentena** | D-01 Inventario | Estado de aislamiento de un lote que no se puede usar por sospecha de daño o aviso del INVIMA. | Un cargamento de vacunas llegó con la cadena de frío alterada: queda bloqueado en cuarentena hasta que calidad decida. |
| **Canasta Quirúrgica** | D-03 Planificación | Kit completo de medicamentos preparado con anticipación para una operación. | Canasta para una cirugía de corazón mañana a las 8:00 AM en el Quirófano 2. |
| **Dosis Unitaria** | D-01 Inventario | Preparación individualizada de medicamento empacada y rotulada para un paciente por 24 horas. | 1 pastilla de Losartán de 50 mg rotulada para el paciente de la cama 301 de Medicina Interna. |
| **Recepción Técnica** | D-01 Inventario | Revisión obligatoria donde se coteja empaque, lote, fecha de vencimiento y registro sanitario al llegar del proveedor. | El regente revisa que la caja de ampollas traiga el sello del fabricante antes de darle entrada al sistema. |
| **Punto de Reorden** | D-04 Abastecimiento | Nivel mínimo de existencias que dispara una alerta para comprar más antes de que se agote. | Cuando queden menos de 40 viales de Propofol en bodega, se crea automáticamente la orden sugerida. |

### Términos que significan cosas distintas según el contexto (Polisemia):
* **«Medicamento»:** Para la farmacia es una caja física con lote, registro sanitario y fecha de vencimiento. Para la Historia Clínica es simplemente la orden médica de qué principio activo darle al paciente ("Ibuprofeno 400 mg cada 8 horas").
* **«Requerimiento / Pedido»:** Para la farmacia es una necesidad técnica calculada por stock. Para el departamento de compras es un documento presupuestal para negociar con laboratorios comerciales.
* **«Consumo»:** En farmacia significa que la pastilla salió de la estantería físicamente. En facturación significa que se le va a cobrar ese valor a la cuenta de la EPS del paciente.

---

## 8. Fase 4: DDD Táctico - Agregados, Comandos y Eventos (V6)

Aquí nos metimos dentro de cada dominio para modelar cómo cambian los datos y qué reglas de consistencia (*invariantes*) protegemos.

![Vista 6 - Agregados](Vistas/V6/V6%20-%20Agregados,%20Comandos%20y%20Eventos.png)

### Desglose de los 4 Agregados:

#### 1. Dominio Inventario: Agregado Raíz `LoteMedicamento`
* **Entidades internas:** `LoteMedicamento` (raíz), `MovimientoInventario`.
* **Objetos de Valor (Value Objects):** `CodigoLote`, `FechaVencimiento`, `RegistroSanitario`, `StockUtilizable`, `EstadoLote` (*Disponible, Cuarentena, Vencido, Bloqueado*).
* **Invariantes protegidos:** El stock nunca puede ser negativo. Ningún lote puede entrar sin fecha de vencimiento ni registro sanitario.
* **Comandos y Eventos:** `Registrar Ingreso de Lote` $\rightarrow$ `Lote Ingresado`; `Descontar Existencias` $\rightarrow$ `Existencias Descontadas`.

#### 2. Dominio Vencimientos: Agregado Raíz `AlertaVencimiento`
* **Entidades internas:** `AlertaVencimiento` (raíz), `LotePriorizado`.
* **Objetos de Valor:** `NivelCriticidad` (*Verde, Amarillo, Rojo*), `DiasAlVencimiento`, `ReglaFEFO`.
* **Invariantes:** Lotes en estado vencido o en cuarentena tienen bloqueo automático transaccional (*Hard Stop*) a las 00:00 h y no se pueden priorizar para consumo.
* **Comandos y Eventos:** `Evaluar Prioridad FEFO` $\rightarrow$ `Prioridad FEFO Calculada`; `Bloquear Lote Vencido` $\rightarrow$ `Lote Bloqueado en Sistema`.

#### 3. Dominio Planificación: Agregado Raíz `EstimacionDemanda`
* **Entidades internas:** `EstimacionDemanda` (raíz), `ItemDemandaEstimada`.
* **Objetos de Valor:** `PeriodoPlanificacion`, `TipoDemanda` (*Programada, No programada*), `FactorSeguridad`.
* **Invariantes:** Toda predicción matemática es una sugerencia; requiere validación técnica del personal antes de pasar a compras.
* **Comandos y Eventos:** `Calcular Demanda por Programación` $\rightarrow$ `Demanda Estimada`.

#### 4. Dominio Abastecimiento: Agregado Raíz `RequerimientoCompra`
* **Entidades internas:** `RequerimientoCompra` (raíz), `LineaRequerimiento`.
* **Objetos de Valor:** `PrincipioActivo` (DCI), `Concentracion`, `CantidadSugerida`, `NivelUrgencia`.
* **Invariantes:** Ningún requerimiento puede viajar al ERP de compras sin la firma o aprobación técnica del Químico Farmacéutico.
* **Comandos y Eventos:** `Generar Requisicion Sugerida` $\rightarrow$ `Requisición Sugerida`; `Autorizar Requisición` $\rightarrow$ `Requerimiento Autorizado para ERP`.

---

## 9. Fase 5: Mapa de Contextos Delimitados (V8)

Aquí mostramos cómo se relacionan los 4 dominios de la farmacia entre sí y cómo se comunican con los sistemas que ya tiene el hospital.

![Vista 8 - Mapa de Contextos](Vistas/V8/V8%20-%20Mapa%20de%20Contextos.png)

### Patrones de Relación Utilizados:
* **Inventario hacia Vencimientos (Customer / Supplier):** Inventario le provee la base de lotes y fechas; Vencimientos es el cliente que los analiza para armar las listas FEFO.
* **Historia Clínica hacia Farmacia (Conformist):** La Historia Clínica es el sistema gigante del hospital y no va a cambiar por nosotros; nos conformamos con el formato en que ellos nos envían las prescripciones de los doctores.
* **Farmacia hacia ERP de Compras (Capa Anticorrupción - ACL):** La farmacia habla en términos de principios activos y recetas técnicas, mientras que el ERP habla de proveedores, facturas e impuestos. Usamos un ACL para que los modelos contables no ensucien nuestra lógica.
* **Vencimientos hacia INVIMA:** Exportación de reportes periódicos de farmacovigilancia bajo el formato oficial regulatorio.

---

## 10. Fase 5: Interoperabilidad y Capa Anticorrupción (V9)

En esta vista resolvimos la parte técnica de cómo conectar la farmacia con los 6 sistemas externos del hospital sin que nuestro sistema se vuelva un desorden.

![Vista 9 - Interoperabilidad y Anticorrupción](Vistas/V9/V9_Interoperabilidad_anticorrupcion.png)

### ¿Cómo funciona la Capa Anticorrupción (ACL-01)?
En vez de poner a cada dominio de farmacia a entenderse directamente con el exterior, creamos un **componente centralizado ACL-01**. Este componente actúa como un "traductor simultáneo" bidireccional:
* **Entradas al ACL:** Recibe la agenda de quirófanos (`OBJ-01`), prescripciones de la historia clínica (`OBJ-02`), urgencias (`OBJ-03`), estados de compras del ERP (`OBJ-04`), despachos del proveedor (`OBJ-05`) y alertas de retiro del INVIMA (`OBJ-06`).
* **Traducción y Protección:** El ACL limpia y transforma esos formatos externos a nuestro lenguaje interno (nuestros comandos y eventos), garantizando que si el ERP o la historia clínica cambian mañana de versión o de base de datos, nuestra lógica de farmacia no se rompa.

---

## 11. Fase 5: Trazabilidad Vertical (V10)

Esta es la vista de control donde demostramos que **el código y las funciones que diseñamos no salieron de la nada**, sino que responden directamente a las metas estratégicas del hospital.

![Vista 10 - Trazabilidad Vertical](Vistas/V10/V10_Trazabilidad_vertical.png)

### El Hilo de Trazabilidad (De la Meta al Software):
1. **Meta del Negocio:** `GO-01 Reducir las pérdidas por vencimiento` (¿Para qué existe el proyecto?).
2. **Requerimiento:** `RE-02 Generar alertas de medicamentos próximos a vencer` (Realiza directamente la meta).
3. **Servicio de Negocio:** `SBN-14 Generar alertas de vencimiento` (Es el servicio que atiende al usuario farmacéutico).
4. **Función de Aplicación:** `Notificación de lotes críticos` (La función lógica que sirve y soporta al servicio).
5. **Componente de Aplicación:** `Gestión de alertas de vencimiento` (El componente de software asignado a ejecutar la tarea).

De esta manera, cualquier evaluador puede pararse en una línea de código o en un componente y justificar con exactitud qué meta del hospital está cumpliendo.

---

## 12. Fase 6: Bounded Context Canvas (V11)

Diseñamos en una sola página el canvas completo del contexto principal: **Gestión de Vencimientos y Priorización**, siguiendo la plantilla oficial de *ddd-crew*.

![Vista 11 - Bounded Context Canvas](Vistas/V11/V11_Bounded_context_canvas.png)

### ¿Cómo resolvimos el Canvas en ArchiMate?
Como ArchiMate no trae una figura nativa llamada "Canvas", hicimos una investigación y tomamos una decisión de diseño bien fundamentada:
* Usamos elementos **Grouping** para delimitar cada uno de los 9 bloques del canvas (Clasificación estratégica, Rol, Lenguaje Ubicuo, Decisiones de negocio, Entradas, Salidas, Supuestos, Métricas y Preguntas abiertas).
* Dentro de cada bloque pusimos elementos **Note** visibles. Decidimos usar notas en vez de documentación oculta para que cualquier persona que abra el diagrama o exporte la imagen pueda leer la información completa sin tener que instalar Archi.

---

## 13. Decisiones y Supuestos Arquitectónicos

### Decisiones Clave del Equipo
1. **Modelo Único Consolidado:** Todo el diseño se encuentra en un único archivo fuente [Modelo/Modelo_Farmacia.archimate](Modelo/Modelo_Farmacia.archimate). No tenemos vistas dispersas en archivos sueltos; los mismos elementos se reutilizan en las 11 vistas para garantizar coherencia total.
2. **Aislamiento por Capa Anticorrupción (ACL):** Preferimos concentrar la interoperabilidad en un único componente de adaptación para no acoplar el núcleo de farmacia a los modelos legados del hospital.
3. **Enfoque Contract-First:** Definir contratos de datos con objetos pasivos del negocio antes de construir asegura que las interfaces entre áreas queden blindadas desde el diseño.

### Supuestos del Negocio
* **S-01:** La Historia Clínica Electrónica y el sistema de Cirugía tienen la capacidad técnica de enviar las prescripciones y agendas con al menos 24 horas de antelación para casos programados.
* **S-02:** Los medicamentos ingresan al hospital con códigos legibles (estándar GS1 o código de barras) que incluyen lote y fecha de expiración, facilitando su lectura en recepción técnica.
* **S-03:** El departamento de compras respeta las requisiciones técnicas calculadas por farmacia y notifica las órdenes de compra emitidas de manera oportuna.
