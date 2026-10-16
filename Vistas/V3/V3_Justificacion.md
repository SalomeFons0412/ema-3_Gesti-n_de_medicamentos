# V3. Cadena de Valor y Proceso E2E

## 1. ¿A qué responde esta vista?
* Define el alcance exacto de la aplicación dentro del hospital, explicando dónde opera, quién ejecuta cada acción y qué evento desencadena cada paso.
* Muestra el proceso operativo de principio a fin de forma realista, integrando tanto la demanda programada (cirugías y hospitalización) como la no programada (urgencias).
* Sirve como puente directo y justificación base para los servicios que luego se descomponen en la vista V4.

## 2. ¿Por qué se usan estos elementos?
La vista se estructura en tres bandas horizontales correspondientes a las capas de ArchiMate:

* **Estrategia (Flujo de valor):** Mapea 5 etapas de alto nivel (*Planificar, Abastecer, Almacenar, Dispensar y Conciliar*) que se alinean directamente con las agrupaciones de capacidades de la V2.
* **Negocio (Proceso E2E):**
  * Utiliza carriles (*lanes*) para asignar responsabilidades claras a roles internos de la farmacia (Auxiliar de Farmacia, Regente y Químico Farmacéutico) y actores externos colaboradores (Compras, Proveedor y Programación Quirúrgica).
  * Modela las actividades secuenciales impulsadas por eventos reales del negocio (por ejemplo, "Agenda quirúrgica publicada" o "Alerta de lote próximo a vencer").
* **Aplicación:**
  * Incluye el componente de software con los servicios de apoyo (Predicción, Requerimientos de compra, Gestión de inventario y Alertas FEFO) que corresponden exactamente con las capacidades priorizadas en naranja en la V2.

## 3. ¿Por qué se usan estas relaciones?
* **Disparo (Triggering):** Modela la secuencia temporal y lógica (de izquierda a derecha) entre eventos, actividades y transiciones de etapa.
* **Servicio (Serving):** Señala con claridad cuáles actividades operativas específicas son soportadas directamente por la aplicación. Lo que no tiene esta relación corresponde a una tarea manual o externa a la solución.
* **Asignación (Assignment):** Define quién es responsable de cada actividad al anidarla dentro del carril del rol correspondiente.
* **Realización (Realization):** Conecta el proceso operativo con las etapas del flujo de valor estratégico, y la aplicación con los servicios que ofrece.
* **Decisiones clave de diseño:** Se modelan restricciones operativas reales, como que un lote en "cuarentena" debe bloquearse antes de que el algoritmo FEFO lo priorice. Además, se diferencian en color gris los procesos externos como las compras definitivas en el ERP y la conciliación contable.