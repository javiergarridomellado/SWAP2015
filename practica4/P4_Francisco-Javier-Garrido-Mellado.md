#Practica 3. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

##Pruebas AB

**Comandos:**
 - Para una máquina servidora se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**
 - Para la maquina balanceadora **nginx** se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.130/ >>resultados_ab_contra1921682130.txt**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**

Se han realizado 10 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Transfer rate[Kbytes/sec]|Requests per second|Time per request(ms)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|   1245.49      |       3304.12     |    3.08         | 
|192.168.2.130| 753.548        |       2057.691   |    4.863         | 
|192.168.2.131| 720.044        |    1907.694      |    5.2588       | 

-Desviación estandar

|**Máquina**|Transfer rate[Kbytes/sec]|Requests per second|Time per request(ms)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|  152.10        |       403.5       |    0.501        | 
|192.168.2.130| 43.59          |       119.55      |    0.283        | 
|192.168.2.131| 40.74          |       107.95      |    0.3011       | 
