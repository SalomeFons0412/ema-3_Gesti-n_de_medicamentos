## 1. Contexto de la vista V6 - Agregados, Comandos y Eventos (DDD Táctico)

Esta vista responde al diseño táctico de Domain-Driven Design (DDD). Su propósito es definir, para cada uno de los 4 dominios establecidos en la V5, cuáles son las raíces de agregación (Aggregate Roots), los límites de consistencia que protegen (Invariantes) y el flujo de mutación de estado a través de Comandos (acciones) y Eventos de Dominio (resultados).

## 2. Justificación de los Agregados por Dominio

La definición de estos agregados se basó estrictamente en las reglas de negocio levantadas en la entrevista con el experto de Farmacia Hospitalaria.

### D-01. Gestión de inventario farmacéutico (Core)
*   **Agregado Raíz:** `LoteMedicamento`
*   **Límites e Invariantes:** La cantidad de existencias nunca puede ser negativa. Todo lote debe tener obligatoriamente Código, Descripción, Proveedor, Fecha de Vencimiento y Registro Sanitario desde la Recepción Técnica. Un lote solo puede estar en estado: Disponible, Cuarentena, Vencido o Bloqueado (Baja).
*   **Comandos:** `Registrar Ingreso de Lote`, `Descontar Existencias` (originado por consumo o avería), `Cambiar Estado`.
*   **Eventos:** `Lote Ingresado`, `Existencias Descontadas`, `Estado de Lote Actualizado`.

### D-02. Gestión de vencimientos y priorización (Core)
*   **Agregado Raíz:** `AlertaVencimiento`
*   **Límites e Invariantes:** Regla FEFO (First Expired, First Out) innegociable. Lotes en estado "Vencido" o "Cuarentena" no suman al Inventario Disponible y generan un bloqueo transaccional (Hard Stop) automático impidiendo su picking o dispensación a las 00:00 horas del día de expiración.
*   **Comandos:** `Evaluar Prioridad FEFO`, `Generar Alerta de Vencimiento` (semaforización), `Bloquear Lote Vencido`.
*   **Eventos:** `Prioridad FEFO Calculada`, `Alerta Generada`, `Lote Bloqueado en Sistema`.

### D-03. Planificación y predicción de demanda (Supporting)
*   **Agregado Raíz:** `EstimacionDemanda`
*   **Límites e Invariantes:** La estimación debe considerar la Demanda Programada (Programación quirúrgica, censos hospitalarios) y la Demanda no programada (Histórico de urgencias). Toda predicción es una sugerencia que debe ser validada por un humano antes de pasar a abastecimiento.
*   **Comandos:** `Calcular Demanda por Programación`, `Ajustar Predicción Manualmente`.
*   **Eventos:** `Demanda Estimada`, `Predicción Ajustada por Químico`.

### D-04. Abastecimiento y requerimientos de compra (Supporting)
*   **Agregado Raíz:** `RequerimientoCompra` (Requisición)
*   **Límites e Invariantes:** El requerimiento debe contener obligatoriamente el principio activo (Denominación Común Internacional), concentración, cantidad y nivel de urgencia. Invariante crítica: Ningún requerimiento puede viajar a Compras (ERP) sin la revisión y autorización técnica previa del Químico Farmacéutico.
*   **Comandos:** `Generar Requisición Sugerida`, `Autorizar Requisición Técnica`.
*   **Eventos:** `Requisición Sugerida`, `Requerimiento Autorizado para ERP`.

## 3. Decisiones de Arquitectura ArchiMate
*   Se utilizaron **Data Objects** (Objetos de Datos de la capa de aplicación) para representar los *Agregados*, ya que encapsulan el estado de la entidad.
*   Se utilizaron **Application Processes** para modelar los *Comandos*, reflejando la acción o intención de modificar el estado.
*   Se utilizaron **Application Events** para representar los *Eventos de Dominio* que se desencadenan una vez el invariante es validado y el comando se ejecuta exitosamente.