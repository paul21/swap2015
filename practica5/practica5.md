Práctica 5: Replicación de bases de datos MySQL
-----------------------------------------------

Comenzamos creando una base de datos denominada `contactos` dentro de la cual crearemos una tabla llamada `datos` que contendrá dos campos `nombre varchar(100)` y `tlf int`. Luego insertamos un registro de ejemplo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura1.png)

Vemos la descripción de la tabla.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura2.png)

###Replicación de una BD MySQL utilizando mysqldump

Para este apartado creamos una base de datos de ejemplo como la de la sección anterior, pero agregamos algunos campos extra.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura3.png)

Añadimos un registro de ejemplo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura4.png)

Completamos los datos de ejemplo.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura5.png)

Antes de hacer la copia de seguridad nos aseguramos que los datos no estén actualizándose.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura6.png)

Ya estamos en condiciones de volcar nuestra BD a un archivo llamado `contactos.sql` mediante el comando `mysqldump`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura7.png)

Ahora podemos desbloquear las tablas.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura8.png)

Desde *server2* copiamos con `scp`el archivo con el volcado con la información que contiene *server1*.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura9.png)

Creamos la base de datos en MySQL.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura10.png)

Cargamos el archivo de volcado a la misma.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura11.png)

Comprobamos que los datos se hayan cargado correctamente.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura12.png)

###Replicación mediante una configuración maestro-esclavo

Partimos de la BD de ejemplo de la sección anterior. Luego de editar el archivo de configuración del maestro creamos un usuario `esclavo` en el mismo servidor y obtenemos los datos de la BD que vamos a replicar.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura13.png)

Ahora en el MySQL del esclavo proporcionamos los datos del maestro.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura14.png)

Ahora desde el mismo esclavo comprobamos el estado.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura15.png)

Para probar nuestra configuración introducimos un nuevo registro en *server1*, nuestro maestro.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura16.png)

Vamos al maestro y comprobamos que el nuevo registro se encuentra debidamente replicado.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica5/figura17.png)





