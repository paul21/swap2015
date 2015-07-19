Práctica 6: Discos en RAID
--------------------------

Para esta práctica agregamos dos discos idénticos a una máquina vitual e instalamos  **mdadm** mediante `apt-get install mdadm`. Comprobamos el estados de los discos con el comando`fdisk -l`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura1.png)

Observarmos que tanto el disco `/dev/sdb` como `/dev/sdc` tienen el mismo tamaño y no cuentan todavía con una tabla de particiones.

Ya estamos en condiciones de crear un **RAID1** con dichos discos.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura2.png)

Creamos una partición en nuestro dispositivo virtual `/dev/md0`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura3.png)

Detallamos el estado de nuestro RAID.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura4.png)

Ahora necesitamos automatizar el montaje de este dispositivo cuando inicia el equipo. Insertamos la siguiente línea en el archivo `/etc/rc.local`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura5.png)

Reiniciamos el servidor y comprobamos los dispositivos montados con el comando `mount`.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica6/figura6.png)

Podemos ver que el arreglo se encuentra montado en `/datos`.

