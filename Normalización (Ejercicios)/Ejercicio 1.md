# Ejercicio 1

La siguiente tabla representa una base de datos simplificada para una biblioteca:

| CodLibro | Titulo               | Autor                  | Editorial     | CodLector | NombreLector           | FechaDev    |
|----------|----------------------|------------------------|---------------|-----------|------------------------|-------------|
| 1001     | Variable Compleja     | Murray Spiegel         | McGraw Hill   | 501       | Pérez Gómez, Juan      | 15/04/2014  |
| 1004     | Visual Basic 5        | E. Petroustsos         | Anaya         | 502       | Ríos Terán, Ana        | 17/04/2014  |
| 1005     | Estadística           | Murray Spiegel         | McGraw Hill   | 503       | Roca, René             | 16/04/2014  |
| 1006     | Oracle University     | Nancy Greenberg, Priya Nathan | Oracle Corp. | 504       | García Roque, Luis     | 20/04/2014  |
| 1007     | Clipper 5.01          | Ramalho                | McGraw Hill   | 501       | Pérez Gómez, Juan      | 18/04/2014  |
| 1004     | Visual Basic 5        | E. Petroustsos         | Anaya         | 505       | Padrón González, Alicia | 30/04/2014 |

## Primera Forma Normal (1NF)

La clave primaria estaría compuesta por los campos `CodLibro` y `CodLector`, ya que cada combinación única identifica un préstamo específico de un libro a un lector.

> Nota: Al elegir como clave primaria `CodLibro` y `CodLector`, significa que no permitimos que un mismo lector pueda sacar el mismo libro más de una vez.

Para cumplir con la 1NF, descomponemos el nombre del lector en sus componentes atómicos: Apellido1, Apellido2 y Nombre.

### Tabla Biblioteca en 1NF:

| CodLibro | Titulo               | Autor                  | Editorial     | CodLector | Apellido1 | Apellido2 | Nombre | FechaDev    |
|----------|----------------------|------------------------|---------------|-----------|----------|-----------|--------|-------------|
| 1001     | Variable Compleja     | Murray Spiegel         | McGraw Hill   | 501       | Pérez    | Gómez     | Juan   | 15/04/2014  |
| 1004     | Visual Basic 5        | E. Petroustsos         | Anaya         | 502       | Ríos     | Terán     | Ana    | 17/04/2014  |
| 1005     | Estadística           | Murray Spiegel         | McGraw Hill   | 503       | Roca     |           | René   | 16/04/2014  |
| 1006     | Oracle University     | Nancy Greenberg, Priya Nathan | Oracle Corp. | 504       | García   | Roque     | Luis   | 20/04/2014  |
| 1007     | Clipper 5.01          | Ramalho                | McGraw Hill   | 501       | Pérez    | Gómez     | Juan   | 18/04/2014  |
| 1004     | Visual Basic 5        | E. Petroustsos         | Anaya         | 505       | Padrón   | González  | Alicia | 30/04/2014  |

Además, los valores del campo `Autor` no son atómicos, por lo que creamos distintos registros para crear cada uno de los autores:

### Tabla Biblioteca:

| CodLibro | Titulo               | Editorial     | CodLector | Apellido1 | Apellido2 | Nombre | FechaDev    |
|----------|----------------------|---------------|-----------|----------|-----------|--------|-------------|
| 1001     | Variable Compleja     | McGraw Hill   | 501       | Pérez    | Gómez     | Juan   | 15/04/2014  |
| 1004     | Visual Basic 5        | Anaya         | 502       | Ríos     | Terán     | Ana    | 17/04/2014  |
| 1005     | Estadística           | McGraw Hill   | 503       | Roca     |           | René   | 16/04/2014  |
| 1006     | Oracle University     | Oracle Corp.  | 504       | García   | Roque     | Luis   | 20/04/2014  |
| 1007     | Clipper 5.01          | McGraw Hill   | 501       | Pérez    | Gómez     | Juan   | 18/04/2014  |
| 1004     | Visual Basic 5        | Anaya         | 505       | Padrón   | González  | Alicia | 30/04/2014  |

### Tabla Autores:

| CodLibro | Autor              |
|----------|--------------------|
| 1001     | Murray Spiegel     |
| 1004     | E. Petroustsos     |
| 1005     | Murray Spiegel     |
| 1006     | Nancy Greenberg    |
| 1006     | Priya Nathan       |
| 1007     | Ramalho            |

Este cambio genera una redundancia en la clave primaria (`CodLibro`, `CodLector`), por lo que seguimos sin cumplir la 1NF, por lo que debemos crear una tabla para guardar los autores.

### Claves primarias:
- **Tabla Biblioteca** (compuesta): `CodLibro` y `CodLector`.
- **Tabla Autores**: `CodLibro`.

## Segunda Forma Normal (2NF)

La 2NF elimina las dependencias funcionales.

### Análisis de dependencias funcionales:
- `Titulo` y `Editorial` dependen solamente de `CodLibro`, ya que estas características pertenecen exclusivamente al libro y no dependen del lector. Esto indica una dependencia funcional parcial con respecto a la clave primaria compuesta (`CodLibro`, `CodLector`).
- `Apellido1`, `Apellido2` y `Nombre` dependen solamente de `CodLector`, porque estos datos pertenecen únicamente al lector y no están relacionados directamente con el libro. Esto también es una dependencia funcional parcial respecto a la clave primaria compuesta (`CodLibro`, `CodLector`).
- `FechaDev` depende de `CodLibro` y `CodLector`, porque estos datos están relacionados con el lector y el libro. Esto es una dependencia funcional completa respecto a la clave primaria compuesta.

Se crean tablas adicionales para los datos de los libros, lectores y la relación préstamo.

### Tabla Libros:

| CodLibro | Titulo               | Editorial     |
|----------|----------------------|---------------|
| 1001     | Variable Compleja     | McGraw Hill   |
| 1004     | Visual Basic 5        | Anaya         |
| 1005     | Estadística           | McGraw Hill   |
| 1006     | Oracle University     | Oracle Corp.  |
| 1007     | Clipper 5.01          | McGraw Hill   |

### Tabla Lectores:

| CodLector | Apellido1 | Apellido2 | Nombre |
|-----------|----------|-----------|--------|
| 501       | Pérez    | Gómez     | Juan   |
| 502       | Ríos     | Terán     | Ana    |
| 503       | Roca     |           | René   |
| 504       | García   | Roque     | Luis   |
| 505       | Padrón   | González  | Alicia |

### Tabla Prestamos:

| CodLibro | CodLector | FechaDev    |
|----------|-----------|-------------|
| 1001     | 501       | 15/04/2014  |
| 1004     | 502       | 17/04/2014  |
| 1005     | 503       | 16/04/2014  |
| 1006     | 504       | 20/04/2014  |
| 1007     | 501       | 18/04/2014  |
| 1004     | 505       | 30/04/2014  |

### Tabla Autores (sin cambios):

| CodLibro | Autor              |
|----------|--------------------|
| 1001     | Murray Spiegel     |
| 1004     | E. Petroustsos     |
| 1005     | Murray Spiegel     |
| 1006     | Nancy Greenberg    |
| 1006     | Priya Nathan       |
| 1007     | Ramalho            |

Ahora sí cumple la 2NF.

## Tercera Forma Normal (3NF)

No existen dependencias transitivas:

### Tabla Autores:
**Clave primaria** (compuesta): `CodLibro` y `Autor`.

**Análisis de dependencias transitivas**:
- `CodLibro → Autor`: `Autor` depende directamente de `CodLibro`.
- En esta tabla, no hay dependencias transitivas porque cada atributo `Autor` depende únicamente de la clave primaria `CodLibro` y no de ningún otro atributo intermedio.

### Tabla Libros:
**Clave primaria**: `CodLibro`.

**Análisis de dependencias transitivas**:
- `CodLibro → Titulo`: `Titulo` depende directamente de `CodLibro`.
- `CodLibro → Editorial`: `Editorial` depende directamente de `CodLibro`.
- En esta tabla, no hay dependencias transitivas porque cada atributo `Titulo` y `Editorial` depende únicamente de la clave primaria `CodLibro` y no de ningún otro atributo intermedio.

### Tabla Lectores:
**Clave primaria**: `CodLector`.

**Análisis de dependencias transitivas**:
- `CodLector → Apellido1, Apellido2, Nombre`: todos estos atributos dependen directamente de `CodLector`.
- No existen dependencias transitivas en esta tabla, ya que cada atributo `Apellido1`, `Apellido2` y `Nombre` depende exclusivamente de la clave primaria `CodLector` y no de ningún otro atributo intermedio.

### Tabla Prestamos:
**Clave primaria compuesta**: (`CodLibro`, `CodLector`).

**Análisis de dependencias transitivas**:
- (`CodLibro`, `CodLector`) → `FechaDev`: `FechaDev` depende directamente de la clave primaria compuesta (`CodLibro`, `CodLector`).

En esta tabla, no existen dependencias transitivas porque `FechaDev` depende completamente de la clave primaria compuesta (`CodLibro`, `CodLector`) y no de ningún otro atributo intermedio.
