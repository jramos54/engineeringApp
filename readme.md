# Arquitectura de Frontend con Vue

Este proyecto sigue una arquitectura hexagonal (puertos y adaptadores) aplicada en un frontend desarrollado con Vue.js, implementando principios **SOLID** y patrones de diseño. La estructura modular asegura que el código sea altamente mantenible, escalable y desacoplado.

## Estructura de Carpetas

```bash
src/
├── adapters/
│   ├── api/
│   │   ├── axios-instance.js
│   │   ├── userApiAdapter.js
│   │   └── productApiAdapter.js
│   ├── storage/
│   │   ├── localStorageAdapter.js
│   │   └── sessionStorageAdapter.js
│   └── ui/
│       ├── Button.vue
│       ├── Modal.vue
│       └── Input.vue
├── application/
│   ├── use-cases/
│   │   ├── user/
│   │   │   ├── CreateUserUseCase.js
│   │   │   ├── GetUserUseCase.js
│   │   │   └── UpdateUserUseCase.js
│   │   └── product/
│   │       ├── GetProductUseCase.js
│   │       └── DeleteProductUseCase.js
│   └── ports/
│       ├── UserRepositoryPort.js
│       └── ProductRepositoryPort.js
├── domain/
│   ├── models/
│   │   ├── User.js
│   │   └── Product.js
│   ├── services/
│   │   ├── UserService.js
│   │   └── ProductService.js
│   └── value-objects/
│       ├── Email.js
│       └── ProductPrice.js
├── infrastructure/
│   ├── repositories/
│   │   ├── user/
│   │   │   ├── InMemoryUserRepository.js
│   │   │   └── RestUserRepository.js
│   │   └── product/
│   │       ├── InMemoryProductRepository.js
│   │       └── RestProductRepository.js
│   └── logging/
│       ├── ConsoleLogger.js
│       └── RemoteLogger.js
├── presentation/
│   ├── components/
│   │   ├── UserList.vue
│   │   ├── UserForm.vue
│   │   └── ProductList.vue
│   ├── views/
│   │   ├── Home.vue
│   │   ├── UserPage.vue
│   │   └── ProductPage.vue
│   └── router/
│       └── index.js
├── App.vue
└── main.js
```
## Descripción de Carpetas

- **`adapters/`**: Contiene los adaptadores que transforman las peticiones de datos entre el mundo externo (APIs, almacenamiento, UI) y la aplicación. Esto permite que la lógica del negocio sea independiente de detalles externos.
  - **`api/`**: Adaptadores que interactúan con APIs externas.
  - **`storage/`**: Adaptadores para almacenamiento en `localStorage` y `sessionStorage`.
  - **`ui/`**: Componentes UI que actúan como adaptadores entre la presentación y la lógica de la aplicación.

- **`application/`**: Contiene la lógica de negocio en forma de casos de uso y define los puertos que actúan como contratos que los adaptadores deben implementar.
  - **`use-cases/`**: Implementaciones de los casos de uso.
  - **`ports/`**: Interfaces que los adaptadores deben cumplir para comunicar la lógica de negocio con el mundo externo.

- **`domain/`**: Contiene la lógica de negocio central, incluyendo modelos de dominio, servicios y objetos de valor.
  - **`models/`**: Entidades de dominio.
  - **`services/`**: Lógica del negocio que no depende de un caso de uso específico.
  - **`value-objects/`**: Modelos de datos inmutables.

- **`infrastructure/`**: Implementaciones concretas de los puertos definidos en la capa de aplicación.
  - **`repositories/`**: Implementaciones de los repositorios que interactúan con APIs, bases de datos, etc.
  - **`logging/`**: Implementaciones de loggers.

- **`presentation/`**: Todo lo relacionado con la presentación de la aplicación.
  - **`components/`**: Componentes reutilizables de Vue.
  - **`views/`**: Vistas que representan páginas completas.
  - **`router/`**: Configuración del enrutamiento de Vue Router.

- **Raíz (`App.vue`, `main.js`)**: Archivos de configuración principal de la aplicación Vue.

## Principios SOLID Aplicados

- **S**: Cada clase tiene una única responsabilidad.
- **O**: Las interfaces (`ports/`) permiten que la arquitectura sea abierta para extensión pero cerrada para modificación.
- **L, I**: Las clases implementan interfaces bien definidas, asegurando que las dependencias puedan ser sustituidas sin romper la funcionalidad.
- **D**: Las dependencias se inyectan a través de adaptadores y puertos, permitiendo que la lógica del negocio no dependa de implementaciones concretas.

## Patrones de Diseño Utilizados

- **Adaptador**: Las implementaciones de las interfaces conectan la lógica interna con el mundo externo.
- **Factory**: Puede usarse para crear instancias de servicios o repositorios.
- **Inversión de Dependencias**: La lógica del negocio depende de abstracciones, no de detalles concretos.
