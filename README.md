# Spring5ConAngular

## 4. Backend: Spring API REST 01:28:02

**REST** REpresentational State Transfer Transferencia de Estado Representacional.

Es un protocolo entre cliente y servidor sin estado (Stateless), en el BackEnd no se maneja ningún tipo de sesiones eso se manejara en el Front End usando el SessionStorage o LocalStorage de HTML5.

Una de las aplicaciones principales es integrar aplicaciones utilizando REST para **obtener datos** desde el BackeEnd en formato JSON a un cliente o **enviar datos** desde cualquier cliente al BackeEnd y realizar operaciones CRUD en la BD.

La transferencia y envio de datos se realiza a través de un **ENDPOINT** que es una URL o URI que envía una petición HTTP al servidor con diferentes métodos o verbos del Request, por ejemplo:

Encabezado | Encabezado | Encabezado
-----------|------------|------------
1|2|3


### Descargar Código Fuente 00:03

:+1:

### Demostración de lo que lograremos al finalizar las siguientes secciones 02:33

* CRUD con paginación
* Insertar, Editar y Eliminar Clientes
* Ver detalles del perfil con un modal
* Subir una imágen
* Uso de Date Picker
* Relaciones de tablas

### Herramientas necesarias Backend 06:46

* JDK (Java SE Development Kit (JDK(Desarrollo) + JRE(Ejecución))
* IDE Eclipse + Spring Tools
* Maven
* MySQL
* Postman

#### Instalar JDK

[Download JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

```
java -version
java version "1.8.0_231"
```

#### Instalar Eclipse

[Eclipse](https://www.eclipse.org/)

[Download Packages](https://www.eclipse.org/downloads/packages/)

Debemos descargar [Eclipse IDE for Enterprise Java Developers](https://www.eclipse.org/downloads/packages/release/2019-12/r/eclipse-ide-enterprise-java-developers)

### Instalación y configuración del IDE Eclipse 06:40

Debemos descomprimir el ZIP descargado y ejecutar el archivo **eclipse.exe**. Nos pide el lugar de nuestro workspace.

#### Instalar Spring Tools 4 (4.5.0 Release)

* Ir `Help / Eclipse Marketplace` y en `Find` escribir `Spring` y `Enter`, en el listado nos saldra **Spring Tools 4 - for Spring Boot (aka Spring Tool Suite 4) 4.5.0 RELEASE** pulsamos en el botón `Install`.
* Con todo seleccionado pulsamos en `Confirm`.
* Aceptamos las licencias, terminos y pulsamos el botón `Finish`.
* Se instala el software y nos pide reiniciar Eclipse.

Con todo esto hecho en la barra de herramientas nos aparecerá el icono de **Boot Dashboard**
* Pulsamos en el icono y nos aparece la pestaña **Boot Dashboard** 

En esta pestaña tendremos nuestro proyecto Spring Boot.

#### Crear proyecto Spring

Para crear un proyecto Spring podemos seguir los siguientes pasos:

* Seleccionar `File / New / Other...`
* Seleccionar `Spring Boot` y se nos presentan dos opciones:
   * `Import Spring Started Content`
   * `Spring Starter Project`
* Seleccionamos `Spring Starter Project` y se nos pidiran varios datos
   * Name
   * Type
      * Maven
      * Gradle
   * Packaging
      * Jar (Proyectos Spring que no utlizan parte visual (JSPs))
      * War (Proyectos Spring con parte visual o que se desplegaran en un servidor externo)
   * Java Version
   * Language
      * Java
      * Kotlin
      * Groovy

#### Eclipse configurado por el equipo de Spring

Existe un Eclipse ya configirado con Spring Boot creado por el equipo de [Spring](https://spring.io/), hasta el final de la página tenemos el [link de tools](https://spring.io/tools) donde se nos permite descargar un Eclipse ya configurado con Spring Boot el cual es muy similar al que instalamos a mano.

### Actualización: Wizard para seleccionar dependencias en Spring Tools 02:25

: +1: 

### Creando Proyecto Backend API REST 10:23


### Configurando el Datasource a MySQL en el proyecto backend 06:46
### Instalando MySQL 04:12
### Creando la Base de Datos 03:11
### Añadiendo la clase Entity Cliente al Backend 08:20
### Añadiendo las clases Repository y Service de la lógica de negocio 11:48
### Creando controlador @RestController y EndPoint para listar 04:22
### Añadiendo Datos de pueba 02:54
### Usando Postman para probar nuestras APIs 04:09
### Uso de Cors para compartir recursos en API REST 04:02
### Implementando Servicio Angular con HttpClient 09:28
