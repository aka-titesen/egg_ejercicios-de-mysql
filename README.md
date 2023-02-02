# MySQL

## Temas

- CRUD

- Consultas multitablas implicitas y explicitas 

- COUNT, ORDER BY, GROUP BY, etc.

- Tipos de JOINS

## Comandos para crear, modificar y eliminar una base de datos

### Crea una base de datos

```sql
 CREATE SCHEMA `egg`; 
```

### Crea una tabla

```sql
CREATE TABLE `egg`.`curso_programacion` (
    `nombre_curso` VARCHAR(255) NOT NULL,
    `cantidad_alumno` INT NULL);
```

### Selecciona la base de datos egg

```sql
USE egg 
```

### Elimina la tabla

```sql
drop table curso_programacion;
```

### Elimina la base de datos

```sql
drop schema egg; 
```

### Modifica una columna de la base de datos

```sql
ALTER TABLE `egg`.`curso_de_programacion`
CHANGE COLUMN `cruso_programacion` `curso` VARCHAR(255) NOT NULL;
```

### Ejemplo de un Script

```sql
CREATE DATABASE tienda CHARACTER SET utf8mb4;

Use tienda

CREATE TABLE fabricante (
  codigo INT UNSIGNED AUTO_INCRENENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

INSERT INTO fabricante VALUES (1,'Asus');
INSERT INTO fabricante VALUES (2,'Lenovo');
INSERT INTO fabricante VALUES (3,'Hewlett-Packard');
INSERT INTO fabricante VALUES (4,'Samgung');
INSERT INTO fabricante VALUES (5,'Seagate');
INSERT INTO fabricante VALUES (6,'Crucial');
INSERT INTO fabricante VALUES (7,'Gigabyte');
INSERT INTO fabricante VALUES (8,'Huawei');
INSERT INTO fabricante VALUES (9,'Xiaomi');
```

## Comandos para consultar, insertar, actualizar y eliminar registros en una tabla

### Consulta

```sql
SELECT * FROM fabricante;
```

### Insertas valores en una tabla

```sql
INSERT INTO FABRICANTE (CODIGO, NOMBRE) VALUES (10 , 'HP');
INSERT INTO FABRICANTE (CODIGO, NOMBRE) VALUES (11, 'LG');
```

### Actualizar valores  de un registro en una tabla

```sql
UPDATE FABRICANTE
SET NOMBRE = 'HP2'
WHERE CODIGO = 10;
```

### Eliminar registro en una tabla

```sql
DELETE
FROM FABRICANTE    
WHERE CODIGO = 10;
```

## El comando Where

> <u>La clausula más importante.
> </u>La cláusula <mark>SELECT </mark>en ella vamos a poder determinar mediante la cláusula <mark>FROM </mark>qué tablas queremos consultar y mediante la cláusula <mark>WHERE </mark>qué condición o qué criterio vamos a utilizar para extraer nuestra información

### Extrae todos los registros de la tabla fabricante

```sql
 SELECT * FROM fabricante;
```

### Extrae al fabricante con la id 3

```sql
 SELECT * FROM fabricante WHERE codigo = 3;
```

### Extrae el nombre del fabricante con la id 5

```sql
SELECT nombre FROM fabricante WHERE codigo = 5;
```

### Si queremos extraer la información de varias columnas

```sql
SELECT codigo, nombre FROM fabricante WHERE codigo = 5;
SELECT codigo, nombre, nacionalidad FROM fabricante WHERE codigo = 5;  
```

### Extrae todos los fabricante con un código mayor y menor que 7

```sql
SELECT * FROM fabricante WHERE codigo > 7;
SELECT * FROM fabricante WHERE codigo < 7;
```

### Extrae el fabricante con el nombre LENOVO

```sql
SELECT * FROM fabricante WHERE nombre = 'Lenovo';
```

### Extrae todos los fabricantes de CHINA

```sql
SELECT * FROM fabricante WHERE nacionalidad = 'China';
```

### Extrae los fabricantes ASUS pero de CHINA

```sql
SELECT * FROM fabricante WHERE nacionalidad = 'China' AND nombre = 'Asus';
```

### Extrae los fabricantes de Japon o Corea del Sur

```sql
SELECT * FROM fabricante WHERE nacionalidad = 'Japon' OR nacionalidad = 'Corea del sur';
```

### Extrae la cantidad de registros de la tabla fabricante

```sql
SELECT COUNT(*) FROM fabricante;
SELECT COUNT(*) as 'cantidad' FROM fabricante;
```

### Contar la cantidad de fabricantes de JAPON

```sql
SELECT COUNT(*) as 'cantidad' FROM fabricante WHERE nacionalidad = 'Japon';
```

### Extrae el máximo y mínimo código de un fábricante

```sql
SELECT MAX(codigo) FROM fabricante;
SELECT MIN(codigo) FROM fabricante;
```

### Extrae los fabricantes entre 2 códigos de un extremo inferior al superior

```sql
SELECT * FROM fabricante WHERE codigo IN (3,5);
```

### Extrae ciertos registros que tengan de carácteres en el medio "su"

```sql
SELECT * FROM fabricante WHERE nombre LIKE '%su%';
```

### Extrae ciertos registros que comiencen con los carácteres "su"

```sql
SELECT * FROM fabricante WHERE nombre LIKE 'su%';
```

### Extrae ciertos registros que finlacen con los carácteres "su"

```sql
SELECT * FROM fabricante WHERE nombre LIKE '%su';
```

### Extrae un rango de registros

```sql
SELECT * FROM fabricante WHERE codigo BETWEEN 2 AND 7;
```

## Consultas multitablas

### Ejemplo de como crear una clave foranea

#### Crea la tabla producto

```sql
CREATE TABLE producto (
  codigo INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  precio DOUBLE NOT NULL,
  codigo_fabricante INT UNSIGNED NOT NULL, -- Éste atributo se convertirá en clave foranea y hará referencia al atributo codigo de la tabla fabricante
  FOREIGN KEY (codigo_fabricante) REFERENCES fabricante(codigo) -- Acá le digo que será clave foranea y hará referencia a fabricante
);
```

### Consulta con 2 tablas

1. En el **FROM **selecciona las 2 tablas, a **fabricante **le asigna el **alias **<mark>f</mark> y a **producto  **<mark>p</mark>

2. En el **SELECT **mostraremos la columa nombre de fabricante con el alias fabricante y el de producto con el alias producto, y finalmente mostraremos el precio de producto

3. En el **WHERE **mostraremos básicamente todos los registros, ya que la condición es que la clave primaria del fabricante sea igual a la clave foranea 'codigo_fabricante' en producto sean iguales. Es decir mostramos productos que estén relacionados con fabricantes y viceversa.

```sql
SELECT f.nombre AS 'fabricante', p.nombre AS 'producto' , p.precio
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante;
```

### Extrae todos los productos del fábricante con el código 7

#### En el select mostramos diferentes informacion

```sql
SELECT f.nombre AS 'fabricante', p.nombre AS 'producto' , p.precio
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
AND f.codigo = 7;

SELECT f.nombre AS 'fabricante', f.codigo AS 'codigo'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
AND f.codigo = 7;

SELECT p.codigo AS 'Codigo producto', p.nombre AS 'Nombre del producto', p.precio AS 'Precio del producto'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
AND f.codigo = 7;
```

### Extrae todos productos del fabricante de nombre LENOVO

```sql
SELECT p.codigo AS 'Codigo producto', p.nombre AS 'Nombre del producto', p.precio AS 'Precio del producto'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
AND f.nombre = 'Lenovo';
```

### Extrae todos productos del fabricante LENOVO donde sus precios sean mayores a $450

```sql
SELECT p.codigo AS 'Codigo producto', p.nombre AS 'Nombre del producto', p.precio AS 'Precio del producto'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
AND f.nombre = 'Lenovo'
AND p.precio > 450;
```

## Tipos de Joins

### Inner Join

> Extraer todos los productos que tienen una relación con un fabricante.<u> En la intersección de ambas tablas nos trae los valores con relación</u>

```sql
SELECT *
FROM producto p
INNER JOIN fabricante f
ON p.codigo_fabricante = f.codigo
```

### Left Join

> Extrae todos los productos que se indican en el FROM más los valores de la intersección que es fábricante. <u>Mostrará todos los productos aunque no tengan fabricantes</u>. Otra forma de decirlo es que traerá SOLO los fabricantes que estén relacionados con productos

```sql
SELECT *
FROM producto p
LEFT OUTER JOIN fabricante f
ON p.codigo_fabricante = f.codigo
```

### Right Join

> Extrae todos los fabricantes que se indican en el RIGHT OUTER JOIN más los valores de la intersección que es productos. <u>Mostrará todos los fabricantes aunque no tengan productos</u>. Otra forma decirlo es que traerá SOLO los productos que estén relacionados con fabricante

```sql
SELECT *
FROM producto p
RIGHT OUTER JOIN fabricante f
ON p.codigo_fabricante = f.codigo
```

### Semi Join (Exists)

> Nos permite saber si un producto existe para un fabricante. Nos traerá todos los productos relacionados con un fabricante

```sql
SELECT *
FROM producto p
WHERE EXISTS (SELECT f.codigo
            FROM fabricante f
            WHERE f.codigo = p.codigo_fabricante
            );
```

### Anti Semi Join (Not exists)

> Nos permite saber si un producto no existe para un fabricante. Nos traerá todos los productos que no están relacionados con un fabricante

```sql
SELECT *
FROM producto p
WHERE NOT EXISTS (SELECT f.codigo
            FROM fabricante f
            WHERE f.codigo = p.codigo_fabricante
            );
```

### Cross Join

> Nos multiplica cada producto por cada fabricante, es decir; cada producto tendrá n cantidad de fabricantes

```sql
SELECT *
FROM producto p
CROSS JOIN fabricante f;
```

## Funciones

### Order by

> Nos permite ordenar una consulta en base a una columna. Ordena de manera ascendente 

```sql
SELECT * FROM fabricante ORDER BY nombre asc;
```

#### Ordena de manera descendente

```sql
SELECT *
FROM fabricante
ORDER BY nombre desc;
```

#### Con más criterios

```sql
SELECT * FROM fabricante ORDER BY nombre, codigo;
```

#### Ordenar en un consulta multitabla implicita

```sql
SELECT f.nombre, p.nombre, p.precio
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
ORDER BY f.nombre asc, p.nombre asc;

SELECT f.nombre, p.nombre, p.precio
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
ORDER BY p.nombre asc, f.nombre asc;
```

### Group by

> Permite agrupar una consulta. Agrupar los fabricantes que tienen productos

```sql
SELECT f.nombre
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nombre
```

#### Agrupamos por nacionalidad y nombre del fabricante

```sql
SELECT f.nacionalidad, f.nombre
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nacionalidad, f.nombre;
```

#### Agrupamos y contamos por nombre de fabricante

```sql
SELECT f.nombre, COUNT(*) AS 'cantidad'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nombre
```

#### Agrupamos y contamos por nacionalidad de fabricante

```sql
SELECT f.nacionalidad, COUNT(*) AS 'cantidad'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nacionalidad
```

## Having

> Con éste podemos introducir criterios de ordenación. <u>Contamos y agrupamos los fabricantes que tienen más de un producto</u> 

```sql
SELECT f.nombre, COUNT(*) AS 'cantidad' 
FROM fabricante f, producto p 
WHERE f.codigo = p.codigo_fabricante 
GROUP BY f.nombre 
HAVING COUNT(*) > 1;
```

#### Contamos y agrupamos los fabricantes que tienen más de un producto que no sean LENOVO

```sql
SELECT f.nombre, COUNT(*) AS 'cantidad'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nombre
HAVING COUNT(*) > 1 AND f.nombre <> 'Lenovo';
```

#### Contamos, agrupamos los fabricantes, sumaremos todos los productos y solo mostraremos la suma de los productos de los fabricantes que sean mayores a $300

> Es decir que la suma de todos los productos de un fabricante sea mayor a $300 será el requisito para que la consulta lo muestre

```sql
SELECT f.nombre, COUNT(*) AS 'cantidad', SUM(p.precio) AS 'total de produtos'
FROM fabricante f, producto p
WHERE f.codigo = p.codigo_fabricante
GROUP BY f.nombre
HAVING SUM(p.precio) > 300;
```

## Índices

### Agregar un índice a una tabla ya creada

> En éste caso agregaremos un índice a la tabla nombre

```sql
ALTER TABLE `ExampleOfADatabase`.`ExampleOfATable` ADD INDEX(name);
```

> También podemos delimitar los carácteres que tome un índice pero no es recomendable, ya que varios registros podrían dar coincidencia más si es una base de datos de gran tamaño y obligaría a secuenciar uno por uno perdiendo rendimiento

```sql
ALTER TABLE `ExampleOfADatabase`.`ExampleOfATable` ADD INDEX(name(20));
```

> Si encuentra un mónton de registros con victor hernan arana, tendría que recorrer toda la tabla. Por ejemplo, Victor Hernan Arana Flores, Victor Hernan Arana Falcon, Victor Hernan Arana Gonzalez, Victor Hernan Arana Miralles 

### Agregar un índice a una tabla que vamos a crear

```sql
CREATE TABLE `ExampleOfADatabase`.`Contacts` (
    `id` INT NOT NULL AUTO_INCREMENT,
    `nombre` VARCHAR(64) NOT NULL,
    `apellidos` VARCHAR(64) NOT NULL,
    `direccion` VARCHAR(255) NOT NULL,
    `telefono` VARCHAR(9) NOT NULL,
    `correo` VARCHAR(255) NOT NULL,
    `fecha_nacimiento` DATE NULL,
    `familia` TINYINT NULL,
    PRIMARY KEY(id),
    INDEX(nombre),
    INDEX(apellido)     
);
```

### Segunda forma de agregar un índice a una tabla ya creada

```sql
USE world; --Indicamos la database a usar

--Creamos un índice llamado world_index en la database word en la tabla city y en la columna name
CREATE INDEX world_index on world.city(name); 

SHOW IDEX FROM world.city; --Mostramos los índices en la tabla city
 
ALTER TABLE world.city DROP INDEX world_index; --Eliminamos el indice world_index en la tabla city de la database world 
```
