Práctica 2: Clonar la información de un sitio web
-------------------------------------------------

Comenzamos con dos servidores de idéntica configuración.

Listamos los archivos del primer servidor.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura1.png)

Ahora los del segundo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura2.png)

Como podemos observar en *server1* se ha creado un archivo nuevo pablo.html, ahora la información en *server2* está desactualizada. Para sincronizarla utilizamos la herramienta rsync.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura3.png)

Comprobamos que los archivos en *server2* están efectivamente actualizados.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura4.png)

**rsync** solicita la contraseña de root cada vez que ejecutemos esta acción, por ello para poder automatizar tareas necesitamos crear un par de claves pública y privada de ssh para poder identificarnos sin necesidad de intervención por parte del operador.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura5.png)

Copiamos ahora la clave pública al otro servidor mediante el comando `ssh-copy-id -i .ssh/id_dsa.pub root@server1` y comprobamos que podemos ingresar mediante ssh sin ingresar contraseña.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura6.png)

Podemos agregar un comando al final de la línea para ejectuar en el otro equipo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura7.png)

Por último corroboramos que la clave pública de *server2* se encuentra en el directorio `.ssh/authorized_keys` de *server1*.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura8.png)

Ya estamos en condiciones de automatizar la tarea de mantener sincronizados ambos servidores añadiendo la siguiente línea al archivo `/etc/crontab'.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura9.png)

Creamos un nuevo archivo llamado `home.html` en *server1*.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura10.png)

Y esperamos a que **cron** realice su trabajo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica2/figura11.png)
