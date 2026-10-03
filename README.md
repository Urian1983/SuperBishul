# SuperBishul — v2.0

> **De plataforma monolítica a arquitectura de microservicios distribuida.**
> Modernización integral de e-commerce de restauración, orientada a alta escalabilidad, desacoplamiento de dominios y adopción de estándares modernos con **Spring Boot 4** y **Spring Security 7**.

---

## 📌 Descripción del Proyecto

**SuperBishul** es la evolución de una plataforma monolítica de comercio electrónico enfocada en el sector de la restauración. El objetivo central de esta v2.0 es refactorizar la arquitectura original dividiéndola en un ecosistema de microservicios autónomos, profesionalizando la base de código y adaptando las configuraciones a las especificaciones y breaking changes de **Spring Boot 4** y **Spring Security 7**.

### Objetivos Clave de la v2.0
* **Descomposición del Monolito:** Separación clara de responsabilidades en dominios independientes (`Catalog`, `Cart`, `Order`, `User`).
* **Estandarización Transversal:** Centralización de modelos comunes, seguridad JWT, gestión global de errores y utilidades en el módulo `bishul-common`.
* **Seguridad Moderna:** Implementación del modelo de seguridad reactivo/declarativo exigido por **Spring Security 7**.
* **Evolución Progresiva:** Migración por fases (Fase F0 en curso), garantizando contratos estables antes del despliegue de nuevos servicios.

---

## 🏗️ Arquitectura y Estructura del Proyecto

### Visión General de Servicios

```
                        +----------------------------+
                        |       bishul-common        |
                        | (DTOs, Security, Errors)   |
                        +--------------+-------------+
                                       ^
                                       | (dependencia Maven)
     +-----------------+---------------+---------------+-----------------+
     |                 |                               |                 |
+----+----+    +-------+-------+               +-------+-------+   +-----+-----+
|  User   |    |    Catalog    |               |     Cart      |   |   Order   |
| Service |    |    Service    |               |    Service    |   |  Service  |
+---------+    +---------------+               +---------------+   +-----------+
```

### Árbol de Directorios del Repositorio

```text
SuperBishul/
├── .gitignore
├── README.md
├── pom.xml                          (Parent POM - Packaging pom)
├── bishul-common/                   (Módulo compartido - Packaging jar)
│   ├── pom.xml
│   └── src/
│       └── main/
│           └── java/
│               └── com/bishul/common/
│                   ├── dto/         (DTOs compartidos organizados por dominio)
│                   │   ├── error/
│                   │   ├── auth/
│                   │   ├── product/
│                   │   └── ...
│                   ├── error/       (ErrorCode, BusinessException, GlobalExceptionHandler)
│                   ├── security/    (JwtService, Filtros JWT, Interceptores)
│                   └── util/        (Utilidades puras)
└── bishul-catalog-service/          (Microservicio de Catálogo - Spring Boot App)
    ├── pom.xml
    └── src/
        ├── main/
        │   ├── java/
        │   │   └── com/bishul/catalog/
        │   │       ├── controller/
        │   │       ├── service/
        │   │       ├── repository/
        │   │       ├── model/       (Entidades JPA)
        │   │       ├── mapper/
        │   │       ├── exception/
        │   │       └── config/
        │   └── resources/
        │       ├── application.properties
        │       ├── application-dev.properties
        │       └── application-docker.properties
        └── test/
            └── java/
                └── com/bishul/catalog/
```

---

## ⚙️ Despliegue y Ejecución

### Arranque en Local (Dev)
> 🟡 **[PENDIENTE - Fase F1]**  
> El entorno de ejecución local estará disponible una vez finalizada la implementación y verificación del microservicio `bishul-catalog-service`.

### Arranque con Docker / Docker Compose
> 🟡 **[PENDIENTE - Marcado explícitamente]**  
> La orquestación de contenedores y los archivos `Dockerfile` / `docker-compose.yml` se incorporarán tras completar la fase de construcción de servicios principales.

---

## 🧠 Decisiones de Diseño y Patrones Aceptados

| Dominio / Área | Decisión de Diseño | Descripción Técnica |
| :--- | :--- | :--- |
| **Control de Stock** | **Nivel 2 (Reserva Temporal)** | Estrategia de bloqueo/reserva temporal durante el checkout para evitar condiciones de carrera (race conditions) sin penalizar el rendimiento global. |
| **Flujo de Checkout** | **Orquestación / Idempotencia** | Garantía de idempotencia en peticiones de pago y creación de pedidos mediante *Idempotency Keys* para evitar duplicidades en reintentos de red. |
| **Autenticación** | **JWT Centralizado en `common`** | `JwtService` y filtros de validación ubicados en `bishul-common` para homogeneizar la verificación del token sin duplicar lógica en cada microservicio. |
| **Seguridad** | **Spring Security 7** | Adopción del nuevo paradigma de configuración sin componentes obsoletos (*deprecated*), delegando la autorización de rutas de forma declarativa. |
| **Gestión de Errores** | **Respuestas Estandarizadas** | Formato de error unificado vía `BusinessException` y `ErrorCode` codificados según especificación *RFC 7807 (Problem Details)*. |

---

## 🚀 Hoja de Ruta e Iteraciones Futuras

- [x] **Fase F0 (En progreso):** Definición de la estructura Maven multi-módulo, configuración base e implementación de `bishul-common`.
- [ ] **Fase F1:** Desarrollo completo de `bishul-catalog-service` (mantenimiento de productos, categorías y stock).
- [ ] **Fase F2:** Implementación de `bishul-cart-service` y `bishul-order-service`.
- [ ] **Fase F3:** Implementación de **API Gateway** y servidor de autenticación dedicado (`bishul-user-service`).
- [ ] **Fase F4:**
  - Integración de **Apache Kafka** para mensajería asíncrona entre servicios (event-driven).
  - Tolerancia a fallos y resiliencia con **Resilience4j** (Circuit Breakers, Rate Limiters).
  - Auditoría global de operaciones críticas.

---

## 📊 Estado Actual del Proyecto

* **Fase Activa:** **`F0` — En progreso.**
* **Hito actual:** Construcción del módulo `bishul-common` y preparación de dependencias globales dentro del `pom.xml` padre.
