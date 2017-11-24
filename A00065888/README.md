### Examen 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos
**Docente:** Daniel Barragán C. 
**Estudiante:** Nicolás Recalde M.
**Tema:** Descubrimiento de servicios, Microservicios  
**Correo:** daniel.barragan@correo.icesi.edu.co & nikoremi97@hotmail.com

### Objetivos
* Implementar servicios web que puedan ser consumidos por usuarios o aplicaciones
* Conocer y emplear tecnologías de descubrimiento de servicio

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7
* Framework consul, zookeper o etcd

### Descripción
El tercer parcial del curso sistemas operativos trata sobre la creación de servicios web y el uso de tecnologías para el descubrimiento de servicio


**Figura 1.** Despliegue básico de microservicios

## Desarrollo  
Mediante la conexión de diferentes máquinas virtuales en los computadores de algunos compañeros, se desplegó el siguiente esquema de balanceador de carga y descubrimiento de servicios:  
![][1]  
Primero tenemos un **Balanceador de Carga** que hace peticiones a un servidor **Consul server** que usa un servicio de **Discovery Service**, el cual a su vez hace peticiones a tres **Consul Client** que prestan un microservicio similar. Para un posterior diferenciador de los servicios, modificamos un mensaje inicial, pero si se quisiera prestar el mismo microservicio mediante todos los servidores el microservicio debería ser idéntico.  
Lo que pretende hacer este sistema es lo siguiente: un cliente, como un *Web Browser*, realiza solicitudes a una página web. Dicha página, tiene un balanceador de carga que se conecta a un servidor de descubrimiento de servicios. Este servidor, busca qué otros servidores tienen disponible el servicio que el cliente está pidiendo y le asigna uno. Es por esto que si el cliente realiza la misma solicitud dos veces, el balanceador de carga hace que otro servidor sea el que le conteste la solicitud al cliente.  
El montaje del sistema se realizó de la siguiente forma:  
Cada servidor presta el siguiente microservicio:  
![][2]  
Cada servidor se subscribe como un *Consul agent client* a un *Consul agent server*.  
Aquí el servidor se vuelve un *Consul agent client*.
![][3]  
Ahora el servidor se subscribe a un *Consul agent Server*
![][4]  
##
Por el lado del Consul agent server se realiza lo siguiente:  
![][5]  
Cuando un agent client se le une, es notificado:  
![][6]  
Por último, revisamos qué otros clientes al servidor de consul se han unido:    
![][7]  
##
Todo listo por parte de los servidores, ahora necesitamos configurar nuestro balanceador de carga que se encargará de distribuir las peticiones entre los servidores.  

## Referencias


[1]: images/Microservices_Deployment.png
[2]: images/operationspython.JPG
[3]: images/initiclient.JPG
[4]: images/consuljoin.JPG
[5]: images/consul_agent_server.PNG
[6]: images/consul_logs.PNG	
[7]: images/consul_members.PNG



