## ¿Qué es una Base de Datos?

Una **base de datos** es un conjunto organizado de información o datos estructurados, típicamente almacenados electrónicamente en un sistema informático. Las bases de datos permiten el almacenamiento, modificación y acceso eficiente a los datos.

- Ejemplos: sistemas bancarios, plataformas de streaming, redes sociales.
- Tipos:
  - Relacionales (SQL)
  - No relacionales (NoSQL)

## ¿Qué es SQL?

**SQL (Structured Query Language)** es un lenguaje estándar para el manejo de bases de datos relacionales. Permite realizar operaciones como:

- Crear estructuras (tablas, vistas)
- Insertar, consultar, actualizar y eliminar datos (CRUD)
- Controlar acceso a los datos

## ¿Qué es MySQL?

**MySQL** es un sistema de gestión de bases de datos relacional (RDBMS) de código abierto basado en SQL. Es ampliamente usado por su velocidad, confiabilidad y facilidad de uso.

- Licencia dual: GPL (gratuita) y comercial (Oracle)
- Muy utilizado en aplicaciones web

## Descarga e Instalación de MySQL

### Instalación en Windows

Tutorial:  
[https://www.youtube.com/watch?v=tPPFPC86XnQ](https://www.youtube.com/watch?v=tPPFPC86XnQ)

### Instalación en Linux (Ubuntu/Debian)

Tutorial:
[https://www.youtube.com/watch?v=KM2y_BeDxGg&t=450s](https://www.youtube.com/watch?v=KM2y_BeDxGg&t=450s)

## ¿Qué es MySQL Workbench?

**MySQL Workbench** es una herramienta visual para administración y diseño de bases de datos MySQL.

### Funcionalidades:

- Modelado de bases de datos (ERD)
- Ejecución de consultas SQL
- Administración de usuarios y permisos
- Backups y monitoreo

## Leer la base de datos Sakila

**Sakila** es una base de datos de ejemplo que simula un videoclub. Es ideal para practicar consultas SQL reales.

### Consultas de ejemplo:

```sql
-- Ver todas las películas
SELECT * FROM film;

-- Listar películas y su categoría
SELECT f.title, c.name 
FROM film f
JOIN film_category fc ON f.film_id = fc.film_id
JOIN category c ON fc.category_id = c.category_id;
```


## Lenguaje de Definición de Datos (DDL)

El **DDL (Data Definition Language)** permite definir, modificar y eliminar la estructura de las bases de datos (tablas, columnas, llaves, etc.).

### Tipos de Datos en SQL

SQL ofrece varios tipos de datos. Aquí algunos comunes:

#### Numéricos:
- `INT` / `INTEGER`: Entero (e.g., 10, 245)
- `DECIMAL(p,s)` / `NUMERIC`: Decimales exactos (e.g., 10.50)
- `FLOAT`, `DOUBLE`: Reales de punto flotante

#### Texto:
- `CHAR(n)`: Cadena de longitud fija
- `VARCHAR(n)`: Cadena de longitud variable
- `TEXT`: Texto largo

#### Fechas y Tiempos:
- `DATE`: Fecha (YYYY-MM-DD)
- `DATETIME`: Fecha y hora (YYYY-MM-DD HH:MM:SS)
- `TIMESTAMP`: Marca de tiempo

#### Booleano:
- `BOOLEAN` / `TINYINT(1)`: Verdadero (1) o Falso (0)

### ¿Qué es una Primary Key?

Una **Primary Key** (clave primaria) identifica de forma única cada fila de una tabla.

- No puede ser `NULL`
- Debe ser única
- Una tabla solo puede tener una clave primaria

```sql
CREATE TABLE empleados (
    id INT PRIMARY KEY,
    nombre VARCHAR(100)
);
```

### ¿Qué es una Foreign Key?

Una **Foreign Key** (clave foránea) establece una relación entre dos tablas.

- Enlaza la columna de una tabla con la clave primaria de otra
- Ayuda a mantener la integridad referencial

```sql
CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    empleado_id INT,
    FOREIGN KEY (empleado_id) REFERENCES empleados(id)
);
```

### Agregar y Quitar Columnas

Agregar columna:

```sql
ALTER TABLE empleados ADD correo VARCHAR(100);
```

Quitar columna:

```sql
ALTER TABLE empleados DROP COLUMN correo;
```

### Eliminar Tablas

Elimina la tabla completa **y sus datos**:

```sql
DROP TABLE empleados;
```

### Truncar Tablas

Elimina todos los registros de una tabla **pero conserva la estructura**:

```sql
TRUNCATE TABLE empleados;
```

`TRUNCATE` es más rápido que `DELETE` sin `WHERE`, pero no se puede deshacer (no activa triggers en muchas bases de datos).


## ALTER TABLE

### Modificar la estructura de una tabla

El comando `ALTER TABLE` permite modificar la estructura de una tabla existente sin eliminarla. Aquí se detallan varios usos importantes.

### Agregar y Quitar PRIMARY KEY

Agregar PRIMARY KEY:

```sql
ALTER TABLE empleados ADD PRIMARY KEY (id);
```

Solo si la columna ya existe y cumple con los requisitos: única y no nula.

Quitar PRIMARY KEY:

```sql
ALTER TABLE empleados DROP PRIMARY KEY;
```

Solo si no existe una Foreign Key que dependa de ella.

### Agregar y Quitar FOREIGN KEY

Agregar FOREIGN KEY:

```sql
ALTER TABLE pedidos 
ADD CONSTRAINT fk_empleado 
FOREIGN KEY (empleado_id) 
REFERENCES empleados(id);
```

`fk_empleado` es un nombre simbólico para la relación.

```sql
ALTER TABLE pedidos 
ADD CONSTRAINT fk_empleado 
FOREIGN KEY (empleado_id) 
REFERENCES empleados(id);
```

Quitar FOREIGN KEY:

```sql
ALTER TABLE pedidos DROP FOREIGN KEY fk_empleado;
```

Debes conocer el nombre de la clave foránea (`SHOW CREATE TABLE pedidos;` puede ayudarte).

### Agregar UNIQUE Constraint

El `UNIQUE` constraint asegura que todos los valores en una columna (o combinación de columnas) sean únicos.

```sql
ALTER TABLE empleados ADD CONSTRAINT unique_correo UNIQUE (correo);
```

Puedes nombrar la restricción (`unique_correo`) o dejar que el sistema lo haga automáticamente.

### Cambiar el nombre de una columna

En MySQL:

```sql
ALTER TABLE empleados CHANGE nombre nombre_completo VARCHAR(100);
```

Se debe especificar el **tipo de dato** de nuevo, aunque no cambie.

### Cambiar el data type de una columna

```sql
ALTER TABLE empleados MODIFY nombre_completo TEXT;
```

En `MODIFY`, se conserva el nombre pero se cambia el tipo.


## Lenguaje de Manipulación de Datos (DML)

El **DML (Data Manipulation Language)** permite trabajar directamente con los datos dentro de las tablas: insertar, actualizar y eliminar registros.

### Insertar datos en una tabla

Se utiliza el comando `INSERT INTO`.

#### Insertar valores en todas las columnas:

```sql
INSERT INTO personas (id, nombre, email)
VALUES (1, 'Carlos Pérez', 'carlos@mail.com');
```

Insertar varios registros:

```sql
INSERT INTO personas (id, nombre, email)
VALUES 
(2, 'Ana Torres', 'ana@mail.com'),
(3, 'Luis Gómez', 'luis@mail.com');
```

### Actualizar datos de una tabla

Se utiliza el comando `UPDATE` junto con `SET` y una cláusula `WHERE` (para evitar afectar todas las filas).

Actualizar una fila específica:

```sql
UPDATE personas
SET email = 'nuevo_correo@mail.com'
WHERE id = 1;
```

Actualizar múltiples columnas:

```sql
UPDATE personas
SET nombre = 'Carlos P. Díaz', email = 'carlos.diaz@mail.com'
WHERE id = 1;
```

Si omites el `WHERE`, se actualizan **todas** las filas de la tabla.

### Borrar datos de una tabla

Se utiliza el comando `DELETE`.

Borrar una fila específica:

```sql
DELETE FROM personas
WHERE id = 2;
```

Borrar todas las filas:

```sql
DELETE FROM personas;
```

Esto elimina todos los registros, pero **no** la estructura de la tabla (para eso se usa `TRUNCATE`).