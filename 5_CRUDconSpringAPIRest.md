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

Si todo sale bien el HTTP estátus va a quedar como Ok, con el código 200, pero en este caso queremos retornar un Status 201 que representa un CREATE, por lo que usaremos la anotación `@ResponseStatus(HttpStatus.CREATED)`. Por lo que el método `create` completo queda así:
```java
@PostMapping("/clientes")
@ResponseStatus(HttpStatus.CREATED)
public Cliente create(@RequestBody Cliente cliente) {
	return clienteService.save(cliente);
}
```

Lo que es redundante es colocar la anotación `@ResponseStatus(HttpStatus.OK)` por que es la que regresa por default.

### Escribiendo los métodos update y delete en el Controlador Backend API Rest 05:37

Ahora vamos a implementar el actualizar y el eliminar.

* Para actualizar utilizaremos la anotación `@PutMapping` Nuestro método `update` queda así:

```java
@PutMapping("/clientes/{id}")
@ResponseStatus(HttpStatus.CREATED)
public Cliente update(@RequestBody Cliente cliente, @PathVariable Long id) {

	// Recupero el Cliente de la BD por su ID
	Cliente clienteActual = clienteService.findById(id);

	// Actualizo el clienteActual con los datos del Cliente que me pasan de parámetro
	clienteActual.setApellido(cliente.getApellido());
	clienteActual.setNombre(cliente.getNombre());
	clienteActual.setEmail(cliente.getEmail());

	// Salvo el clienteActual que ya esta modificado y retorno el Cliente actualizado.
	return clienteService.save(clienteActual);
}
```
Este método recibe dos parámetros un `cliente` y un `id` el cual no permite recuperar de la BD el cliente con ese id que vamos a modificar con los datos del cliente que se nos pasa como parámetro. Una vez que se actualiza el clienteActual, se salva en la BD y se retorna el cliente actualizado.

Para actualizar los datos estamos usando el método `save` igual que cuando lo insertamos, este método `save` sirve tanto para hacer un insert como para un update. Si el id es Null o cero estamos hablando de un nuevo objeto que se inserta pero cuando existe el id lo que se hace es un update, por lo tanto lo va a actualizar en vez de insertar.

* Para eliminar usaremos la anotación `@DeleteMapping`, nuestro método queda así:
```java
@DeleteMapping("/clientes/{id}")
@ResponseStatus(HttpStatus.NO_CONTENT)
public void delete(@PathVariable Long id) {
	clienteService.delete(id);
}
```
Se recibe el parámetro `id` y con ese eliminamos el registro usando el método `delete` del `Service`, en este caso no se retorna nada. Con la anotación `@ResponseStatus(HttpStatus.NO_CONTENT)` retornamos un `204 No Content.`

Ya tenemos completamente terminado nuestra API REST con nuestro Controlador terminado. El CÓDIGO COMPLETO DEL CONTROLADOR es: 

```java
package com.bolsadeideas.spring.boot.backend.apirest.controllers;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

import com.bolsadeideas.spring.boot.backend.apirest.models.entity.Cliente;
import com.bolsadeideas.spring.boot.backend.apirest.models.services.IClienteService;

@RestController
@RequestMapping("/api")
@CrossOrigin(origins = {"http://localhost:4200"})
public class ClienteRestController {
	
	@Autowired
	private IClienteService clienteService;
	
	
	@GetMapping("/clientes")
	public List<Cliente> index(){
		return clienteService.findAll();
	}
	
	@GetMapping("/clientes/{id}")
	public Cliente show(@PathVariable Long id) {
		return clienteService.findById(id);
	}
	
	@PostMapping("/clientes")
	@ResponseStatus(HttpStatus.CREATED)
	public Cliente create(@RequestBody Cliente cliente) {
		return clienteService.save(cliente);
	}
	
	@PutMapping("/clientes/{id}")
	@ResponseStatus(HttpStatus.CREATED)
	public Cliente update(@RequestBody Cliente cliente, @PathVariable Long id) {
		
		// Recupero el Cliente de la BD por su ID
		Cliente clienteActual = clienteService.findById(id);
		
		// Actualizo el clienteActual con los datos del Cliente que me pasan de parámetro
		clienteActual.setApellido(cliente.getApellido());
		clienteActual.setNombre(cliente.getNombre());
		clienteActual.setEmail(cliente.getEmail());
		
		// Salvo el clienteActual que ya esta modificado y retorno el Cliente actualizado.
		return clienteService.save(clienteActual);
	}
	
	@DeleteMapping("/clientes/{id}")
	@ResponseStatus(HttpStatus.NO_CONTENT)
	public void delete(@PathVariable Long id) {
		clienteService.delete(id);
	}
}
```

### Probando nuestro Backend API Rest con Postman 06:41

El siguiente paso es probar nuestra API REST con Postman, en nuestro controlador tenemos los distintos métodos:

* Listar todos los clientes que es una petición del tipo **get @GetMapping("/clientes")**
* Obtener un Cliente por id que también es una petición del tipo **get @GetMapping("/clientes/{id}")**
* Crear un nuevo cliente que es una petición del tipo **post @PostMapping("/clientes")** 
* Modificar un cliente que es una petición de tipo **put @PutMapping("/clientes/{id}")** 
* Eliminar un cliente que es una petición de tipo **delete @DeleteMapping("/clientes/{id}")**

Nos vamos a **Postman** para probar:

* `GET http://localhost:8080/api/clientes`		**Status: 200 OK**
* `GET http://localhost:8080/api/clientes/5`		**Status: 200 OK**
* `POST http://localhost:8080/api/clientes`		**Status: 201 CREATED**
En `Body > Raw > JSON `  (Es el Cliente a insetar, lo que recibe de parámetro)
```js
{
    "nombre": "Adolfo",
    "apellido": "De la Rosa",
    "email": "adolfodelarosa@gmail.com"
        
}
```
* `PUT http://localhost:8080/api/clientes/14`		**Status: 201 CREATED**
En `Body > Raw > JSON `  (Es el Cliente a insetar, lo que recibe de parámetro)
```js
{
    "nombre": "Edgar",
    "apellido": "De la Rosa",
    "email": "edgardelarosa@gmail.com"
        
}
```
Retorna:
`{"id":14,"nombre":"Edgar","apellido":"De la Rosa","email":"edgardelarosa@gmail.com","createAt":"2020-01-13"}`
* `DELETE http://localhost:8080/api/clientes/14`		**Status: 204 No Content**


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
