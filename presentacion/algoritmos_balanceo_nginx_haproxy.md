Análisis y comparación de algoritmos de balanceo con Nginx y HAProxy
--------------------------------------------------------------------

##1 - Introducción

El objetivo de este trabajo es configurar y analizar las diferencias en rendimiento de distintos tipos de algoritmos de balanceo tanto en Nginx y HAProxy para compararlos entre sí y respecto a un entorno sin balanceo de carga. El primero es un servidor web, reverse-proxy y balanceador y el segundo es un balanceador de diversos tipos de tráfico TCP que reparte peticiones entre diferentes servidores web.


##2 - Entorno de pruebas

Antes de ejecutar las pruebas preparamos un esquema de máquinas virtuales con diferentes características de hardware.

	Especificaciones máquina Host:
	
	Intel Core2 Duo CPU P8600  @ 2.40GHz
	4GB RAM DDR3
	Disco SSD Kingston SKC300S37A 120GB
	Kubuntu 14.04 LTS 64 bits
	VirtualBox 4.3.10

	Entorno virtualizado:
	
	Ubuntu Server 12.04 LTS 32 bits
	Balanceador: 512MB RAM
	Servidor 1 y 2: 512MB RAM
	Servidor 3: 380MB RAM
	Servidor 4: 256MB RAM
	Servidor Apache 2.2.22

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/esquema_mixto.png)

Para simular las diferencias de capacidades de procesamiento de los distintos servidores, limitamos el porcentaje de CPU en la configuración de VirtualBox:

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/vb_conf.png)


##3 - Algoritmos de balanceo de carga

###No-LB (sin balanceo)

Es el caso más simple, un sólo servidor web recibe todas las peticiones directamente sin intermediarios. La configuración de Apache es conocida y excede el alcance de este trabajo.

###Round-Robin

En esta configuración el balanceador mantiene una cola cíclica de servidores. Cuando entra una nueva conexión se le asigna al siguiente servidor en la lista, independientemente de cualquier otro factor, como cantidad de conexiones existentes o capacidad de procesamiento.

En Nginx:

	upstream apaches {
	    round-robin;
	    server 192.168.1.101;
	    server 192.168.1.102;
	    server 192.168.1.103;
	    server 192.168.1.104;
	}	

En HAProxy:

	backend servers {
	    balance roundrobin
	    server s1 192.168.1.101 maxconn 32
	    server s2 192.168.1.102 maxconn 32
	    server s3 192.168.1.103 maxconn 32
	    server s4 192.168.1.104 maxconn 32
	}


###Least Connections

Este algoritmo asigna la siguiente petición entrante al servidor con menor cantidad de conexiones en curso.

En Nginx:

	upstream apaches {
	    least_conn;
	    server 192.168.1.101;
	    server 192.168.1.102;
	    server 192.168.1.103;
	    server 192.168.1.104;
	}	

En HAProxy:

	backend servers {
	    balance leastconn
	    server s1 192.168.1.101 maxconn 32
	    server s2 192.168.1.102 maxconn 32
	    server s3 192.168.1.103 maxconn 32
	    server s4 192.168.1.104 maxconn 32
	}

###Weight

Es un algoritmo basado en la ponderación previa de los servidores de acuerdo a su capacidad de recibir peticiones. Así, el balanceador asignará las peticiones en la proporción indicada.

Para nuestras pruebas utilizaremos los siguientes pesos, siguiendo la lógica que los servidores 3 y 4 parecen poseer el 75% y el 50% respectivamente la capacidad de los dos primeros:

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/esquema_mixto_w.png)

En Nginx:

	upstream apaches {
	    server 192.168.1.101 weight=4;
	    server 192.168.1.102 weight=4;
	    server 192.168.1.103 weight=3;
	    server 192.168.1.104 weight=2;
	}	

En HAProxy:

	backend servers {
	    server s1 192.168.1.101 weight 4 maxconn 32
	    server s2 192.168.1.102 weight 4 maxconn 32
	    server s3 192.168.1.103 weight 3 maxconn 32
	    server s4 192.168.1.104 weight 2 maxconn 32
	}


###Mixto (LC+W)

Por último es posible combinar los algoritmos mencionados anteriormente. En este estudio utilizaremos los algoritmos de ponderación y menor cantidad de conexiones simultáneamente.

En Nginx:

	upstream apaches {
	    least_conn;
	    server 192.168.1.101 weight=4;
	    server 192.168.1.102 weight=4;
	    server 192.168.1.103 weight=3;
	    server 192.168.1.104 weight=2;
	}	

En HAProxy:

	backend servers {
	    balance leastconn
	    server s1 192.168.1.101 weight 4 maxconn 32
	    server s2 192.168.1.102 weight 4 maxconn 32
	    server s3 192.168.1.103 weight 3 maxconn 32
	    server s4 192.168.1.104 weight 2 maxconn 32
	}


##4 - Proceso de medición

###4.1 Herramientas

Apache Benchmark

Es una herramienta de línea de comandos que viene integrada con la instalación estándar de Apache. En este ejemplo ejecutamos 1000 peticiones GET hasta 10 de ellas en forma concurrente:

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/ab.png)

Siege

Es una herramienta de prueba de carga y benchmarking con soporte de autenticación, cookies y configuración de un número simulado de clientes:

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/siege.png)


##5 – Resultados

###Nginx

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/nginx_time.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/nginx_fail.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/nginx_rps.png)

###HAProxy

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/haproxy_time.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/haproxy_fail.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/haproxy_rps.png)


###Comparativa

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/com_time.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/com_fail.png)

![imagen] (https://github.com/paul21/swap2015/blob/master/presentacion/imagenes/com_rps.png)


##6 - Conclusiones

Antes de adentrarnos en la interpretación de los resultados es preciso aclarar que este estudio posee varias limitaciones y por lo tanto las conclusiones aquí expuestas sólo pueden entenderse como una serie de observaciones válidas para entornos de prueba similares y no deben ser tomadas como definitivas.

Algunas consideraciones a tener en cuenta:

 - Los recursos de hardware son compartidos.
A diferencia de un entorno con servidores físicamente independiente, las máquinas virtuales comparten los recursos de la máquina anfitriona, como procesador, memoria y entrada/salida, por lo que la capacidad total del sistema es en cierto grado la suma de las partes. Esto provoca que el 

 - La red cliente/servidor es interna. Tanto el cliente que realiza las peticiones, el balanceador y los servidores web están en la misma red de bucle. Esto significa que la latencia está dentro del orden de los 10^-2 ms. Una mayor velocidad de red se traduce a priori en mejoras en los tiempos de respuesta de ida y vuelta de las peticiones, pero permite al mismo tiempo inundar más fácilmente al balanceador con peticiones que si éstas tuvieran que recorrer un camino más largo y una serie de dispositivos intermedios para llegar al mismo.


 - Pruebas específicas vs. usuarios reales. En un servicio web en producción, los recursos son solicitados por cientos o miles de usuarios diferentes, desde distintos puntos geográficos y que no solicitan siempre el mismo recurso. 

Realizadas estas aclaraciones podemos concluir que ambos balanceadores rindieron de forma similar. Ambos son balanceadores comerciales fuertemente optimizados, desarrollados en lenguajes de bajo nivel y utilizados por grandes empresas con enormes cantidad de usuarios, por lo que es esperable que esto sea así. El algoritmo que mejor distribuyó la carga de acuerdo a las métricas fue el mixto, dejandolo como claro ganador.

También se puede observar que al contrario de lo que podría indicar la intuición, el rendimiento del algoritmo por ponderación estuvo por debajo que aquellos más simples. Esto puede deberse a que un porcenaje 

Por último, se puede observar que la utilización de un balanceador significó una diferencia de rendimiento considerable independientemente del algoritmo utilizado.
