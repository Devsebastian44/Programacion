# Introducción a Hadoop
## Comprender qué es Hadoop

Hadoop es un framework de código abierto que permite el procesamiento distribuido de grandes volúmenes de datos a través de clusters de computadoras utilizando modelos de programación simples. Fue desarrollado inicialmente por Apache Software Foundation y se basa en dos componentes principales:

- **HDFS (Hadoop Distributed File System)**: Sistema de archivos distribuido.
- **MapReduce**: Modelo de programación para el procesamiento paralelo de datos.

Hadoop está diseñado para escalar desde servidores individuales hasta miles de máquinas, cada una con su almacenamiento y computación local.

## Comprender qué es Big Data


Big Data se refiere al manejo de grandes volúmenes de datos que no pueden ser procesados eficientemente con herramientas tradicionales. Hadoop es una de las tecnologías más utilizadas para abordar los retos del Big Data, gracias a su capacidad de escalar horizontalmente, manejar tolerancia a fallos y trabajar con datos estructurados y no estructurados.

## Aprender sobre otro software de código abierto relacionado con Hadoop

- **Apache Hive**: Motor de consulta SQL para Hadoop.
- **Apache Pig**: Lenguaje de alto nivel para analizar datos en Hadoop.
- **Apache HBase**: Base de datos NoSQL distribuida sobre HDFS.
- **Apache Spark**: Motor de procesamiento en memoria, complementario a MapReduce.
- **Apache Flume**: Recolección de datos en tiempo real.
- **Apache Sqoop**: Importación/exportación de datos entre Hadoop y bases de datos relacionales.
- **Apache Oozie**: Programador de flujos de trabajo para Hadoop.

## Entender cómo las soluciones de Big Data pueden funcionar en la Nube

Las soluciones Big Data como Hadoop pueden implementarse en la nube para aprovechar escalabilidad y flexibilidad. Ejemplos incluyen:

- **Amazon EMR (Elastic MapReduce)**: Hadoop administrado en AWS.
- **Google Cloud Dataproc**: Servicio de procesamiento de datos en clúster.
- **Azure HDInsight**: Implementación de Hadoop en la nube de Microsoft.  

### Ventajas de usar Hadoop en la nube:

- Menor coste de infraestructura.
- Escalabilidad automática.
- Mantenimiento y gestión simplificados.


# Arquitectura de Hadoop

## Entender los principales componentes de Hadoop

- **HDFS**: Almacenamiento distribuido y tolerante a fallos.
- **MapReduce**: Motor de procesamiento por lotes.
- **YARN (Yet Another Resource Negotiator)**: Administrador de recursos del clúster.
- **Hadoop Common**: Librerías y utilidades compartidas.

## Aprender cómo funciona HDFS

HDFS divide los archivos en bloques grandes (por defecto 128 MB o 256 MB) y los distribuye a través del clúster. Está compuesto por:

- **NameNode**: Controla la metadata y el namespace del sistema de archivos.
- **DataNode**: Almacena los bloques reales de los datos.

El NameNode conoce la ubicación de cada bloque, pero no almacena los datos reales.

## Enumerar los patrones de acceso a datos para los cuales HDFS está diseñado

HDFS está optimizado para:

- Acceso secuencial a grandes archivos.
- Escritura una vez, lectura muchas veces.
- Procesamiento batch de grandes volúmenes de datos.

### No es ideal para:

- Acceso en tiempo real.
- Escritura frecuente o edición de archivos existentes.

## Describir cómo se almacenan los datos en un clúster de HDFS

- Los archivos se dividen en bloques grandes.
- Cada bloque se replica (normalmente 3 veces) en distintos DataNodes.
- Esta replicación proporciona tolerancia a fallos.
- El NameNode mantiene una tabla de metadatos con las ubicaciones de los bloques.


# Administración de Hadoop

## Agregar y quitar nodos de un clúster


Agregar nodo:

  - Configurar el nodo como DataNode y/o NodeManager.
  - Actualizar archivos de configuración (e.g., `slaves`).
  - Reiniciar o refrescar el clúster.

Quitar nodo:

  - Marcar el nodo para ser descomisionado.
  - Esperar a que los bloques se repliquen en otros nodos.
  - Retirar físicamente del clúster.

## Verificar la salud de un clúster y detener y poner en marcha los componentes de un clúster
  
Comandos útiles:

  - `hdfs dfsadmin -report`
  - `yarn node -list`
  - `jps` (ver procesos en cada nodo)

Iniciar/Detener componentes:

  - `start-dfs.sh` / `stop-dfs.sh`
  - `start-yarn.sh` / `stop-yarn.sh`

## Modificar los parámetros de configuración de Hadoop

Archivos de configuración clave:

  - `core-site.xml`: Configuración general.
  - `hdfs-site.xml`: Configuración específica de HDFS.
  - `yarn-site.xml`: Parámetros de YARN.
  - `mapred-site.xml`: Parámetros de MapReduce.

Cambiar parámetros como tamaño de bloque, número de réplicas, puertos, etc.

## Configurar una topología de rack

La topología de red permite a Hadoop ubicar los datos de manera más eficiente. Beneficios:

- Minimiza tráfico entre racks.
- Mejora la tolerancia a fallos.

Se configura mediante scripts de mapeo y el parámetro `topology.script.file.name`.


# Componentes de Hadoop

## Describir la filosofía de MapReduce

- Divide los datos en bloques.
- Aplica una función "Map" para procesar los datos en paralelo.
- Luego una función "Reduce" para combinar los resultados.

### Ejemplo:

- **Map**: Contar ocurrencias de palabras por bloque.
- **Reduce**: Sumar todas las ocurrencias de cada palabra.

## Explicar cómo se pueden utilizar Pig y Hive en un entorno Hadoop

Apache Pig:

  - Lenguaje Pig Latin.
  - Bueno para flujos de datos complejos.
  - Más flexible para programadores.

Apache Hive:

  - Sintaxis similar a SQL (HiveQL).
  - Ideal para analistas.
  - Permite trabajar con tablas sobre HDFS.
  
Ambos convierten sus scripts en tareas MapReduce que se ejecutan sobre Hadoop.

## Describir cómo se pueden utilizar Flume y Sqoop para mover datos a Hadoop

Apache Flume:

  - Recoge, agrega y transporta grandes cantidades de datos en tiempo real.
  - Ejemplo: logs de servidores web.

Apache Sqoop:

  - Transfiere datos entre bases de datos relacionales y Hadoop.
  - Útil para importar datos desde MySQL, Oracle, PostgreSQL, etc.

  

## Describir cómo se usa Oozie para programar y controlar la ejecución de trabajos en Hadoop

Apache Oozie:

  - Motor de orquestación de flujos de trabajo (workflows).
  - Controla dependencias entre tareas MapReduce, Hive, Pig, etc.
  - Permite programar tareas basadas en tiempo o eventos.

Utiliza archivos XML para definir flujos de trabajo y coordina su ejecución dentro del ecosistema Hadoop.