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

### Desarrollo  
Mediante la conexión de diferentes máquinas virtuales en los computadores de algunos compañeros, se desplegó el siguiente esquema de balanceador de carga y descubrimiento de servicios:  
![][1]  
Primero tenemos un **Balanceador de Carga** que hace peticiones a un servidor **Consul server** que usa un servicio de **Discovery Service**, el cual a su vez hace peticiones a tres **Consul Client** que prestan un microservicio similar. Para un posterior diferenciador de los servicios, modificamos un mensaje inicial, pero si se quisiera prestar el mismo microservicio mediante todos los servidores el microservicio debería ser idéntico.  
Lo que pretende hacer este sistema es lo siguiente: un cliente, como un *Web Browser*, realiza solicitudes a una página web. Dicha página, tiene un balanceador de carga que se conecta a un servidor de descubrimiento de servicios. Este servidor, busca qué otros servidores tienen disponible el servicio que el cliente está pidiendo y le asigna uno. Es por esto que si el cliente realiza la misma solicitud dos veces, el balanceador de carga hace que otro servidor sea el que le conteste la solicitud al cliente.  
El montaje del sistema se realizó de la siguiente forma:  
Cada 





[1]: images/Microservices_Deployment.png
