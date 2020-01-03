# Spring5ConAngular

## 4. Backend: Spring API REST 01:28:02

**REST** REpresentational State Transfer Transferencia de Estado Representacional.

Es un protocolo entre cliente y servidor sin estado (Stateless), en el BackEnd no se maneja ningún tipo de sesiones eso se manejara en el Front End usando el SessionStorage o LocalStorage de HTML5.

Una de las aplicaciones principales es integrar aplicaciones utilizando REST para **obtener datos** desde el BackeEnd en formato JSON a un cliente o **enviar datos** desde cualquier cliente al BackeEnd y realizar operaciones CRUD en la BD.

La transferencia y envio de datos se realiza a través de un **ENDPOINT** que es una URL o URI que envía una petición HTTP al servidor con diferentes métodos o verbos del Request, por ejemplo:

Verbos | URI                | Action o Handler
-------|--------------------|------------
GET    |/clientes           | index()
GET    |/clientes/create    | create()
POST   |/clientes           | storre()
GET    |/clientes/{id}      | show()
GET    |/clientes/{id}/edit | edit()
PUT    |/clientes/{id}      | update()
DELETE |/clientes/{id}      | destroy()


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
* Seleccionamos `Spring Starter Project` y se nos pediran varios datos
   * Name
   * Type
      * Maven (Usa una estructura XML)
      * Gradle (Usa una estructura DSL (domain-specific language) o lenguaje de dominio específico)
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

:+1: 

### Creando Proyecto Backend API REST 10:23

Para crear un proyecto nuevo seguir los siguientes pasos:

* Seleccionar `File / New / Other...`
* Seleccionar `Spring Boot` y se nos presentan dos opciones:
   * `Import Spring Started Content`
   * `Spring Starter Project`
* Seleccionamos `Spring Starter Project` y se nos pediran varios datos
   * Name: **spring-boot-backend-apirest**
   * Type: **Maven**
   * Packaging: **Jar**
   * Java Version: **8**
   * Language: **Java**
   * Group: **com.bolsadeideas.springboot.backend.apirest**
   * Package: **com.bolsadeideas.springboot.backend.apirest**
* Presionamos el botón **Next**, para ir a la ventana de Dependencias o Librerías **New Spring Starter Project Dependencies**
   * Spring Boot Version: **2.2.2** ( La versión más estable hasta el momento**
   * Seleccionar **Web** 
      * Marcar **Spring Web** (Contiene el API, las anotaciones y controladores para crear nuestro REST
   * Seleccionar **SQL**
      * Marcar la dependencia **Spring Data JPA**
      * Marcar la dependencia **MySQL Driver**
   * Seleccionar **Developer Tools**
      * Marcar **Spring Boot DevTools** (Para actualizar automáticamente el deploy cuando se realicen cambios)
* Dar click en **Finish**

Una vez hecho esto se genera la estructura de nuestro proyecto, algunos archivos importantes son:
   
   * **pom.xml**: Contiene la estructura de nuestro proyecto. (Con la información que se metio al crear el proyecto)
   * **application.properties**: Archivo principal de configuración. Permite sobreescribir cualquier configuración del proyecto. (Actualmente vacío)
   * **SpringBootBackendApirestApplication**: Clase principal, es el **Boot Start** el arranque, una clase que se crea de forma automática en el package base de nuestro proyecto. Contiene lo siguiente:
      * **@SpringBootApplication**: anotación más importante de la aplicación 
   
#### Ejecutar nuestro proyecto

* Colocarnos en la raiz del proyecto
* Pulsar click derecho
* Seleccionar `Run As / Spring Boot App`

Nos marcara algún error por que aún falta configurar el proyecto.

### Configurando el Datasource a MySQL en el proyecto backend 06:46

Ir a **application.properties** e insertar el siguiente codigo:
 
 ```
spring.datasource.url=jdbc:mysql://localhost/db_springboot_backend?usesSSL=false&serverTimezone=UTC&useLegacyDatetimeCode=false
spring.datasource.username=root
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL57Dialect
spring.jpa.hibernate.ddl-auto=create-drop
logging.level.org.hibernate.SQL=debug
 ```

### Instalando MySQL 04:12

Ir a [MySQL](https://www.mysql.com/), vamos a [MySQL Community (GPL) Downloads](https://dev.mysql.com/downloads/) Seleccionamos MySQL Community Server 8.0.18 y descargamos la versión completa para nuestro Sistema Operativo. 
Ejecutarlo para instalar:
* MySQL Server
* Workbeanch

### Creando la Base de Datos 03:11

Vamos a crear la BD `db_springboot_backend` con comandos seria así:

```
> mysql -u root -p
Enter password:
...
MySQL [(none)]> CREATE DATABASE db_springboot_backend;

MySQL [(none)]> show databases;
```

#### Probar la conexión

Una vez creada la BD podemos crear la conexión ejecutando la aplicación. `Run As / Spring Boot Ass`. 
Se levanta el servidor sin errores y en la **consola** podemos ver el dialecto que esta utilizando **Using dialect: org.hibernate.dialect.MySQL57Dialect** es el que configuramos.

#### Abrir la BD en Workbeanch

Podemos abrir la BD creada en Workbeanch, actualmente no tendra tablas pero ya las crearemos.

### Añadiendo la clase Entity Cliente al Backend 08:20

Vamos a crear la clase **Entity Cliente**, la idea es que esta clase este mapeada a la tabla Clientes y represente la persistencia o datos de los clientes por el lado del servidor. Vamos a seguir los siguientes pasos:

* Crear dentro del package principal el package **models.entity**.
* Dentro del nuevo package creamos la clase **Cliente**
* Declaramos las propiedades de la clase:
```java
public class Cliente {
	
	private Long id;
	private String nombre;
	private String apellido;
	private String email;
	private Date createAt;

}
```
* Añadimos los `Getters` y `Setters` de todas las propiedades:
```java
public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public String getApellido() {
		return apellido;
	}

	public void setApellido(String apellido) {
		this.apellido = apellido;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public Date getCreateAt() {
		return createAt;
	}

	public void setCreateAt(Date createAt) {
		this.createAt = createAt;
	}

```
* El siguiente paso es convertir esta clase en una clase **Entity**, en una clase de persistencia que esta mapeada a una tabla de una BD, cada atributo de la clase corresponde a un campo en la tabla, los pasos son:
   * Implementar la **interface Serializable**:    `public class Cliente implements Serializable {`
   * Crear el **default serial version ID** pulsando el foco amarillo que sale: `private static final long serialVersionUID = 1L;` es un atributo estatico que es requerido cuando se implementa el serializable.
   * Marcar la clase para indicar que se trata de una clase Entity con: `@Entity` importarla de `javax.persistence`
   * La siguiente anotación no seria necesaria si la Tabla y la Clase se llaman igual pero en este caso no sera así (por que la tabla se llama **clientes**: `@Table(name="clientes")`
   * La siguiente anotación nos va a permitir indicar que nuestra propiedad id corresponde a la clave primaria: `@Id`
   * También debemos indicar como se genera o cual es la estrategia de generación de esta llave en la BD: `@GeneratedValue(strategy=GenerationType.IDENTITY)` para MySQL es IDENTITY, para Oracle es SeQUENCE.
   * Para las propiedades **nombre**, **apellido** e **email** van a representar columnas por lo que se podrían anotar con `@Column` sin embargo, cuando el nombre del campo y la propidad son iguales se puede omitir. Se utilizaría si los nombres no son iguales, o para indicar la longitud, para indicar si acepta nulos, etc. Para estas propiedades no lo usaremos.
   * Para la propiedad **createAt** si usaremos la anotación `@Column()` como sigue: `@Column(name="create_at")`
   * También le aplicaremos la antoación `@Temporal()` para indicar cual va a ser la transformación o tipo equivalente en la BD: `@Temporal(TemporalType.DATE)` transforma la fecha de Java a la de SQL. 
   
Eso es todo, ya tenemos nuestra clase Entity, que esta mapeada parte del contexto de persistencia de JPA, por lo tanto esta sincronizada con la BD:

```java
package com.bolsadeideas.springboot.backend.apirest.models.entity;

import java.io.Serializable;
import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

@Entity
@Table(name="clientes")
public class Cliente implements Serializable {

	
	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	private Long id;
	
	private String nombre;
	private String apellido;
	private String email;
	
	@Column(name="create_at")
	@Temporal(TemporalType.DATE)
	private Date createAt;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public String getApellido() {
		return apellido;
	}

	public void setApellido(String apellido) {
		this.apellido = apellido;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public Date getCreateAt() {
		return createAt;
	}

	public void setCreateAt(Date createAt) {
		this.createAt = createAt;
	}
	
	private static final long serialVersionUID = 1L;

}
```

Vamos a ejecutar el proyecto para ver como se genera la tabla.
Si vemos el log nos indica:

```
2020-01-02 18:52:40.014 DEBUG 21596 --- [  restartedMain] org.hibernate.SQL                        : drop table if exists clientes
2020-01-02 18:52:40.028 DEBUG 21596 --- [  restartedMain] org.hibernate.SQL                        : create table clientes (id bigint not null auto_increment, apellido varchar(255), create_at date, email varchar(255), nombre varchar(255), primary key (id)) engine=InnoDB
```
Esta boorando la tabla si existe y luego la crea con todas las caracteristicas en los campos que se definieron en la clase.

Podemos abrir el Workbeanch y ver nuestra tabla desde allí. Cabe aclarar que el orden de los campos es alfabetico a excepción del id por ser clave primaria.

`id   apellido   create_at   email   nombre`

### Añadiendo las clases Repository y Service de la lógica de negocio 11:48

Vamos a comenzar creando la clase de acceso a datos DAO o Repository la cual tiene como función acceder a los datos para realizar consultas y todo tipo de operaciones en la BD. 

Después crearemos la clase Service que en el fondo puede contener a las clases DAO que interactuan todas bajo una misma transacción, el objetivo principal del Service es evitar ensuciar el Controlador con las clases DAO, simplemente se desacopla y se lleva a una fachada.

Hay diferentes formas de implementar un DAO, podemos crear una clase, 

Podemos trabajar directamente con el Entity manager con JPA implementar todo de forma manual pero es

Brink tiene un componente bastante potente robusto que se llama esprín data JPA y esto nos ahorra un

montón de trabajo de tener que estar implementando todos métodos y consultas de internet de JPA de forma

manual.

Simplemente se implementa una interfaz heredamos de la interfaz repositor y prácticamente estamos listos

ya que todos los métodos básicos para un club para un club para poder listar para buscar por ahí para

modificar para guardar y para eliminar.

Y además si queremos podemos implementar nuestros propios métodos customizados usando la notación qwerty

o también utilizando el nombre el método cosa que vamos a ver un poco en esta clase.

Pero vamos a implementar primero la interfaz dado entonces con un clic derecho en el package modo dado

y nos vamos a interfaz y le damos el nombre y Killen te damos finalizar simplemente una interfaz que

no tiene implementación y acá vamos a extender de Cruz repositorio acá propone un generic tipo genérico

acá tenemos el nombre de la clase Entity que está asociado a este dado a este repositorio sería el cliente

la importamos justamente la clase que creamos el video anterior sería el tipo de dato de la llave Heydi

loncco y hoy tenemos implementado nuestro Dawa de acceso a datos y acá mismo te hacemos por ejemplo

con control click nos vamos al detalle a los métodos de esta interfaz de interfaz para generar operaciones

Grout genéricas de un repositorio de una clase de acceso datos.

Acá tenemos el método sait que recibe un genérico de una entidad.

Acá tenemos guardar varias entidades buscar por Heydi retorna un optional un optional una clase un tipo

de dato que nos permite manejar mejor el resultado.

Por ejemplo si se realizó bien la consulta va a retornar el objeto con ket.

Si no podemos manejar el error con manejo decepción o bien retornar Null.

En fin hay distintas alternativas.

Luego tenemos si existe Prandi para comprobar buscar todas.

Find all retorna un terabyte.

La interfaz de colección Effy.

Hay un montón de cosas con eliminar por Heydi eliminar por un objeto entidad y varios métodos más.

Es una interfaz bien interesante que nos propone esprinta atajos tapear.

Incluso podríamos revisar la documentación la página Project esprín esprinta JPA nos vamos acá a referencia.

Documentación y acá tenemos concepto del core una interfaz repositor tal como la vimos y esta interesante

porque va a realizar las consultas y operaciones de acuerdo al nombre el método.

Si tenemos el nombre se va a realizar un persiste en JPA.

Por lo tanto hacer un insert a la Hayato si hacemos un fi por ahí va a realizar una consulta Selleck

a la clase entity.

En nuestro caso cliente Wer el atributo Didí sea igual al parámetro entonces de acuerdo al nombre el

método va a realizar la consulta incluso más arriba en la documentación.

Por acá tenemos definiendo mi método definiendo un método entonces a través del nombre Método haca podemos

realizar consulta en la interfaz podemos implementar métodos personalizados por ejemplo con fines para

hacer un Selic Boy es para Wer y Haunt es para el Wer aunt entonces sería Selleck la clase Entity Wer

y mail address igual a este parámetro an las es igual a este parámetro y así también tenemos para el

distinctive.

En fin acá podemos buscar y realiza consulta de acuerdo al nombre del método y también tenemos por ejemplo

operadores tales como el Bitcoin por ejemplo esto operadores se pueden utilizar en el nombre del método

les dan menor que Gray dan mayor que Lykke un Wer Lykke.

Otra alternativa aparte de realizar consulta a través del nombre Método podemos utilizar una anotación

la notación QWERTY si nos vamos a JPA repositorios usando query la notación QWERTY.

También podemos tener un método con algún nombre que le queremos dar y lo notábamos con qwerty y acá

colocamos la consulta de JPA o Bernet.

Recordemos que esas consultas son H cueles es decir de Internet Query Language orientada a objetos no

son de tablas y acá pasamos un parámetro un nombre parámetro con el signo pregunta y el índice que sería

el primer parámetro.

Y acá lo pasamos por argumento Tuset internamente va a pasar el valor del email address.

De este argumento al Quay bien.

Entonces volviendo al cliente dado que contamos con los métodos que lo estamos heredando los métodos

para el club por lo tanto tenemos implementado nuestro dado prácticamente de forma automática utilizando

Grub repositorio.

El siguiente paso es crear la clase servis entonces vamos a crear un nuevo package Models punto servicios.

Vamos a finalizar y en servicios vamos a crear una interfaz y cliente Service y esta interfaz le vamos

a dar un contrato de implementación.

Los métodos del club por ejemplo para alistar para buscar por ahí guardar editar y eliminar Vamos primero

con el fin al

cliente.

Hoy importamos Liseth de Java útil importamos cliente y por ahora vamos a dejar solamente implementado

el final y después vamos a ir implementando lo demás métodos del servis para realizar.

Entonces ahora vamos a crear la clase servis y vamos a implementar este método.

Vamos a crear entonces la clase servis

cliente servis implement de implementación.

Le ponemos imploro finaliza entonces implement

y cliente servis hakama un error que nos pide implementar el método final.

Entonces acá tenemos que inyectar usando atu Wired inyección de dependencia en sprint la vamos a importar.

Vamos a inyectar el cliente dado entonces es un Privat

importamos el cliente Davo y le llevamos al atributo cliente el agua.

Entonces en el método final acabamos a ocupar a utilizar este atributo para acceder a los clientes a

la lista de clientes.

Entonces cliente dado fin Holl.

Este método retorna un terabyte por lo tanto acá tenemos que convertir a un listo.

Hacemos un cast de List de cliente y ya está terminado implementado el método.

Luego tenemos que anotar con transaccional

esta notación nos permite manejar transacción en el método transaccional de transacción anotación y

como es una consulta su Select sería solamente de lectura.

Acá colocamos Read Only igual bien ahora de todas formas los métodos del crudo repositorios ya vienen

con transaccionalidad ya son transaccionales.

Por lo tanto podríamos omitir esta notación.

Ahora yo prefiero anotarla en el servis ya que describe la transaccionalidad de la clase repositor y

más que nada para tener el control y hacerlo de una forma más explícita.

Pero de todas formas se puede omitir así que daría exactamente igual ahora lo que si todos los métodos

nuevos que queramos implementar en el cliente dado ya sea a través del nombre Método o utilizando la

notación QWERTY ahí tendríamos que utilizar el Transactions solamente para los métodos propios pero

los que ya vienen dentro como el SAI como el fin por ahí fenol etcétera.

Estos métodos ya vienen con transaccion.

También quería destacar el autor Wired una notación para inyectar el quién te daba el cliente dado.

A pesar de que es una interfaz pero por detrás de escena exprima crear una instancia de una implementación

concreta utilizando la interfaz iba a quedar guardada en el contenedor de sprint en el contexto.

Por lo tanto la podemos inyectar en el otro componente ya sea una clase servis ya sea en un controlador

en cualquier clase de nuestra aplicación.

Qué más queda para finalizar faltaría anotar con servis

una actuación muy importante ya que con esto decoramos y marcamos esta clase como un componente de servicio

en sprint y también se va a guardar en el contenedor de sprint que va a quedar almacenado en el contexto.

Y después podemos inyectar este objeto.

Este Vins de sprint en el controlador y lo podemos utilizar pero para eso tenemos que decorarlo y servis

lo que hace es justamente es un tipo un estereotipo de component.

Por lo tanto con componentes marca la clase la decora para que sea un componente del frente un Vins

y se registra en el contenedor vamos a guardar y ya tenemos implementar nuestra clase serves bien y

estamos listo con nuestra lógica negocio con toda la clase del modelo y la próxima clase continuamos

con el controlador resto quedamos hasta acá y cualquier duda que tengan las revisamos hasta la próxima.

Spring cuenta con un componente bastante robusto llamado **Spring Data JPA** 
Pasos para crear el Repository o DAO:

* Crear el package **models.dao** dentro del package principal.



### Creando controlador @RestController y EndPoint para listar 04:22
### Añadiendo Datos de pueba 02:54
### Usando Postman para probar nuestras APIs 04:09
### Uso de Cors para compartir recursos en API REST 04:02
### Implementando Servicio Angular con HttpClient 09:28
