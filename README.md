# Coordinator(BC1)

El componente Cooordinador se desarrollo bajo el lenguage java, con el fin de coordinar a los diferentes componentes del proyecto.

## Contenido

Dentro del desarrollo de componente se crearon diferentes clases y métodos los cuales se especifican mas a detalle en el el repositorio [javadoc_coordinator ](https://pip.pypa.io/en/stable/)

### Clase coordinator

En esta clase se encuentra diferentes servicios:

#### Servicio Paso de Saldo

Este servicio es de tipo post, es consumido por el componente Wallet para realizar una transferencia de saldo de una cuenta origen a una cuenta destino. Se recibe un archivo tipo json con lola información dirección de origen (dir_origen), dirección destino (dir_destino) y la cantidad de dinero a transferir (monto). 

El servicio devuelve como respuesta un dato de tipo "String" con el valor de “true” si la transferencia se realizó de forma exitosa y el valor “false” si se generó algún error durante la transferencia.

Para el consumo del servicio se debe realizar a dirección [http://192.168.0.18:8080/coordinator/RestU/](http://192.168.0.18:8080/coordinator/RestU/)

```bash
    @POST
	@Consumes({MediaType.APPLICATION_JSON})
	@Produces({MediaType.TEXT_PLAIN})
	@Path ("/pasodesaldo")
	public String pasodesaldo(register r)
	{
		
		String val_r="false";
		
		val_r=clr.client_register(r.getDir_origen(), r.getDir_destino(),r.getMonto());
	    
		if (val_r.equals("true"))
		{
		 	String val_b="false";
			
			val_b=clb.client_transacion(r.getDir_origen(), r.getDir_destino(),r.getMonto());
			
			if(val_b.equals("true"))
			{
				r.setValidacion("true");
			}
			else
			{
				r.setValidacion("false");
			}
		}
		else
		{
			r.setValidacion("false");
		}
			
	   return r.getValidacion();
	}
```

#### Ejemplo de Consumo

```
Archivo tipo Json
{
 "dir_origen":"#direccionOrigen",
 "dir_destino":"#direccionDestino",
 "monto":"#cantidad"
}
```

#### Respuesta del Consumo
```
{
 "false" (Transacción Rechazada)
 "true" (Transacción exitosa)
}
