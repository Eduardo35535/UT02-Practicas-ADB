# Ejercicio 2

Partimos de una tabla sin normalizar que contiene información sobre órdenes de compra y artículos.

| IdOrden | Fecha     | IdCliente | NombreCliente | TelefonoCliente    | Municipio               | Provincia              | IdArticulo | NombreArticulo | Cantidad | Precio |
|---------|-----------|-----------|---------------|--------------------|-------------------------|------------------------|------------|-----------------|----------|--------|
| 2301    | 23/02/14  | 101       | Martin        | 922141423 657659873| Candelaria              | Santa Cruz de Tenerife | 3786       | Red             | 3        | 35,00  |
| 2301    | 23/02/14  | 101       | Martin        | 922141423 657659873| Candelaria              | Santa Cruz de Tenerife | 4011       | Raqueta         | 6        | 65,00  |
| 2301    | 23/02/14  | 101       | Martin        | 922141423 657659873| Candelaria              | Santa Cruz de Tenerife | 9132       | Paq-3           | 8        | 4,75   |
| 2302    | 25/02/14  | 107       | Herman        | 928339463 633987125| Telde                   | Las Palmas             | 5794       | Paq-6           | 4        | 5,00   |
| 2303    | 27/02/14  | 110       | Pedro         | 928388841 798456870| Telde                   | Las Palmas             | 4011       | Raqueta         | 2        | 65,00  |
| 2303    | 27/02/14  | 110       | Pedro         | 928388841 798456870| Telde                   | Las Palmas             | 3141       | Funda           | 2        | 10,00  |

### Datos a tener en cuenta:

- El campo **Precio** indica el valor unitario del producto.

## Primera Forma Normal (1NF)

Para cumplir con la 1NF, el campo **TelefonoCliente** no es atómico, así que creamos la tabla **Telefonos**.

### Tabla Ordenes:

| IdOrden | Fecha     | IdCliente | NombreCliente | Municipio               | Provincia              | IdArticulo | NombreArticulo | Cantidad | Precio |
|---------|-----------|-----------|---------------|-------------------------|------------------------|------------|-----------------|----------|--------|
| 2301    | 23/02/14  | 101       | Martin        | Candelaria              | Santa Cruz de Tenerife | 3786       | Red             | 3        | 35,00  |
| 2301    | 23/02/14  | 101       | Martin        | Candelaria              | Santa Cruz de Tenerife | 4011       | Raqueta         | 6        | 65,00  |
| 2301    | 23/02/14  | 101       | Martin        | Candelaria              | Santa Cruz de Tenerife | 9132       | Paq-3           | 8        | 4,75   |
| 2302    | 25/02/14  | 107       | Herman        | Telde                   | Las Palmas             | 5794       | Paq-6           | 4        | 5,00   |
| 2303    | 27/02/14  | 110       | Pedro         | Telde                   | Las Palmas             | 4011       | Raqueta         | 2        | 65,00  |
| 2303    | 27/02/14  | 110       | Pedro         | Telde                   | Las Palmas             | 3141       | Funda           | 2        | 10,00  |

### Tabla Telefonos:

| IdCliente | TelefonoCliente   |
|-----------|-------------------|
| 101       | 922141423         |
| 101       | 657659873         |
| 107       | 928339463         |
| 107       | 633987125         |
| 110       | 928388841         |
| 110       | 798456870         |

**Claves primarias:**

- **Tabla Ordenes**: compuesta por los campos `IdOrden` y `IdArticulo`, ya que cada combinación única de estos campos identifica una orden de compra.
- **Tabla Telefonos**: compuesta por los campos `IdCliente` y `TelefonoCliente`.

Las tablas **Ordenes** y **Telefonos** cumplen con la 1FN ya que todos los valores son atómicos.

## Segunda Forma Normal (2NF)

Para cumplir con la 2NF, eliminamos las dependencias funcionales:

- **Tabla Ordenes**:
  - `NombreArticulo` y `Precio` dependen exclusivamente de `IdArticulo`, no de `IdOrden`. Esto significa que `NombreArticulo` y `Precio` tienen una dependencia parcial respecto a la clave compuesta (`IdOrden`, `IdArticulo`).
  - `NombreCliente`, `Municipio` y `Provincia` dependen exclusivamente de `IdCliente`. Esto significa que `NombreCliente`, `Municipio` y `Provincia` tienen una dependencia parcial respecto a la clave compuesta (`IdOrden`, `IdArticulo`).
  - `Fecha` depende de `IdOrden` y de `IdCliente`. Esto significa que `Fecha` tiene una dependencia completa respecto a la clave compuesta (`IdOrden`, `IdArticulo`).
  - `Cantidad` depende de `IdOrden` y `IdArticulo`. Esto significa que `Cantidad` tiene una dependencia parcial respecto a la clave compuesta (`IdOrden`, `IdArticulo`).

- **Tabla Telefonos**:
  - `TelefonoCliente` depende exclusivamente de `IdCliente`, tienen una dependencia funcional completa respecto a la clave primaria `IdCliente`.

Para eliminar las dependencias funcionales creamos las tablas **Articulos** y **Clientes**.

### Tabla Articulos:

| IdArticulo | NombreArticulo | Precio |
|------------|-----------------|--------|
| 3786       | Red             | 35,00  |
| 4011       | Raqueta         | 65,00  |
| 9132       | Paq-3           | 4,75   |
| 5794       | Paq-6           | 5,00   |
| 3141       | Funda           | 10,00  |

**Clave primaria:** `IdArticulo`.

### Tabla Clientes:

| IdCliente | NombreCliente | Municipio               | Provincia              |
|-----------|---------------|-------------------------|------------------------|
| 101       | Martin        | Candelaria              | Santa Cruz de Tenerife |
| 107       | Herman        | Telde                   | Las Palmas             |
| 110       | Pedro         | Telde                   | Las Palmas             |

**Clave primaria:** `IdCliente`.

### Tabla Ordenes:

| IdOrden | Fecha     | IdCliente |
|---------|-----------|-----------|
| 2301    | 23/02/14  | 101       |
| 2302    | 25/02/14  | 107       |
| 2303    | 27/02/14  | 110       |

**Clave primaria:** `IdOrden`.

**Nota:** Es necesario añadir el campo `IdCliente` para poder relacionar la orden de compra con el cliente.

### Tabla DetalleOrdenes:

| IdOrden | IdArticulo | Cantidad |
|---------|------------|----------|
| 2301    | 3786       | 3        |
| 2301    | 4011       | 6        |
| 2301    | 9132       | 8        |
| 2302    | 5794       | 4        |
| 2303    | 4011       | 2        |
| 2303    | 3141       | 2        |

**Clave primaria:** `IdOrden` y `IdArticulo`.

### Tabla Telefonos (Calculada en la 1FN):

| IdCliente | TelefonoCliente   |
|-----------|-------------------|
| 101       | 922141423         |
| 101       | 657659873         |
| 107       | 928339463         |
| 107       | 633987125         |
| 110       | 928388841         |
| 110       | 798456870         |

**Clave primaria:** `IdCliente` y `TelefonoCliente`.

Ahora sí cumple la 2FN.

## Tercera Forma Normal (3FN)

**Análisis de dependencias transitivas:**

- **Tabla Clientes**: en esta tabla, existe una dependencia transitiva entre `NombreCliente → Municipio → Provincia`, con lo que crearemos la **Tabla Provincias**.

### Tabla Clientes:

| IdCliente | NombreCliente | IdMunicipio |
|-----------|---------------|-------------|
| 101       | Martin        | 1           |
| 107       | Herman        | 2           |
| 110       | Pedro         | 2           |

**Clave primaria:** `IdCliente`.

### Tabla Provincias:

| IdProvincia | Provincia              |
|-------------|------------------------|
| 1           | Santa Cruz de Tenerife |
| 2           | Las Palmas             |

**Clave primaria:** `IdProvincia`.

### Tabla Municipios:

| IdMunicipio | Municipio    | IdProvincia |
|-------------|--------------|-------------|
| 1           | Candelaria   | 1           |
| 2           | Telde        | 2           |

**Clave primaria:** `IdMunicipio`.

Ahora sí cumple la 3FN.

## Ajuste de Normalización

Eliminamos redundancia del campo **Municipio** en la tabla **Clientes** y del campo **Provincia** en la tabla **Provincias**.

- En el caso del campo **Municipio** que se encuentra en ambas tablas **Clientes** y **Provincias**:
  - Descomponer la tabla para que **Clientes** almacene solamente la información exclusiva del cliente y utilice una clave foránea (`IdMunicipio`) para relacionarse con una tabla independiente llamada **Municipios**.
  
- En el caso del campo **Provincia** que se encuentra en la tabla **Provincias**:
  - Modificar la tabla **Municipios** para que utilice una clave foránea (`IdProvincia`) para relacionarse con la tabla independiente llamada **Provincias**.

### Tabla Clientes:

| IdCliente | NombreCliente | IdMunicipio |
|-----------|---------------|-------------|
| 101       | Martin        | 1           |
| 107       | Herman        | 2           |
| 110       | Pedro         | 2           |

**Clave primaria:** `IdCliente`.

### Tabla Provincias:

| IdProvincia | Provincia              |
|-------------|------------------------|
| 1           | Santa Cruz de Tenerife |
| 2           | Las Palmas             |

**Clave primaria:** `IdProvincia`.

### Tabla Municipios:

| IdMunicipio | Municipio    | IdProvincia |
|-------------|--------------|-------------|
| 1           | Candelaria   | 1           |
| 2           | Telde        | 2           |

**Clave primaria:** `IdMunicipio`.

