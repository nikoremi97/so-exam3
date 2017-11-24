### Examen 3
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Docente:** Daniel Barragán C.  
**Estudiante:** Nicolás Recalde M.  
**Tema:** Descubrimiento de servicios, Microservicios  
**Correo:** daniel.barragan@correo.icesi.edu.co & nikoremi97@hotmail.com  
**URL:** https://github.com/nikoremi97/so-exam3.git

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
## Punto 3
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
Como balanceador de carga usamos HAProxy. Para su configuración, usamos la guía de https://www.upcloud.com/support/haproxy-load-balancer-centos/ .   
Configurar HAproxy:
``` 
$ sudo mkdir -p /etc/haproxy
$ sudo mkdir -p /var/lib/haproxy 
$ sudo touch /var/lib/haproxy/stats
$ sudo cp ~/haproxy-1.7.8/examples/haproxy.init /etc/init.d/haproxy
$ sudo chmod 755 /etc/init.d/haproxy
$ sudo systemctl daemon-reload
$ sudo chkconfig haproxy on
$ sudo useradd -r haproxy
```  
Abrir puertos
```
$ sudo firewall-cmd --permanent --zone=public --add-service=http
$ sudo firewall-cmd --permanent --zone=public --add-port=8181/tcp
$ sudo firewall-cmd --reload
```  
configure archivos y reload service
```
$ sudo vi /etc/haproxy/haproxy.cfg
$ sudo systemctl restart haproxy
```  
Debemos configurar además los siguientes aspectos, agregar el nombre del *consul agent client*, su dirección IP.  
![][8]  
Además en el Consul-template debemos poner el nombre del microservicio de cada servidor.  
![][9]  
Ahora verificamos que el balanceador esté funcionando.  
![][10]  
## Punto 4  
En el punto anterior se describió que se iba a montar el mismo servicio en los 3 servidores pero con un mensaje difernciador. Esto nos sirve para corroborar que el balanceador está cumpliendo bien su trabajo. El balanceador utiliza un método de balanceo llamado *round-robin*, mediante el cual se asigna un servidor diferente a cada solicitud realizada al balanceador hasta repetirse el primer servidor y así sucesivamente.  
![][11]  
Realizando 3 peticiones al balanceador vemos como este rota los servidores para la atención a la solicitud del cliente:  
![][12]
![][13]
![][14]
## Punto 5  
Si se quisiera agregar un microservicio al ambiente ya desplegador deberíamos entender dos conceptos importantes. El de paradigma reactivo, el de API Gateway y el de protocolo pubicador/subscriptor.  
El paradigma reactivo, establece una nueva forma de atender solicitudes y puede aplicarse tanto a la parte de servidor como a la de clientes. Se trata de responder sólo con lo solicitado de manera asíncrona y óptima.
![][15]  
El API Gateway "es el único punto de entrada para todos los clientes. Algunas solicitudes son simplemente proxy/enrutadas al servicio apropiado. Gestiona otras solicitudes desplegándose a múltiples servicios". Esto quiere decir que usando el paradigma reactivo, los clientes le harán todas las solicitudes al API Gateway y este se encargará de redirigir las solicitudes al balanceador de carga para cada microservicio (en caso de que todos tengan un balanceador de carga).  
El API Gateway se ilustra en la siguiente imagen:  
![][16]  
Así vemos que ahora todos los clientes le realizan las peticiones al API Gateway. Entonces, en nuestro esquema inicial, el punto de entrada no sería el balanceador de carga, sino el API Gateway. Éste redigirá las solicitudes a los balanceadores para que redistribuyan las solicitudes entre los servidores con el nuevo servicio. Y por último, está claro que se debe montar el servicio en los servidores y modificar algunas configuraciones de Consul, como el nombre del microservicio en sus templates, suscribirse al consul agent server y otros aspectos de configuración menores.

## Referencias
https://www.upcloud.com/support/haproxy-load-balancer-centos/  
https://github.com/ICESI/so-microservices-python  
https://github.com/ICESI/so-discovery-service/blob/master/README.md  
http://microservices.io/patterns/apigateway.html  
https://www.arquitecturajava.com/reactive-microservices-y-arquitectura/


[1]: images/Microservices_Deployment.png
[2]: images/operationspython.JPG
[3]: images/initiclient.JPG
[4]: images/consuljoin.JPG
[5]: images/consul_agent_server.PNG
[6]: images/consul_logs.PNG	
[7]: images/consul_members.PNG
[8]: images/configuracionBalanceador.png
[9]: images/configuracionConsulTemplates.png
[10]: images/BalanceadorCorriendo.png
[11]: images/DemostracionFuncionBalanceador.png
[12]: images/loadbalancer1.JPG
[13]: images/loadbalancer2.JPG
[14]: images/loadbalancer3.JPG
[15]: images/ReactiveMicroServicesRx.jpg
[16]: images/apigateway.jpg 


