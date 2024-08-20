# Arquitectura de Microservicios con Puertos y Adaptadores

Este documento describe la arquitectura de microservicios basada en **Django Rest Framework (DRF)** y **FastAPI**, utilizando principios SOLID y patrones de diseño con la arquitectura de puertos y adaptadores. Cada microservicio es independiente, desplegable por separado, y puede interactuar con otros microservicios mediante APIs o colas de mensajes.

## Estructura General para Microservicios

Cada microservicio tendrá su propio repositorio o directorio. Dentro de cada microservicio, seguiremos la estructura de puertos y adaptadores, con capas bien definidas para asegurar la modularidad y el desacoplamiento.

### Ejemplo de Estructura para un Microservicio

```bash
service-name/
├── src/
│   ├── adapters/
│   │   ├── http/
│   │   │   ├── drf/
│   │   │   │   ├── views.py
│   │   │   │   ├── serializers.py
│   │   │   │   └── urls.py
│   │   │   ├── fastapi/
│   │   │   │   ├── views.py
│   │   │   │   └── urls.py
│   │   ├── repositories/
│   │   │   ├── django_repository.py
│   │   │   └── async_repository.py
│   ├── application/
│   │   ├── use-cases/
│   │   │   ├── CreateUserUseCase.py
│   │   │   └── UpdateUserUseCase.py
│   │   ├── ports/
│   │       ├── user_repository_port.py
│   │       └── async_service_port.py
│   ├── domain/
│   │   ├── models/
│   │   │   ├── user.py
│   │   │   └── product.py
│   │   ├── services/
│   │   │   ├── user_service.py
│   │   │   └── product_service.py
│   ├── infrastructure/
│   │   ├── database/
│   │   │   ├── migrations/
│   │   │   └── db_handler.py
│   │   ├── logging/
│   │   ├── cache/
│   │   │   ├── redis_cache.py
│   │   │   └── memcached_cache.py
│   │   └── message_brokers/
│   │       ├── kafka_broker.py
│   │       └── rabbitmq_broker.py
├── tests/
│   ├── unit/
│   └── integration/
├── Dockerfile
├── docker-compose.yml
└── manage.py / app.py
```
## Descripción de Carpetas y Archivos

- **`src/`**: Contiene el código fuente del microservicio.

  - **`adapters/`**: Contiene las implementaciones que permiten a la lógica de negocio interactuar con el mundo externo.

    - **`http/`**: Adaptadores que manejan la comunicación HTTP. Puedes tener subcarpetas separadas para **DRF** y **FastAPI**, dependiendo de cuál maneje las diferentes rutas y controladores.
    
    - **`repositories/`**: Adaptadores que implementan los puertos de repositorio para interactuar con la base de datos de manera síncrona (usando **DRF**) o asíncrona (usando **FastAPI**).

  - **`application/`**: Contiene la lógica de negocio en forma de casos de uso y define los **puertos** que actúan como contratos que los adaptadores deben implementar.
    
    - **`use-cases/`**: Implementaciones de casos de uso específicos, como la creación de usuarios, productos, etc.
    
    - **`ports/`**: Interfaces que definen los métodos que los adaptadores deben implementar, como `user_repository_port.py` o `async_service_port.py`.

  - **`domain/`**: Define los modelos, servicios y objetos de valor fundamentales para la lógica de negocio.
    
    - **`models/`**: Entidades de dominio como `User` y `Product`.
    
    - **`services/`**: Contiene la lógica central de negocio relacionada con cada modelo, como `user_service.py` y `product_service.py`.

  - **`infrastructure/`**: Implementaciones técnicas que interactúan con servicios externos como bases de datos, cachés y brokers de mensajes.
    
    - **`database/`**: Manejo de bases de datos y migraciones.
    
    - **`logging/`**: Implementaciones para la gestión de logs.
    
    - **`cache/`**: Implementaciones de caché, como `redis_cache.py`.
    
    - **`message_brokers/`**: Manejo de colas de mensajes, como Kafka o RabbitMQ, para la comunicación entre microservicios.

- **`tests/`**: Carpeta para pruebas unitarias e integrales.
  
  - **`unit/`**: Pruebas de componentes individuales.
  
  - **`integration/`**: Pruebas que verifican la integración entre diferentes componentes y servicios externos.

- **`Dockerfile`**: Archivo de configuración para construir la imagen Docker del microservicio.

- **`docker-compose.yml`**: Archivo de configuración para levantar el servicio y sus dependencias (bases de datos, caché, etc.) en contenedores Docker.

## Características Clave de Microservicios

1. **Independencia**  
   Cada microservicio es completamente autónomo. Cada servicio tiene su propio código, `Dockerfile`, y puede ser desplegado, actualizado y escalado de manera independiente de los otros.

2. **Comunicación entre Microservicios**  
   Los microservicios pueden comunicarse entre sí utilizando **APIs REST** (sincrónicamente) o a través de **colas de mensajes** (asíncronamente). Esto permite una arquitectura flexible y desacoplada.

   - **APIs REST**: Los microservicios pueden exponerse mediante APIs utilizando **DRF** o **FastAPI**.
   - **Colas de Mensajes**: Para operaciones asíncronas o eventos, los microservicios pueden interactuar a través de sistemas de mensajería como Kafka o RabbitMQ.

3. **Despliegue Independiente**  
   Cada microservicio tiene su propio `Dockerfile` y pipeline de CI/CD, lo que permite que cada servicio sea desplegado sin afectar a otros. Esto asegura que las actualizaciones sean más rápidas y específicas.

4. **Separación de Preocupaciones**
   - **Adaptadores**: Conectan la lógica de negocio con el mundo externo (HTTP, bases de datos, APIs).
   - **Puertos**: Definen contratos para la lógica de negocio que los adaptadores deben cumplir, garantizando que los microservicios sean extensibles y fácilmente modificables.
   - **Dominio**: Contiene las reglas de negocio puras, independientes de los detalles técnicos.

## Principios SOLID Aplicados

- **S**: Cada clase y módulo tiene una única responsabilidad, facilitando su mantenimiento y evolución.
- **O**: El sistema es abierto a la extensión mediante la creación de nuevos adaptadores o servicios, pero cerrado a la modificación del código existente.
- **L**: Las implementaciones de los puertos pueden ser reemplazadas sin afectar el comportamiento del sistema.
- **I**: Las interfaces están diseñadas para ser específicas y evitar que las clases dependan de métodos innecesarios.
- **D**: El código de negocio depende de abstracciones (puertos) y no de implementaciones concretas (adaptadores).

## Patrones de Diseño Utilizados

- **Adaptador**: Conecta la lógica de negocio con sistemas externos.
- **Factory**: Utilizado para crear instancias de clases o servicios en función de la configuración.
- **Inversión de Dependencias**: La lógica de negocio depende de interfaces y no de implementaciones concretas.
