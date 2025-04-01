# Spring Cloud Config Server: Gestión Centralizada de Configuración para Microservicios

Este repositorio contiene la configuración centralizada para nuestros microservicios, gestionada a través de **Spring Cloud Config Server**. El uso de un Config Server es fundamental en una arquitectura de microservicios por las siguientes razones:

**Importancia de Utilizar Spring Cloud Config Server:**

* **Centralización de la Configuración:** Permite almacenar toda la configuración de tus microservicios en un único lugar versionado. Esto facilita la gestión, el seguimiento de cambios y la consistencia entre entornos.
* **Gestión de Entornos:** Facilita la definición de configuraciones específicas para diferentes entornos (desarrollo, pruebas, producción, etc.) utilizando convenciones de nomenclatura claras (como los sufijos `-dev`, `-prod`).
* **Consistencia:** Asegura que todos tus microservicios utilicen la configuración correcta para el entorno en el que se están ejecutando, reduciendo errores y comportamientos inesperados.
* **Seguridad:** Permite gestionar información sensible (como claves de API, contraseñas de bases de datos) de forma segura, potencialmente integrándose con soluciones de gestión de secretos como HashiCorp Vault.
* **Actualización Dinámica de la Configuración:** En muchos casos, los cambios en la configuración pueden aplicarse a los microservicios en tiempo real (sin necesidad de reiniciarlos), lo que mejora la agilidad y reduce el tiempo de inactividad.
* **Separación de la Configuración del Código:** Cumple con los principios de las aplicaciones de doce factores, separando la configuración del código fuente, lo que facilita la gestión y el despliegue.
* **Versionado:** Al utilizar un sistema de control de versiones como Git (comúnmente utilizado con Spring Cloud Config Server), se obtiene un historial completo de los cambios en la configuración, lo que facilita la auditoría y la reversión a versiones anteriores si es necesario.

**Aplicación en tus Microservicios:**

Para que tus microservicios utilicen la configuración gestionada por este Spring Cloud Config Server, debes seguir los siguientes pasos en cada uno de ellos:

**1. Agregar la Dependencia de Spring Cloud Config Client:**

En el archivo `pom.xml` (para Maven) o `build.gradle` (para Gradle) de cada microservicio, agrega la dependencia de Spring Cloud Config Client:

**Maven (`pom.xml`):**

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```


**2. Configurar el cliente de Configuración:**

Cada microservicio necesita saber la ubicación de tu Spring Cloud Config Server. Esto se configura en el archivo bootstrap.properties o bootstrap.yml (este archivo se carga antes que application.properties o application.yml).

```

spring:
  application:
    name: order-service # El nombre de la aplicación debe coincidir con el prefijo de tus archivos de configuración (ej: order-service-dev.yml)
  cloud:
    config:
      uri: http://<direccion-del-config-server>:<puerto-del-config-server> # Reemplaza con la URL de tu Config Server
      # profile: dev # El perfil se puede especificar aquí o a través de una variable de entorno/argumento de JVM
      # label: main # Opcional: la rama o etiqueta de Git a utilizar (por defecto es 'main' o 'master')

```

**3. Utilizar la configuración en tus microservicios:**


Una vez configurado el cliente, las propiedades definidas en los archivos de este repositorio estarán disponibles en el Environment de tu microservicio. Puedes acceder a estas propiedades utilizando la anotación @Value:

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class OrderServiceConfig {

    @Value("<span class="math-inline">\{order\.service\.api\.url\}"\)</2\>
private String apiUrl;
@Value\("</span>{order.service.max.items}")
    private int maxItems;

    public String getApiUrl() {
        return apiUrl;
    }

    public int getMaxItems() {
        return maxItems;
    }
}
      
