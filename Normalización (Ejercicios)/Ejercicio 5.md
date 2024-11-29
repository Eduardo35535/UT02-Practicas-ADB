# Ejercicio 5

En una universidad, se lleva un registro de cursos, estudiantes y sus calificaciones. Sin embargo, la tabla `CursosEstudiantes` no está completamente normalizada.

Normaliza la tabla `CursosEstudiantes` para que cumpla con 1FN, 2FN y 3FN. Identifica las dependencias funcionales y elimina las dependencias parciales y transitivas en cada paso.


| RegistroID | EstudianteID | NombreEstudiante | Cursos                | Profesor    | Notas       | Departamento  |
|------------|--------------|------------------|-----------------------|-------------|-------------|---------------|
| 1          | 201          | Alicia           | Matemáticas, Física   | Dr. Pérez   | 85, 90      | Ciencias      |
| 2          | 202          | Roberto          | Matemáticas, Química  | Dr. Pérez   | 78, 88      | Ciencias      |
| 3          | 203          | Julia            | Historia, Literatura  | Dr. Gómez   | 92, 80      | Humanidades   |
| 4          | 204          | Mario            | Química               | Dr. Pérez   | 75          | Ciencias      |

---

## **Primera Forma Normal (1FN)**

Para cumplir con la 1FN, debemos asegurarnos de que cada campo contenga solo valores atómicos (sin listas ni valores repetidos). 

### Dependencias a tratar:
- **Cursos** y **Notas** contienen listas, lo que no es atómico. Esto implica que debemos dividir estos campos en registros separados para que cada uno tenga un solo valor.

### Solución:

Dividimos los campos `Cursos` y `Notas` en registros separados:

| RegistroID | EstudianteID | NombreEstudiante | Curso        | Profesor    | Nota  | Departamento  |
|------------|--------------|------------------|--------------|-------------|-------|---------------|
| 1          | 201          | Alicia           | Matemáticas  | Dr. Pérez   | 85    | Ciencias      |
| 1          | 201          | Alicia           | Física       | Dr. Pérez   | 90    | Ciencias      |
| 2          | 202          | Roberto          | Matemáticas  | Dr. Pérez   | 78    | Ciencias      |
| 2          | 202          | Roberto          | Química      | Dr. Pérez   | 88    | Ciencias      |
| 3          | 203          | Julia            | Historia     | Dr. Gómez   | 92    | Humanidades   |
| 3          | 203          | Julia            | Literatura   | Dr. Gómez   | 80    | Humanidades   |
| 4          | 204          | Mario            | Química      | Dr. Pérez   | 75    | Ciencias      |

### Claves primarias:
- Tabla **CursosEstudiantes**: (RegistroID, EstudianteID, Curso) 

---

## **Segunda Forma Normal (2FN)**

La 2FN elimina las dependencias parciales, es decir, cada atributo no clave debe depender completamente de la clave primaria.

### Dependencias a tratar:
- `NombreEstudiante` depende solo de `EstudianteID`, y no de la clave compuesta (RegistroID, EstudianteID, Curso).
- `Profesor` y `Departamento` dependen de `Curso`, no de la clave primaria.

### Solución:

- Crear una tabla separada para los estudiantes.
- Crear una tabla separada para los cursos.

### Tablas normalizadas:

#### Tabla Estudiantes:

| EstudianteID | NombreEstudiante |
|--------------|------------------|
| 201          | Alicia           |
| 202          | Roberto          |
| 203          | Julia            |
| 204          | Mario            |

#### Tabla Cursos:

| Curso        | Profesor    | Departamento  |
|--------------|-------------|---------------|
| Matemáticas  | Dr. Pérez   | Ciencias      |
| Física       | Dr. Pérez   | Ciencias      |
| Química      | Dr. Pérez   | Ciencias      |
| Historia     | Dr. Gómez   | Humanidades   |
| Literatura   | Dr. Gómez   | Humanidades   |

#### Tabla CursosEstudiantes (desglosada):

| RegistroID | EstudianteID | Curso        | Nota  |
|------------|--------------|--------------|-------|
| 1          | 201          | Matemáticas  | 85    |
| 1          | 201          | Física       | 90    |
| 2          | 202          | Matemáticas  | 78    |
| 2          | 202          | Química      | 88    |
| 3          | 203          | Historia     | 92    |
| 3          | 203          | Literatura   | 80    |
| 4          | 204          | Química      | 75    |

### Claves primarias:
- Tabla **Estudiantes**: EstudianteID.
- Tabla **Cursos**: Curso.
- Tabla **CursosEstudiantes**: (RegistroID, EstudianteID, Curso).

---

## **Tercera Forma Normal (3FN)**

La 3FN elimina las dependencias transitivas, es decir, los atributos no clave deben depender directamente de la clave primaria.

### Dependencias a tratar:
- `Departamento` depende transitivamente de `Curso` a través de `Profesor`. El departamento debería estar relacionado directamente con el curso, no con el profesor.

### Solución:

- Se elimina la columna `Departamento` de la tabla **Cursos** y se crea una tabla **Departamentos** que relaciona el curso con el departamento.

### Tablas normalizadas:

#### Tabla Departamentos:

| Curso        | Departamento  |
|--------------|---------------|
| Matemáticas  | Ciencias      |
| Física       | Ciencias      |
| Química      | Ciencias      |
| Historia     | Humanidades   |
| Literatura   | Humanidades   |

#### Tabla Cursos (modificada):

| Curso        | Profesor    |
|--------------|-------------|
| Matemáticas  | Dr. Pérez   |
| Física       | Dr. Pérez   |
| Química      | Dr. Pérez   |
| Historia     | Dr. Gómez   |
| Literatura   | Dr. Gómez   |

### Claves primarias:
- Tabla **Departamentos**: Curso.
- Tabla **Cursos**: Curso.
- Tabla **CursosEstudiantes**: (RegistroID, EstudianteID, Curso).

---

## **Resumen de tablas normalizadas:**

1. **Tabla Estudiantes**:
   | EstudianteID | NombreEstudiante |
   |--------------|------------------|
   | 201          | Alicia           |
   | 202          | Roberto          |
   | 203          | Julia            |
   | 204          | Mario            |

2. **Tabla Cursos**:
   | Curso        | Profesor    |
   |--------------|-------------|
   | Matemáticas  | Dr. Pérez   |
   | Física       | Dr. Pérez   |
   | Química      | Dr. Pérez   |
   | Historia     | Dr. Gómez   |
   | Literatura   | Dr. Gómez   |

3. **Tabla Departamentos**:
   | Curso        | Departamento  |
   |--------------|---------------|
   | Matemáticas  | Ciencias      |
   | Física       | Ciencias      |
   | Química      | Ciencias      |
   | Historia     | Humanidades   |
   | Literatura   | Humanidades   |

4. **Tabla CursosEstudiantes**:
   | RegistroID | EstudianteID | Curso        | Nota  |
   |------------|--------------|--------------|-------|
   | 1          | 201          | Matemáticas  | 85    |
   | 1          | 201          | Física       | 90    |
   | 2          | 202          | Matemáticas  | 78    |
   | 2          | 202          | Química      | 88    |
   | 3          | 203          | Historia     | 92    |
   | 3          | 203          | Literatura   | 80    |
   | 4          | 204          | Química      | 75    |

---

Con estas modificaciones, las tablas cumplen con las tres primeras formas normales (1FN, 2FN y 3FN).
