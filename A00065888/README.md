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
Se desplegó el siguiente esquema:  
![][1]  
Primero tenemos un **Balanceador de Carga** que hace peticiones a un servidor **Consul server**, el cual a su vez hace peticiones a tres **Consul Client** que prestan un microservicio similar. Para un posterior diferenciador de los servicios, modificamos un mensaje inicial, pero si se quisiera prestar el mismo microservicio mediante todos los servidores el microservicio debería ser idéntico.  





[1]: images/Microservices_Deployment.png
