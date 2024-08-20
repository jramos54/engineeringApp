# Git Flow

Este proyecto sigue un flujo de trabajo basado en ramas que organiza y separa el desarrollo del frontend, backend y las pruebas. A continuación se describe el flujo de trabajo y las ramas involucradas.

## Estructura de Ramas

### 1. **`main`**
- **Propósito**: La rama `main` es la rama de producción principal. Solo se utiliza para documentación.
- **Regla**: Solo se pueden agregar o actualizar archivos de documentación en esta rama. No se aceptan cambios de código aquí.

### 2. **`frontend`**
- **Propósito**: Rama principal para el desarrollo del frontend.
- **Regla**: Todo el desarrollo de frontend parte de esta rama. Las ramas de características (features) específicas deben derivar de aquí.

### 3. **`backend`**
- **Propósito**: Rama principal para el desarrollo del backend.
- **Regla**: Todo el desarrollo de backend parte de esta rama. Las ramas de características (features) específicas deben derivar de aquí.

### 4. **`developFront`**
- **Propósito**: Rama de integración para el frontend.
- **Regla**: Las ramas de características del frontend se integran aquí mediante Pull Requests.

### 5. **`developBack`**
- **Propósito**: Rama de integración para el backend.
- **Regla**: Las ramas de características del backend se integran aquí mediante Pull Requests.

### 6. **`QA`**
- **Propósito**: Rama principal para las pruebas de QA.
- **Regla**: Las ramas de pruebas (test) derivan de esta rama y contienen las pruebas correspondientes a las características del frontend o backend.

## Flujo de Trabajo

### 1. **Creación de Ramas de Características (Feature Branches)**

Cuando se va a trabajar en una nueva característica o tarea, se crea una nueva rama desde `frontend` o `backend`, dependiendo de si la tarea es de frontend o backend.

- **Frontend**: La rama se crea desde `frontend` con el siguiente formato:
feature/front/TICKET-nombreRama


- **Backend**: La rama se crea desde `backend` con el siguiente formato:
feature/back/TICKET-nombreRama


### 2. **Desarrollo de la Característica**

El trabajo de desarrollo se realiza en la rama `feature/front/TICKET-nombreRama` o `feature/back/TICKET-nombreRama`, dependiendo de la naturaleza del ticket.

### 3. **Pull Request (PR) a Ramas de Desarrollo**

Una vez que el desarrollo de la característica está completo, se realiza un Pull Request (PR) hacia la rama de desarrollo correspondiente:

- **Frontend**: Se realiza un PR hacia la rama `developFront`.
- **Backend**: Se realiza un PR hacia la rama `developBack`.

El equipo revisará el PR, y una vez aprobado, se realiza el merge de la característica a `developFront` o `developBack`.

### 4. **Pruebas (QA)**

Cuando una característica está lista para ser probada, se crea una nueva rama de pruebas desde `QA`:

- **Formato de rama de pruebas**:
test/TICKET-nombreRama


Las pruebas se realizan en esta rama, y cualquier problema identificado puede solucionarse dentro de la rama de pruebas o en la rama de características correspondiente.

### 5. **Documentación en `main`**

Cualquier actualización o adición de documentación se realiza directamente en la rama `main`. No se permite código en esta rama, solo documentación.

## Resumen de Comandos

1. **Crear una rama de características** (Frontend o Backend):
 ```bash
 git checkout -b feature/front/TICKET-nombreRama frontend
 # o
 git checkout -b feature/back/TICKET-nombreRama backend
```
2. **Crear una rama de pruebas**:
git checkout -b test/TICKET-nombreRama QA
3. **Hacer un Pull Request (PR)**:

Dirigido a developFront si es una característica de frontend.
Dirigido a developBack si es una característica de backend.
Notas
Las ramas frontend y backend no deben ser directamente modificadas. Todo el desarrollo debe pasar por las ramas feature.
Las ramas QA solo deben contener código de prueba, y las subramas de prueba deben crearse para cada ticket.
Este flujo asegura una separación clara entre las etapas de desarrollo, pruebas y documentación, permitiendo una integración organizada y un seguimiento eficiente de los cambios.