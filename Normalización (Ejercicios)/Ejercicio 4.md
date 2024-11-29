# Ejercicio 4

Una tienda de suministros almacena los datos de sus pedidos en la siguiente tabla `Pedidos`.

| PedidoID | FechaPedido | ClienteID | ClienteNombre | ProductoID | ProductoNombre | Cantidad | PrecioUnitario |
|----------|-------------|-----------|---------------|------------|----------------|----------|----------------|
| 301      | 05/04/2023  | 501       | Juan          | 1001       | Lápiz          | 10       | 0.5            |
| 301      | 05/04/2023  | 501       | Juan          | 1002       | Cuaderno       | 5        | 1.5            |
| 302      | 06/04/2023  | 502       | María         | 1003       | Bolígrafo      | 8        | 0.8            |
| 303      | 07/04/2023  | 503       | Luis          | 1001       | Lápiz          | 12       | 0.5            |
| 303      | 07/04/2023  | 503       | Luis          | 1004       | Borrador       | 4        | 0.3            |

### Paso 1: Primera Forma Normal (1NF)

Para cumplir con la 1FN, debemos asegurarnos de que cada campo sea atómico. En esta tabla, los campos ClienteNombre y ProductoNombre no son atómicos. Para cumplir con la 1NF, descomponemos la información de manera que no haya duplicación de datos y que cada campo contenga solo un valor.

**Dependencias funcionales en 1NF:**

- `ClienteID` → `ClienteNombre`
- `ProductoID` → `ProductoNombre`, `PrecioUnitario`

**Tabla Pedidos (1NF):**

| PedidoID | FechaPedido | ClienteID | ProductoID | Cantidad | PrecioUnitario |
|----------|-------------|-----------|------------|----------|----------------|
| 301      | 05/04/2023  | 501       | 1001       | 10       | 0.5            |
| 301      | 05/04/2023  | 501       | 1002       | 5        | 1.5            |
| 302      | 06/04/2023  | 502       | 1003       | 8        | 0.8            |
| 303      | 07/04/2023  | 503       | 1001       | 12       | 0.5            |
| 303      | 07/04/2023  | 503       | 1004       | 4        | 0.3            |

**Tabla Clientes:**

| ClienteID | ClienteNombre |
|-----------|---------------|
| 501       | Juan          |
| 502       | María         |
| 503       | Luis          |

**Tabla Productos:**

| ProductoID | ProductoNombre | PrecioUnitario |
|------------|----------------|----------------|
| 1001       | Lápiz          | 0.5            |
| 1002       | Cuaderno       | 1.5            |
| 1003       | Bolígrafo      | 0.8            |
| 1004       | Borrador       | 0.3            |

**Claves primarias:**

- Tabla Pedidos: compuesta por los campos `PedidoID` y `ProductoID`.
- Tabla Clientes: `ClienteID`.
- Tabla Productos: `ProductoID`.

Ahora tenemos la tabla normalizada en 1FN, ya que los valores son atómicos.

### Paso 2: Segunda Forma Normal (2NF)

Para cumplir con la 2NF, debemos eliminar las dependencias parciales. Es decir, si tenemos una clave primaria compuesta, debemos asegurarnos de que cada atributo dependa completamente de la clave primaria.

**Análisis de dependencias funcionales en 2NF:**

- En la tabla Pedidos, `ClienteID` → `ClienteNombre` es una dependencia parcial, porque `ClienteNombre` depende solo de `ClienteID` y no de la clave compuesta `(PedidoID, ProductoID)`.
- En la tabla Pedidos, `ProductoID` → `ProductoNombre`, `PrecioUnitario` es una dependencia parcial, ya que estos atributos dependen solo de `ProductoID`.

**Solución:**

- Creamos la tabla **Clientes** para almacenar la información de los clientes.
- Creamos la tabla **Productos** para almacenar la información de los productos.
- La tabla **Pedidos** se mantiene con las claves de `PedidoID` y `ProductoID` como clave primaria.

**Tabla Pedidos (2NF):**

| PedidoID | FechaPedido | ClienteID | ProductoID | Cantidad |
|----------|-------------|-----------|------------|----------|
| 301      | 05/04/2023  | 501       | 1001       | 10       |
| 301      | 05/04/2023  | 501       | 1002       | 5        |
| 302      | 06/04/2023  | 502       | 1003       | 8        |
| 303      | 07/04/2023  | 503       | 1001       | 12       |
| 303      | 07/04/2023  | 503       | 1004       | 4        |

**Tabla Clientes (2NF):**

| ClienteID | ClienteNombre |
|-----------|---------------|
| 501       | Juan          |
| 502       | María         |
| 503       | Luis          |

**Tabla Productos (2NF):**

| ProductoID | ProductoNombre | PrecioUnitario |
|------------|----------------|----------------|
| 1001       | Lápiz          | 0.5            |
| 1002       | Cuaderno       | 1.5            |
| 1003       | Bolígrafo      | 0.8            |
| 1004       | Borrador       | 0.3            |

**Claves primarias:**

- Tabla Pedidos: compuesta por los campos `PedidoID` y `ProductoID`.
- Tabla Clientes: `ClienteID`.
- Tabla Productos: `ProductoID`.

Ahora cumplimos con la 2NF.

### Paso 3: Tercera Forma Normal (3NF)

Para cumplir con la 3NF, debemos eliminar las dependencias transitivas. Es decir, si un atributo depende de otro atributo que no es clave primaria, debemos crear nuevas tablas.

**Análisis de dependencias transitivas en 3NF:**

- En la tabla **Clientes**, no hay dependencias transitivas, ya que `ClienteNombre` depende directamente de `ClienteID` y no de ningún otro atributo intermedio.
- En la tabla **Productos**, no hay dependencias transitivas, ya que `ProductoNombre` y `PrecioUnitario` dependen directamente de `ProductoID`.

**Conclusión:**

No es necesario realizar cambios adicionales en las tablas **Clientes** y **Productos**, ya que ya cumplen con la 3NF. Sin embargo, si tuviéramos dependencias transitivas adicionales, las eliminaríamos creando nuevas tablas y moviendo los atributos dependientes.

### Resumen de tablas normalizadas:

**Tabla Pedidos (3NF):**

| PedidoID | FechaPedido | ClienteID | ProductoID | Cantidad |
|----------|-------------|-----------|------------|----------|
| 301      | 05/04/2023  | 501       | 1001       | 10       |
| 301      | 05/04/2023  | 501       | 1002       | 5        |
| 302      | 06/04/2023  | 502       | 1003       | 8        |
| 303      | 07/04/2023  | 503       | 1001       | 12       |
| 303      | 07/04/2023  | 503       | 1004       | 4        |

**Tabla Clientes (3NF):**

| ClienteID | ClienteNombre |
|-----------|---------------|
| 501       | Juan          |
| 502       | María         |
| 503       | Luis          |

**Tabla Productos (3NF):**

| ProductoID | ProductoNombre | PrecioUnitario |
|------------|----------------|----------------|
| 1001       | Lápiz          | 0.5            |
| 1002       | Cuaderno       | 1.5            |
| 1003       | Bolígrafo      | 0.8            |
| 1004       | Borrador       | 0.3            |

**Claves primarias:**

- Tabla Pedidos: compuesta por los campos `PedidoID` y `ProductoID`.
- Tabla Clientes: `ClienteID`.
- Tabla Productos: `ProductoID`.

Con esto hemos normalizado la tabla **Pedidos** para que cumpla con la 1FN, 2FN y 3FN.

