# Ejercicio 3

En una base de datos de una empresa de alquiler de vehículos, se tiene la siguiente tabla `Alquileres` que contiene información sobre los vehículos alquilados y sus conductores.

Normaliza la tabla `Alquileres` para que cumpla con 1FN, 2FN y 3FN. Identifica las dependencias funcionales y elimina las dependencias parciales y transitivas en cada paso.

| AlquilerID | FechaAlquiler | ClienteID | ClienteNombre | Vehiculos          | PrecioVehiculos   |
|------------|---------------|-----------|---------------|--------------------|-------------------|
| 101        | 10/03/2023    | 201       | Carlos        | Sedán, SUV         | 50, 70            |
| 102        | 12/03/2023    | 202       | Laura         | Convertible, Pickup| 80, 65            |
| 103        | 15/03/2023    | 203       | Pedro         | SUV, Van, Sedán    | 70, 60, 50        |
| 104        | 18/03/2023    | 204       | Ana           | Sedán              | 50                |

### 1. Primera Forma Normal (1FN)

**Reglas para cumplir con 1FN:**
- Los valores en las columnas deben ser atómicos (es decir, no pueden contener listas o valores múltiples).
- Debemos descomponer los valores en columnas individuales para cada vehículo y su respectivo precio.

#### Descomposición de los valores no atómicos:
- Los campos `Vehiculos` y `PrecioVehiculos` contienen listas, por lo que debemos dividir cada fila para cada vehículo alquilado y su precio correspondiente.

#### Tabla Alquileres en 1FN:

| AlquilerID | FechaAlquiler | ClienteID | ClienteNombre | Vehiculo   | PrecioVehiculo |
|------------|---------------|-----------|---------------|------------|----------------|
| 101        | 10/03/2023    | 201       | Carlos        | Sedán      | 50             |
| 101        | 10/03/2023    | 201       | Carlos        | SUV        | 70             |
| 102        | 12/03/2023    | 202       | Laura         | Convertible| 80             |
| 102        | 12/03/2023    | 202       | Laura         | Pickup     | 65             |
| 103        | 15/03/2023    | 203       | Pedro         | SUV        | 70             |
| 103        | 15/03/2023    | 203       | Pedro         | Van        | 60             |
| 103        | 15/03/2023    | 203       | Pedro         | Sedán      | 50             |
| 104        | 18/03/2023    | 204       | Ana           | Sedán      | 50             |

**Clave primaria**: (AlquilerID, Vehiculo)

Ahora, cada registro tiene un valor atómico para el campo `Vehiculos` y `PrecioVehiculos`.

### 2. Segunda Forma Normal (2FN)

**Reglas para cumplir con 2FN:**
- Eliminar las dependencias parciales, es decir, los atributos que dependen solo de una parte de la clave primaria compuesta.
- Las dependencias funcionales deben ser completas con respecto a la clave primaria.

#### Análisis de dependencias funcionales:
- `FechaAlquiler` y `ClienteID` dependen solo de `AlquilerID`.
- `ClienteNombre` depende de `ClienteID`, no de `AlquilerID`.
- `Vehiculo` y `PrecioVehiculo` dependen de `AlquilerID` y `Vehiculo`, por lo que ya son dependencias completas respecto a la clave primaria.

#### Creación de tablas adicionales:
- **Tabla Clientes**: Para almacenar información sobre los clientes, ya que `ClienteID` determina `ClienteNombre`.
- **Tabla Alquileres**: Para almacenar la relación entre el alquiler y su fecha.
- **Tabla Vehiculos**: Para almacenar la información sobre los vehículos alquilados.

#### Tablas después de aplicar 2FN:

**Tabla Alquileres:**

| AlquilerID | FechaAlquiler | ClienteID |
|------------|---------------|-----------|
| 101        | 10/03/2023    | 201       |
| 102        | 12/03/2023    | 202       |
| 103        | 15/03/2023    | 203       |
| 104        | 18/03/2023    | 204       |

**Tabla Clientes:**

| ClienteID | ClienteNombre |
|-----------|---------------|
| 201       | Carlos        |
| 202       | Laura         |
| 203       | Pedro         |
| 204       | Ana           |

**Tabla Vehiculos:**

| AlquilerID | Vehiculo   | PrecioVehiculo |
|------------|------------|----------------|
| 101        | Sedán      | 50             |
| 101        | SUV        | 70             |
| 102        | Convertible| 80             |
| 102        | Pickup     | 65             |
| 103        | SUV        | 70             |
| 103        | Van        | 60             |
| 103        | Sedán      | 50             |
| 104        | Sedán      | 50             |

**Claves primarias**:
- **Tabla Alquileres**: AlquilerID.
- **Tabla Clientes**: ClienteID.
- **Tabla Vehiculos**: (AlquilerID, Vehiculo).

### 3. Tercera Forma Normal (3FN)

**Reglas para cumplir con 3FN:**
- Eliminar las dependencias transitivas, es decir, los atributos que dependen de otros atributos no clave.

#### Análisis de dependencias transitivas:
- En las tablas actuales no existen dependencias transitivas directas. Todos los atributos dependen directamente de las claves primarias en sus respectivas tablas.

### Conclusión

Después de normalizar la tabla original "Alquileres" en 1FN, 2FN y 3FN, hemos logrado eliminar las dependencias parciales y transitivas. El diseño final está compuesto por tres tablas:

1. **Tabla Alquileres**: Relaciona los alquileres con la fecha y el cliente.
2. **Tabla Clientes**: Contiene los datos de los clientes.
3. **Tabla Vehiculos**: Contiene los vehículos alquilados y sus precios.

De esta forma, la base de datos cumple con la 1FN, 2FN y 3FN, asegurando la eliminación de redundancias y mejorando la integridad de los datos.
