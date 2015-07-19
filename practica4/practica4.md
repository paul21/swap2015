Práctica 4: Comprobar el rendimiento de servidores web
------------------------------------------------------

Vamos a comprobar el rendimiento de nuestros servidores con las herramientas **Apache Benchmark** y **Siege**. Creamos un archivo HTML simple para realizar las pruebas.

![imagen] (https://github.com/paul21/swap2015/blob/master/practica4/figura1.png)

###Apache Benchmark

Ejecutamos 1000 peticiones al balanceador con 10 conexiones concurrentes. Para ello utilizamos el comando `ab -n 1000 -c 10 http://192.168.1.1/test.html`-

![imagen] (https://github.com/paul21/swap2015/blob/master/practica4/figura2.png)

Entre las métricas más importantes encontramos:

    Nivel de concurrencia: 5
    Tiempo transcurrido en la prueba: 2.191 segundos
    Peticiones fallidas: 0
    Peticiones por segundo: 456.32

###Siege

Siege es una herramienta de generación de carga, por lo que la ejecutaremos por un tiempo determinado, en nuestro caso durante 60 segundos, sin pausas, lo que está indicado por el parámetro `-b`. El comando completo es `.siege -b -t60S -v http://192.168.1.1/test.html`

![imagen] (https://github.com/paul21/swap2015/blob/master/practica4/figura3.png)

Observamos las siguientes métricas de interés:

    Disponibilidad: 100.00%
    Tiempo transcurrido: 60.07 segundos
    Tiempo de respuesta: 0.03 segundos
    Tasa de transacciones: 417.60 trans/seg
    
Para un estudio más detallado de rendimiento de Nginx y HAProxy utilizando diferentes algoritmos de balanceo, consulte el trabajo final de la asignatura [aquí](https://github.com/paul21/swap2015/tree/master/presentacion/algoritmos_balanceo_nginx_haproxy.md).

