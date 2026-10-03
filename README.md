# SuperBishul — v2.0

> **De plataforma monolítica a arquitectura de microservicios distribuida.**
> Modernización integral de e-commerce de restauración, orientada a alta escalabilidad, desacoplamiento de dominios y adopción de estándares modernos con **Spring Boot 4** y **Spring Security 7**.

---

## 📌 Descripción del Proyecto

**SuperBishul** es la evolución de una plataforma monolítica de comercio electrónico enfocada en el sector de la restauración. El objetivo central de esta v2.0 es refactorizar la arquitectura original dividiéndola en un ecosistema de microservicios autónomos, profesionalizando la base de código y adaptando las configuraciones a las especificaciones y breaking changes de **Spring Boot 4** y **Spring Security 7**.

### Objetivos Clave de la v2.0
* **Descomposición del Monolito:** Separación clara de responsabilidades en dominios independientes (`User`, `Catalog`, `Order`, `Cart`).
* **Estandarización Transversal:** Centralización de modelos comunes, seguridad JWT, gestión global de errores y utilidades en el módulo `bishul-common`.
* **Seguridad Declarativa:** Implementación del modelo de seguridad declarativo de **Spring Security 7**, basado exclusivamente en lambdas (sin `WebSecurityConfigurerAdapter`, sin `and()`, sin `antMatchers()`), adaptado a los breaking changes de la versión.
* **Evolución Progresiva:** Migración por fases (Fase F0 en curso), garantizando contratos estables antes del despliegue de nuevos servicios.

---

## 🏗️ Arquitectura y Estructura del Proyecto

### Visión General de Servicios y Dependencias Funcionales

```text
                        +----------------------------+
                        |       bishul-common        |
                        | (DTOs, Security, Errors)   |
                        +--------------+-------------+
                                       ^
                                       | (dependencia Maven)
     +-----------------+---------------+---------------+-----------------+
     |                 |                               |                 |
+----+----+    +-------+-------+               +-------+-------+   +-----+-----+
|  User   |    |    Catalog    |<--------------+     Cart      |   |   Order   |
| Service |    |    Service    |               |    Service    |<--+  Service  |
+---------+    +-------+-------+               +---------------+   +-----+-----+
                       ^                                                  |
                       |                                                  |
                       +--------------------------------------------------+
                              (Order también consulta Catalog)
