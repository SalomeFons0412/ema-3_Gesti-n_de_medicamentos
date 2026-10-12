# V3. Cadena de valor y proceso E2E

## 1. ¿A qué responde esta vista?
- Define el alcance exacto de la aplicación dentro del hospital, explicando dónde opera, quién ejecuta cada acción y qué desencadena cada paso.
- Muestra el proceso de principio a fin de forma realista, integrando tanto la demanda programada (ej. cirugías) como la no programada (urgencias). 
- Actúa como puente y justificación base para los servicios que se diseñarán en la vista V4.   

## 2. ¿Por qué se usan estos elementos?
- La vista se estructura en tres bandas correspondientes a las capas de ArchiMate: 
    - **Estrategia (Flujo de valor):**
    Mapea 5 etapas de alto nivel (Planificar, Abastecer, Almacenar, Dispensar y Conciliar) que se alinean directamente con las agrupaciones de capacidades de la V2.
    - **Negocio (Proceso E2E):**
        -  Utiliza carriles (lanes) para asignar responsabilidades claras a roles (Regente, Químico, Auxiliar) y actores (Compras, Proveedor, Programación).   
        - Contiene 18 actividades secuenciales impulsadas por eventos clave de la vida real (ej. "Agenda quirúrgica publicada" o "Alerta de lote próximo a vencer").
        
    - **Aplicación:** 
        - Incluye un componente central con 4 servicios de software (Predicción, Requerimientos de compra, Gestión de inventario y Alertas FEFO) que corresponden uno a uno con las capacidades "naranjas" impactadas en la V2.
        
**3. ¿Por qué se usan estas relaciones?**
        - **Disparo (Triggering):** Expresa la secuencia lógica cronológica (de izquierda a derecha) entre eventos, actividades y etapas.
        - **Servicio:** Indica visualmente cuáles actividades específicas son apoyadas por la aplicación. Lo que no tiene esta flecha, es un proceso manual o externo a la solución.
        - **Asignación:** Define "quién hace qué" al anidar las actividades dentro del carril de su responsable correspondiente.
        - **Realización:** Conecta el proceso operativo con el flujo estratégico, y la aplicación con los servicios que ofrece.
        - **Decisión clave de diseño:** Se modelan restricciones operativas reales, como que un lote en "cuarentena" debe bloquearse antes de que el algoritmo FEFO lo priorice. También se marcan como procesos externos (en gris) las compras definitivas y la contabilidad.   