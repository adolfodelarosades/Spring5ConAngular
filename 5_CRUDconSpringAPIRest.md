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

Bueno lo primero es agregar el transaccional vamos a agregar.

En fin por aquí solamente de lectura en el Sai vamos a tener el transaction al completo y lo mismo para el delito.

Y acá es bastante simple porque el fin por allí vamos a invocar al cliente dado al repositorio y le vamos a invocar el método Bucca por ende éste va a retornar un optional ya que estamos trabajando con principesco y la gracia del optional que nos permite manejar el contenido el contenido de consulta por ejemplo si está presente el resultado de la búsqueda por allí.

Por ejemplo acá tenemos varios métodos acá tenemos el jet que si lo encuentra va a retornar el objeto pero si no lo encuentra va a retornar una excepción nos Satch Element sp3 es decir que no encuentra el elemento exceptúan acá tenemos una opción si no lo encuentra.

Entonces retornamos un nulo por ejemplo si tenemos o bien sino lanzar una sección personalizada acá podemos preguntar si está presente.

En fin acá podemos filtrar.

Acá tenemos el mapa que nos permite modificar el resultado.

Tenemos varias opciones.

Por ejemplo vamos a utilizar el or else y acá simplemente Nul.

Entonces si lo encuentra retorna el objeto cliente.

De lo contrario va a retornar un 9.

Luego tenemos el SAIC que sería cliente dado .6 y el SOIB va a retornar la entidad guardada en la Shehata que contiene el Heydi le pasamos el cliente.

Finalmente tenemos el delito el delito es un cliente dado punto Delight por Heydi.

Acá tenemos dos el delito que recibe una entidad que está tachada en el contexto de persistence por lo tanto está dentro de la sección de JPA del contexto.

Acá tenemos el delito por ahí simplemente le pasamos una Heydi internamente va a obtener la entidad el cliente y lo va a eliminar de forma automática.

Eso de todo ya tenemos implementado nuestro Cruz nuestra clase servis y continuamos con el controlador en la próxima clase.

Quedamos hasta acá y cualquier duda la revisamos en el foro hasta la próxima.




### Escribiendo los métodos show y create en el Controlador Backend API Rest 05:26

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
