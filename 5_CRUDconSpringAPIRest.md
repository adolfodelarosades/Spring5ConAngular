## CRUD con Spring API Rest 01:16:01

### Descargar Código Fuente 00:03

:+1+

### Escribiendo los métodos del CRUD en la clase ClienteService del Backend 03:44

En esta sección vamos a implementar el CRUD en la clase ClienteService.

#### Declarar los contratos del CRUD

* Vamos a Eclipse y abrir la interfaz y **IClienteService** para implementar el contrato pada cada operación del CRUD.
* Empezamos por el método **findById** que recibe el `id` del Cliente a buscar y retorna un Cliente.  
`public Cliente findById(Long id);`
* Continuamos por el método **save** que va a recibir el Cliente a almacenar y va a retornar el Cliente guardado, el Cliente que ya contiene el `id`.
`public Cliente save(Cliente cliente);`
* Luego vamos a tener el método **delete** que recibe el `id` del Cliente a eliminar y va a retornar un `void`.
`public void delete(Long id);`

El código final para **IClienteService** queda así:

```java
package com.bolsadeideas.springboot.backend.apirest.models.services;

import java.util.List;
import com.bolsadeideas.springboot.backend.apirest.models.entity.Cliente;

public interface IClienteService {

	public List<Cliente> findAll();
	
	public Cliente findById(Long id);
	
	public Cliente save(Cliente cliente);
	
	public void delete(Long id);
}

```

#### Implementar los métodos de la Interfaz

Vamos a **ClienteServiceImpl** que ya nos esta marcando un error porque no estan implementados los métodos si damos en el foco, tenemos la opción **Add unimplemented methods** e inserta la implementación con lo que se quita el error:

```java
@Override
	public Cliente findById(Long id) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public Cliente save(Cliente cliente) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public void delete(Long id) {
		// TODO Auto-generated method stub
		
	}
```
Tenemos que añadir sus correspondientes **@Transactional** y lo que se retorna en cada caso:

* Para el caso del **findById** tiene que retornar `return clienteDao.findById(id)` pero esto es un opcional tenemos que completarlo con alguno de los métodos que maneja en este caso usaremos `orElse(null)` el cual le dice que si no encontro ningún Cliente con el id retorne null, por lo que queda así: `return clienteDao.findById(id).orElse(null);`.
* Para el caso del **save** retornamos `return clienteDao.save(cliente);`  el **save** retorna la Entidad que se este guardando en este caso el Cliente que es lo que necesitamos retornar por lo que todo esta correcto.
* Finalmente tenemos el **delete** el cual retorna un **void** por lo que no retorna nada pero debe ejecutarse `clienteDao.deleteById(id);`.

El código final para **ClienteServiceImpl** queda así:

```java
package com.bolsadeideas.springboot.backend.apirest.models.services;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.bolsadeideas.springboot.backend.apirest.models.dao.IClienteDao;
import com.bolsadeideas.springboot.backend.apirest.models.entity.Cliente;

//Anotar como Service para marcarla como una clase de Servicio
@Service
public class ClienteServiceImpl implements IClienteService{

	//Inyectar (Inyección de dependdencias) el ClienteDao
	@Autowired
	private IClienteDao clienteDao;
	
	@Override
	@Transactional(readOnly = true) //Permite manejar transacción en el método y como es un Select será sólo de lectura
	public List<Cliente> findAll() {
		//Como findAll() retorna un Iterable le hacemos un CAST
		return (List<Cliente>) clienteDao.findAll();
	}

	@Override
	@Transactional(readOnly = true)
	public Cliente findById(Long id) {
		return clienteDao.findById(id).orElse(null);
	}

	@Override
	@Transactional
	public Cliente save(Cliente cliente) {
		return clienteDao.save(cliente);
	}

	@Override
	@Transactional
	public void delete(Long id) {
		clienteDao.deleteById(id);
		
	}

}
```

### Escribiendo los métodos show y create en el Controlador Backend API Rest 05:26

Vamos a comenzar implementando el API REST, nuestro controlador con algunos métodos para mostrar por id y el método para crear.

* Vamos a **ClienteRestController**
* Implementemos el mostrar por id, es decir **findById**:
```java
@GetMapping("/clientes/{id}")
public Cliente show(@PathVariable Long id) {
	return clienteService.findById(id);
}
```

`@PathVariable` Anotación que indica que un parámetro de método debe estar vinculado a una variable de plantilla URI. 


* Vamos a implementar el crear el cual será de tipo POST.
```java
@PostMapping("/clientes")
public Cliente create(@RequestBody Cliente cliente) {
	return clienteService.save(cliente);
}
```
Pero cuando se este insertando el cliente debemos asignar la fecha de creación. Una forma de hacerlo sería hacerlo antes de realizar el **save**

```java
@PostMapping("/clientes")
public Cliente create(@RequestBody Cliente cliente) {
	cliente.setCreateAt(new Date());
	return clienteService.save(cliente);
}
```
Pero queda más elegante si nos vamos a la clase **Cliente Entity** y definimos el métodoo **prePersisten** que es un evento del ciclo de vida de las clases Entity, para asignar la fecha a la propiedad **createAt**:
```java
@PrePersist
public void prePersist() {
	createAt = new Date();
}
```
Nos quedaremos con esta segunda opción.
Por último queremos retornar un Status cuando se inserte nuestro Cliente para lo cual usamos la anotación `@ResponseStatus` y debemos indicar que Status queremos regresar por defaul siempre regresa OK status 200, pero en este caso queremos retornar un 201 por lo que la notación completa sería `@ResponseStatus(`  en el crédito vamos a anotar con Ripollet status responde estátus.

Http: estátus como código punto create acá vamos a cambiar el código a 201 que sea creado contenido en nuestra aplicación y por defecto cuando no se asigna el estatus.

Si todo sale bien el HTTP estátus va a quedar como Okkhoy con el código 200 por lo tanto si acá colocamos en el chow o en el listo índex que retorna el listado colocamos el responsable estátus y colocamos el OK.

Ese sería el código 200 por defecto.

Por lo tanto sería redundante colocar esta anotación ya que lo asigna por defecto si se realiza correctamente y entrega la respuesta.

Pero en este caso queremos retornar un 201 un Kraid para indicar que se ha creado contenido.

No vamos a dejar sin la anotación.

Bien eso sería todo y continuamos en la próxima clase para implementar el arte y el delito en nuestra

API res controles y cualquier duda que tengan la revisamos hasta la próxima.

### Escribiendo los métodos update y delete en el Controlador Backend API Rest 05:37

### Probando nuestro Backend API Rest con Postman 06:41

### Creando el componente form.component y la vista del formulario 10:46

### Configurando la ruta y navegación del formulario 04:55

### Escribiendo implementación crear en el cliente.service.ts y en form.component.ts 06:00

### Actualización: nueva versión de SweetAlert2 8.0.1 o superior 00:39

### Instalar SweetAlert2 para enviar mensajes de alerta en el cliente 05:06

### Cargando los datos en el formulario para actualizar 06:28

### Escribiendo el update en el cliente.service.ts y en form.component.ts 07:02

### Escribiendo el delete en la clase service y en el componente clientes 07:37

### Overflow en listado de clientes, ajustando layout 03:16

### Validando los clientes en la tabla HTML con directiva ngIf 02:41
