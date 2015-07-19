Práctica 3: Balanceo de carga
-----------------------------

Para realizar esta práctica comenzamos por instalar el servidor **nginx** en Ubuntu 12.04. Descargamos la firma digital y agregamos los repositorios a `/etc/apt/sources.list`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura1.png)

Luego de actualizar instalamos **nginx** mediante el conocido comando apt `apt-get install nginx`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura2.png)

Editamos la configuración de **nginx** para indicarle que actúe como balanceador añadiendo la línea `proxy_pass` donde indicaremos un grupo de servidores `upstream` que denominamos *apaches*. En la definición de upstream indicamos las direcciones IP de nuestros servidores.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura3.png)

Reiniciamos el servicio mediante `service nginx restart`. Ya estamos en condiciones de probar el balanceo. Para ello creamos un pequeño *script* en PHP llamado `test.php` que nos informará cual servidor atendió la petición.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura4.png)

Utilizamos el comando **curl** con la IP del balanceador para solicitarle el archivo que acabamos de crear.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura5.png)

Podemos observar que cada petición fue atendida por un servidor diferente.

Ahora procederemos a instalar el balanceador **haproxy** que ya se encuentra en los repositorios de la distribución, ejecuntando para ello `apt-get install haproxy`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura6.png)

En el archivo de configuración de **haproxy** definimos las direcciones IP de los `backend servers`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura7.png)

Ya estamos en condiciones de ejecutar el servicio mediante el comando `/usr/bin/haproxy -f /etc/haproxy/haproxy.cfg`. Nuevamente comprobamos con el comando **curl**.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura8.png)

###Balanceo por ponderación

Vamos a editar la configuración de **nginx** para asignarle diferentes pesos a los servidores, asumiendo que el primer servidor tiene el doble de capacidad que el segundo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura9.png)

Para poder comprobar el funcionamiento del algoritmo creamos un *script* de bash que realiza 100 peticiones al balanceador y de acuerdo a la respuesta contabiliza a los servidores que respondieron en cada caso.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura10.png)

Ejecutamos el guión.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica3/figura11.png)

La proporción de peticiones asignadas a cada servidor son las esperadas.
