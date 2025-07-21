# Introducción a Spark

## ¿Qué es Spark y cuál es su propósito?

Apache Spark es un motor de análisis unificado para procesamiento de datos a gran escala. Fue desarrollado originalmente en la Universidad de California, Berkeley, y posteriormente donado a la Apache Software Foundation.

### Características principales de Spark:

- **Velocidad**: Hasta 100 veces más rápido que Hadoop MapReduce para aplicaciones en memoria
- **Facilidad de uso**: APIs disponibles en Java, Scala, Python y R
- **Generalidad**: Combina SQL, streaming, machine learning y procesamiento de grafos
- **Compatibilidad**: Funciona con diversas fuentes de datos y sistemas de almacenamiento

### Propósitos principales:

1. **Procesamiento por lotes**: Análisis de grandes volúmenes de datos históricos
2. **Streaming en tiempo real**: Procesamiento de datos en tiempo real
3. **Machine Learning**: Algoritmos de aprendizaje automático escalables
4. **Análisis de grafos**: Procesamiento de estructuras de datos complejas
5. **Análisis interactivo**: Consultas SQL ad-hoc sobre grandes datasets

### Ventajas sobre sistemas tradicionales:

- **Procesamiento en memoria**: Reduce la latencia al mantener datos en RAM
- **Tolerancia a fallos**: Recuperación automática ante fallos de nodos
- **Escalabilidad horizontal**: Capacidad de añadir más nodos al clúster
- **Abstracción de alto nivel**: APIs simples para operaciones complejas

### Componentes de la pila unificada de Spark

La arquitectura de Spark está diseñada como una pila unificada que proporciona múltiples capacidades sobre un motor central común.

### Spark Core

Es el motor de ejecución subyacente que proporciona:

- Gestión de memoria distribuida
- Programación de tareas
- Recuperación ante fallos
- Interacción con sistemas de almacenamiento

### Spark SQL

Módulo para trabajar con datos estructurados:

- **DataFrame API**: Abstracción de datos estructurados
- **Catalyst Optimizer**: Optimizador de consultas
- **Soporte para SQL**: Sintaxis SQL estándar
- **Conectores**: Integración con diversas fuentes de datos

```sql
-- Ejemplo de consulta SQL en Spark
SELECT department, AVG(salary) as avg_salary
FROM employees
WHERE age > 25
GROUP BY department
ORDER BY avg_salary DESC
```

### Spark Streaming

Procesamiento de flujos de datos en tiempo real:

- **DStreams**: Discretized Streams para procesamiento por micro-lotes
- **Structured Streaming**: API de alto nivel para streaming
- **Integración**: Kafka, Flume, TCP sockets, etc.

### MLlib (Machine Learning Library)

Biblioteca de machine learning escalable:

- **Algoritmos**: Clasificación, regresión, clustering, filtrado colaborativo
- **Utilidades**: Evaluación de modelos, pipelines de ML
- **Optimización**: Algoritmos distribuidos para grandes datasets

### GraphX

Procesamiento de grafos y computación paralela de grafos:

- **Graph abstraction**: Representación de grafos distribuidos
- **Algoritmos**: PageRank, Connected Components, Triangle Counting
- **Transformaciones**: Operaciones sobre vértices y aristas

## Conjunto de Datos Distribuidos Resilientes (RDD)

Los RDD (Resilient Distributed Datasets) son la abstracción fundamental de datos en Spark.

### Características de los RDD:

1. **Resilientes**: Tolerantes a fallos mediante linaje de transformaciones
2. **Distribuidos**: Particionados a través del clúster
3. **Inmutables**: No se pueden modificar una vez creados
4. **Evaluación perezosa**: Las transformaciones se evalúan solo cuando se necesitan

### Creación de RDD:

```python
# Parallelizar una colección
data = [1, 2, 3, 4, 5]
rdd = sc.parallelize(data)

# Desde archivo de texto
rdd = sc.textFile("path/to/file.txt")

# Desde archivo CSV
rdd = sc.textFile("data.csv").map(lambda line: line.split(","))
```

### Operaciones sobre RDD:

**Transformaciones** (lazy evaluation):

- `map()`: Aplica una función a cada elemento
- `filter()`: Filtra elementos según una condición
- `flatMap()`: Aplica función y aplana el resultado
- `union()`: Une dos RDD
- `distinct()`: Elimina duplicados

**Acciones** (trigger evaluation):

- `collect()`: Retorna todos los elementos al driver
- `count()`: Cuenta el número de elementos
- `first()`: Retorna el primer elemento
- `take(n)`: Retorna los primeros n elementos
- `reduce()`: Agrega elementos usando una función

```python
# Ejemplo de transformaciones y acciones
numbers = sc.parallelize([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])

# Transformaciones
even_numbers = numbers.filter(lambda x: x % 2 == 0)
squared_numbers = even_numbers.map(lambda x: x ** 2)

# Acción
result = squared_numbers.collect()  # [4, 16, 36, 64, 100]
```

### Particionamiento de RDD:

El particionamiento afecta el rendimiento y la distribución de datos:

```python
# Crear RDD con particiones específicas
rdd = sc.parallelize(data, numSlices=4)

# Verificar número de particiones
print(rdd.getNumPartitions())

# Reparticionar
rdd_repartitioned = rdd.repartition(8)
```

# Descargar e instalar Spark de forma independiente

## Requisitos del sistema:

- **Java**: JDK 8 o superior
- **Scala**: 2.12.x (incluido con Spark)
- **Python**: 2.7+ o 3.4+ (para PySpark)
- **Memoria**: Al menos 2GB RAM recomendados
- **Espacio en disco**: 1GB para instalación básica

## Pasos de instalación:

1. **Descargar Spark**:

```bash
# Descargar desde el sitio oficial
wget https://downloads.apache.org/spark/spark-3.4.0/spark-3.4.0-bin-hadoop3.tgz

# Extraer
tar -xzf spark-3.4.0-bin-hadoop3.tgz

# Mover a directorio de instalación
sudo mv spark-3.4.0-bin-hadoop3 /opt/spark
```

2. **Configurar variables de entorno**:

```bash
# Agregar al ~/.bashrc o ~/.zshrc
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export PYSPARK_PYTHON=python3
```

3. **Verificar instalación**:

```bash
# Verificar versión
spark-submit --version

# Ejecutar ejemplo
run-example SparkPi 10
```

## Estructura de directorios de Spark:

```
spark/
├── bin/           # Scripts ejecutables
├── conf/          # Archivos de configuración
├── data/          # Datos de ejemplo
├── examples/      # Aplicaciones de ejemplo
├── jars/          # Archivos JAR de Spark
├── python/        # Código fuente de PySpark
├── R/             # Código fuente de SparkR
├── sbin/          # Scripts de administración del clúster
└── yarn/          # Archivos para integración con YARN
```

## Descripción general de Scala y Python

### Scala en Spark

Scala es el lenguaje nativo de Spark y ofrece el mejor rendimiento:

**Ventajas**:

- Rendimiento óptimo (sin overhead)
- Acceso completo a la API de Spark
- Compilación estática reduce errores
- Interoperabilidad con Java

**Sintaxis básica**:

```scala
// Variables
val immutableVar = "Hello"
var mutableVar = "World"

// Funciones
def add(x: Int, y: Int): Int = x + y

// Colecciones
val numbers = List(1, 2, 3, 4, 5)
val doubled = numbers.map(_ * 2)

// Pattern matching
val message = numbers.length match {
  case 0 => "Empty list"
  case 1 => "Single element"
  case _ => "Multiple elements"
}
```

### Python en Spark (PySpark)

Python ofrece facilidad de uso y gran ecosistema:

**Ventajas**:

- Sintaxis simple y legible
- Gran ecosistema de librerías
- Popularidad en ciencia de datos
- Integración con pandas, numpy, matplotlib

**Configuración de PySpark**:

```python
from pyspark.sql import SparkSession
from pyspark import SparkContext, SparkConf

# Crear SparkSession
spark = SparkSession.builder \
    .appName("MyApp") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()

# Obtener SparkContext
sc = spark.sparkContext
```

### Comparación de rendimiento:

|Aspecto|Scala|Python|
|---|---|---|
|Velocidad|Excelente|Bueno (con overhead)|
|Facilidad|Moderado|Excelente|
|Ecosistema|Java/Scala|Python científico|
|Debugging|Compilación|Runtime|
|Curva aprendizaje|Pronunciada|Suave|

# Lanzar y usar el shell de Scala y Python de Spark

## Spark Shell (Scala)

El shell de Scala proporciona una interfaz interactiva REPL:

```bash
# Lanzar shell de Scala
spark-shell

# Con configuraciones específicas
spark-shell --master local[4] --driver-memory 2g
```

**Comandos básicos en el shell**:

```scala
// SparkContext está disponible como 'sc'
// SparkSession está disponible como 'spark'

// Crear RDD
val data = sc.parallelize(1 to 100)

// Operaciones
val filtered = data.filter(_ % 2 == 0)
val result = filtered.collect()

// Leer archivo
val textFile = sc.textFile("README.md")
val wordCounts = textFile.flatMap(_.split(" "))
                        .map((_, 1))
                        .reduceByKey(_ + _)
```

## PySpark Shell

El shell de Python ofrece interfaz interactiva similar:

```bash
# Lanzar PySpark shell
pyspark

# Con configuraciones específicas
pyspark --master local[4] --driver-memory 2g
```

**Comandos básicos en PySpark**:

```python
# SparkContext disponible como 'sc'
# SparkSession disponible como 'spark'

# Crear RDD
data = sc.parallelize(range(1, 101))

# Operaciones
filtered = data.filter(lambda x: x % 2 == 0)
result = filtered.collect()

# Leer archivo
text_file = sc.textFile("README.md")
word_counts = text_file.flatMap(lambda line: line.split(" ")) \
                      .map(lambda word: (word, 1)) \
                      .reduceByKey(lambda a, b: a + b)
```

## Jupyter Notebook con PySpark

Configurar PySpark para trabajar con Jupyter:

```python
# Instalar findspark
pip install findspark

# En el notebook
import findspark
findspark.init()

import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("JupyterApp").getOrCreate()
```

## Comandos útiles del shell:

```scala
// Scala shell
:help          // Ayuda
:quit          // Salir
:type expr     // Mostrar tipo de expresión
:load file     // Cargar archivo Scala
```

```python
# Python shell
help()         # Ayuda
exit()         # Salir
%timeit        # Medir tiempo de ejecución (en Jupyter)
```


# Conjunto de Datos Distribuidos Resilientes y DataFrames

## Entender cómo crear colecciones paralelizadas y conjuntos de datos externos

### Colecciones Paralelizadas

Las colecciones paralelizadas permiten convertir colecciones locales en RDD distribuidos:

```python
# Crear RDD desde listas
numbers = sc.parallelize([1, 2, 3, 4, 5])
strings = sc.parallelize(["hello", "world", "spark"])

# Especificar número de particiones
data = sc.parallelize(range(1000), numSlices=10)

# Desde tuplas
pairs = sc.parallelize([("a", 1), ("b", 2), ("c", 3)])
```

En escala:

```scala
val numbers = sc.parallelize(List(1, 2, 3, 4, 5))
val data = sc.parallelize(1 to 1000, numSlices = 10)
```

### Control de particionamiento:

```python
# Verificar particiones
print(f"Número de particiones: {rdd.getNumPartitions()}")

# Ver contenido de cada partición
def print_partition_content(index, iterator):
    print(f"Partición {index}: {list(iterator)}")

rdd.mapPartitionsWithIndex(print_partition_content).collect()
```

## Conjuntos de Datos Externos

Spark puede leer datos de múltiples fuentes externas:

### Archivos de texto:

```python
# Archivo único
text_rdd = sc.textFile("file:///path/to/file.txt")

# Múltiples archivos
text_rdd = sc.textFile("file:///path/to/directory/*.txt")

# Desde HDFS
hdfs_rdd = sc.textFile("hdfs://namenode:port/path/to/file")

# Desde S3
s3_rdd = sc.textFile("s3a://bucket/path/to/file")
```

### Archivos estructurados:

```python
# JSON
json_rdd = sc.textFile("data.json").map(lambda x: json.loads(x))

# CSV (manualmente)
csv_rdd = sc.textFile("data.csv") \
           .map(lambda line: line.split(",")) \
           .filter(lambda row: len(row) > 1)

# Parquet (usando DataFrame)
df = spark.read.parquet("data.parquet")
parquet_rdd = df.rdd
```

### Bases de datos:

```python
# JDBC
df = spark.read \
    .format("jdbc") \
    .option("url", "jdbc:postgresql://localhost/test") \
    .option("dbtable", "employees") \
    .option("user", "username") \
    .option("password", "password") \
    .load()

jdbc_rdd = df.rdd
```

### Configuraciones avanzadas:

```python
# Configurar compresión
spark.conf.set("spark.sql.files.compressionCodecName", "gzip")

# Configurar encoding
text_rdd = sc.textFile("file.txt", use_unicode=True)

# Mínimo de particiones
text_rdd = sc.textFile("file.txt", minPartitions=8)
```

## Trabajar con operaciones de Conjunto de Datos Distribuidos Resilientes (RDD)

### Transformaciones comunes

Las transformaciones crean nuevos RDD sin ejecutar inmediatamente:

```python
# map: Aplica función a cada elemento
numbers = sc.parallelize([1, 2, 3, 4, 5])
squared = numbers.map(lambda x: x ** 2)  # [1, 4, 9, 16, 25]

# filter: Filtra elementos según condición
evens = numbers.filter(lambda x: x % 2 == 0)  # [2, 4]

# flatMap: Aplica función y aplana resultado
words = sc.parallelize(["hello world", "spark is great"])
all_words = words.flatMap(lambda line: line.split(" "))
# ["hello", "world", "spark", "is", "great"]

# distinct: Elimina duplicados
data = sc.parallelize([1, 2, 2, 3, 3, 3])
unique = data.distinct()  # [1, 2, 3]

# union: Une dos RDD
rdd1 = sc.parallelize([1, 2, 3])
rdd2 = sc.parallelize([4, 5, 6])
combined = rdd1.union(rdd2)  # [1, 2, 3, 4, 5, 6]

# intersection: Intersección de RDD
rdd1 = sc.parallelize([1, 2, 3, 4])
rdd2 = sc.parallelize([3, 4, 5, 6])
common = rdd1.intersection(rdd2)  # [3, 4]

# subtract: Diferencia entre RDD
diff = rdd1.subtract(rdd2)  # [1, 2]
```

### Transformaciones de agrupación:

```python
# groupBy: Agrupa elementos por clave
data = sc.parallelize([1, 2, 3, 4, 5, 6])
grouped = data.groupBy(lambda x: x % 2)  # Agrupar por par/impar

# sample: Muestra aleatoria
sample_data = data.sample(withReplacement=False, fraction=0.5, seed=42)

# coalesce: Reduce particiones sin shuffle
coalesced = data.coalesce(2)

# repartition: Repartíciona con shuffle
repartitioned = data.repartition(10)
```

### Acciones principales

Las acciones triggean la evaluación y retornan resultados:

```python
# collect: Retorna todos los elementos
result = numbers.collect()

# count: Cuenta elementos
total = numbers.count()

# first: Primer elemento
first_elem = numbers.first()

# take: Primeros n elementos
first_three = numbers.take(3)

# takeSample: Muestra aleatoria
sample = numbers.takeSample(withReplacement=False, num=3)

# reduce: Agrega elementos
sum_all = numbers.reduce(lambda a, b: a + b)

# fold: Como reduce pero con valor inicial
sum_with_initial = numbers.fold(0, lambda a, b: a + b)

# aggregate: Agregación más flexible
def seq_op(acc, value):
    return (acc[0] + value, acc[1] + 1)

def comb_op(acc1, acc2):
    return (acc1[0] + acc2[0], acc1[1] + acc2[1])

sum_count = numbers.aggregate((0, 0), seq_op, comb_op)
average = sum_count[0] / sum_count[1]
```

### Operaciones de guardado:

```python
# Guardar como archivo de texto
numbers.saveAsTextFile("output/numbers")

# Guardar como archivo de secuencia
pairs = numbers.map(lambda x: (x, x**2))
pairs.saveAsSequenceFile("output/pairs")

# Guardar con compresión
numbers.saveAsTextFile("output/compressed", 
                      compressionCodecClass="org.apache.hadoop.io.compress.GzipCodec")
```

## Utilizar variables compartidas y pares clave-valor

### Variables Broadcast

Las variables broadcast permiten compartir eficientemente datos de solo lectura:

```python
# Crear variable broadcast
lookup_table = {"A": 1, "B": 2, "C": 3}
broadcast_lookup = sc.broadcast(lookup_table)

# Usar en transformaciones
def map_values(x):
    return broadcast_lookup.value.get(x, 0)

data = sc.parallelize(["A", "B", "C", "D"])
mapped = data.map(map_values)  # [1, 2, 3, 0]

# Limpiar variable broadcast
broadcast_lookup.unpersist()
```

### Acumuladores

Los acumuladores permiten agregar información de los workers:

```python
# Crear acumulador
counter = sc.accumulator(0)

def process_line(line):
    global counter
    if "ERROR" in line:
        counter.add(1)
    return line.upper()

# Usar en transformaciones
log_lines = sc.textFile("log.txt")
processed = log_lines.map(process_line)

# Ejecutar acción para activar acumulador
processed.count()

print(f"Líneas con ERROR: {counter.value}")
```

### Acumuladores personalizados:

```python
from pyspark.util import AccumulatorParam

class ListAccumulatorParam(AccumulatorParam):
    def zero(self, value):
        return []
    
    def addInPlace(self, list1, list2):
        list1.extend(list2)
        return list1

# Usar acumulador personalizado
list_accum = sc.accumulator([], ListAccumulatorParam())

def collect_errors(line):
    if "ERROR" in line:
        list_accum.add([line])
    return line

log_lines.map(collect_errors).count()
print(f"Errores encontrados: {list_accum.value}")
```

### Pair RDD (Pares Clave-Valor)

Los Pair RDD son RDD que contienen tuplas de clave-valor:

```python
# Crear Pair RDD
pairs = sc.parallelize([("a", 1), ("b", 2), ("c", 3)])

# Desde datos existentes
data = sc.parallelize(["hello", "world", "hello", "spark"])
word_pairs = data.map(lambda word: (word, 1))
```

### Transformaciones específicas de Pair RDD:

```python
# reduceByKey: Reduce valores por clave
word_counts = word_pairs.reduceByKey(lambda a, b: a + b)

# groupByKey: Agrupa valores por clave
grouped = word_pairs.groupByKey()

# mapValues: Aplica función solo a valores
doubled_values = pairs.mapValues(lambda x: x * 2)

# keys y values: Extrae claves o valores
all_keys = pairs.keys()
all_values = pairs.values()

# sortByKey: Ordena por clave
sorted_pairs = pairs.sortByKey()

# join: Une dos Pair RDD
rdd1 = sc.parallelize([("a", 1), ("b", 2)])
rdd2 = sc.parallelize([("a", 10), ("b", 20)])
joined = rdd1.join(rdd2)  # [("a", (1, 10)), ("b", (2, 20))]

# leftOuterJoin y rightOuterJoin
left_joined = rdd1.leftOuterJoin(rdd2)
right_joined = rdd1.rightOuterJoin(rdd2)

# cogroup: Agrupa valores de múltiples RDD
cogrouped = rdd1.cogroup(rdd2)
```

### Particionamiento para Pair RDD:

```python
# Particionar por hash
hash_partitioned = pairs.partitionBy(4)

# Particionar por rango (requiere RDD ordenado)
range_partitioned = pairs.sortByKey().partitionBy(4)

# Verificar particionador
print(hash_partitioned.partitioner)

# Persist para reutilizar particionamiento
hash_partitioned.persist()
```

### Análisis de logs

```python
# Cargar logs
logs = sc.textFile("access.log")

# Extraer IP addresses
ips = logs.map(lambda line: line.split()[0])

# Contar accesos por IP
ip_counts = ips.map(lambda ip: (ip, 1)) \
              .reduceByKey(lambda a, b: a + b)

# Top 10 IPs
top_ips = ip_counts.takeOrdered(10, key=lambda x: -x[1])

# Guardar resultados
ip_counts.saveAsTextFile("output/ip_analysis")
```


# Programación de aplicaciones Spark

## Entender el propósito y uso de SparkContext

### ¿Qué es SparkContext?

SparkContext es el punto de entrada principal para todas las funcionalidades de Spark. Representa la conexión al clúster de Spark y se usa para crear RDD, acumuladores y variables broadcast.

### Responsabilidades del SparkContext:

1. **Configuración**: Establecer parámetros de la aplicación
2. **Conexión al clúster**: Coordinar con el gestor de clúster
3. **Distribución de tareas**: Enviar trabajos a los executors
4. **Gestión de recursos**: Administrar memoria y CPU
5. **Tolerancia a fallos**: Manejar fallos de nodos

### Arquitectura con SparkContext:

```
Driver Program
├── SparkContext
│   ├── Cluster Manager (YARN/Mesos/Standalone)
│   └── Executors
│       ├── Task 1
│       ├── Task 2
│       └── Cache/Storage
```

### Crear SparkContext:

```python
from pyspark import SparkContext, SparkConf

# Configuración básica
conf = SparkConf().setAppName("MyApp").setMaster("local[2]")
sc = SparkContext(conf=conf)

# Con configuraciones específicas
conf = SparkConf() \
    .setAppName("AdvancedApp") \
    .setMaster("local[4]") \
    .set("spark.executor.memory", "2g") \
    .set("spark.executor.cores", "2") \
    .set("spark.sql.adaptive.enabled", "true")

sc = SparkContext(conf=conf)
```

### SparkSession vs SparkContext:

```python
# SparkSession (recomendado para Spark 2.0+)
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()

# Obtener SparkContext desde SparkSession
sc = spark.sparkContext
```

### Propiedades importantes del SparkContext:

```python
# Información del contexto
print(f"App Name: {sc.appName}")
print(f"Spark Version: {sc.version}")
print(f"Master URL: {sc.master}")
print(f"Default Parallelism: {sc.defaultParallelism}")

# Configuraciones
all_configs = sc.getConf().getAll()
for key, value in all_configs:
    print(f"{key}: {value}")

# Estado del contexto
print(f"Is stopped: {sc._jsc.sc().isStopped()}")
```

## Inicializar Spark con los diversos lenguajes de programación

### Python (PySpark)

```python
# Instalación
pip install pyspark

# Inicialización básica
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PythonSparkApp") \
    .master("local[*]") \
    .config("spark.driver.memory", "2g") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()

sc = spark.sparkContext

# Ejemplo de uso
data = sc.parallelize([1, 2, 3, 4, 5])
result = data.map(lambda x: x * 2).collect()
print(result)

# Cerrar sesión
spark.stop()
```

### Scala

```scala
import org.apache.spark.{SparkConf, SparkContext}
import org.apache.spark.sql.SparkSession

object ScalaSparkApp {
  def main(args: Array[String]): Unit = {
    // Configuración
    val conf = new SparkConf()
      .setAppName("ScalaSparkApp")
      .setMaster("local[*]")
    
    val sc = new SparkContext(conf)
    
    // O usar SparkSession
    val spark = SparkSession.builder()
      .appName("ScalaSparkApp")
      .master("local[*]")
      .getOrCreate()
    
    // Ejemplo de uso
    val data = sc.parallelize(List(1, 2, 3, 4, 5))
    val result = data.map(_ * 2).collect()
    println(result.mkString(", "))
    
    // Cerrar
    sc.stop()
    // spark.stop()
  }
}
```

### Java

```java
import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.sql.SparkSession;
import java.util.Arrays;
import java.util.List;

public class JavaSparkApp {
    public static void main(String[] args) {
        // Configuración
        SparkConf conf = new SparkConf()
            .setAppName("JavaSparkApp")
            .setMaster("local[*]");
        
        JavaSparkContext sc = new JavaSparkContext(conf);
        
        // O usar SparkSession
        SparkSession spark = SparkSession.builder()
            .appName("JavaSparkApp")
            .master("local[*]")
            .getOrCreate();
        
        // Ejemplo de uso
        List<Integer> data = Arrays.asList(1, 2,
```