#Practica 3. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

##Pruebas AB

**Comandos:**
-Para una máquina servidora se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**
-Para la maquina balanceadora **nginx** se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.130/ >>resultados_ab_contra1921682130.txt**
-Para la maquina balanceadora **haproxy** a se ha ejecutado **ab -n 1000 -c 10 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**

Se han realizado 10 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

|**Máquina**|Total transferred|Requests per second|Time per request|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128| 0              |       0          |    0            | 
