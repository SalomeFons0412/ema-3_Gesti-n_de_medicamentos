## 1. Contexto de la vista V7 - Lenguaje Ubicuo (DDD)

Esta vista modela el Lenguaje Ubicuo (Ubiquitous Language) del sistema. Su propósito es definir un glosario estricto y compartido entre los expertos del dominio y el equipo de desarrollo, eliminando ambigüedades. Las definiciones aquí plasmadas rigen la forma en que se nombran los componentes, atributos y eventos en el código y en la arquitectura.

## 2. Glosario de Términos (Extraídos de la entrevista con el experto)

*   **FEFO (First Expired, First Out):** Regla de negocio innegociable para la priorización de lotes. Significa que el medicamento cuya fecha de vencimiento sea más próxima debe ser el primero en despacharse o consumirse.
*   **Cuarentena:** Estado que se le asigna a un lote de medicamentos cuando sufre una alteración (ej. ruptura de cadena de frío) o hay una alerta regulatoria. Un lote en cuarentena no suma al inventario disponible y está bloqueado transaccionalmente.
*   **Canasta Quirúrgica:** Conjunto (kit) de medicamentos requeridos para atender una demanda programada en quirófano. Requiere preparación anticipada (picking) y validación técnica previa.
*   **Dosis Unitaria:** Sistema de dispensación enfocado en la demanda programada (Hospitalización/UCI) donde se prepara y entrega la cantidad exacta de medicamento requerida por un paciente para un periodo de 24 horas.
*   **Recepción Técnica:** Proceso inicial obligatorio al recibir un medicamento del proveedor, donde se verifica el código interno, laboratorio, lote, fecha de vencimiento y registro sanitario antes de considerarlo "Disponible" en el sistema.

## 3. Decisiones de Arquitectura ArchiMate

De acuerdo con las instrucciones de la rúbrica para la capa de Negocio y Motivación, se utilizaron los siguientes elementos:
*   **Representation (Capa Business):** Se utilizó para modelar el término exacto o el significante (el nombre de la palabra en el lenguaje de la farmacia hospitalaria).
*   **Meaning (Capa Motivation):** Se utilizó para modelar el significado estricto y la implicación del término dentro de las reglas del negocio.
*   **Association relationship:** Para vincular lógicamente el término representativo con su definición exacta.