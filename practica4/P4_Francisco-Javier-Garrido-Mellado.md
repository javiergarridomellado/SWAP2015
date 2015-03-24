#Practica 4. Balanceo de carga
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

##Pruebas HTTPERF

**Comandos:**
 - Para una máquina servidora se ha ejecutado **httperf --server 192.168.2.128 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682128**
 - Para la maquina balanceadora **nginx** se ha ejecutado **httperf --server 192.168.2.130 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682130**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **httperf --server 192.168.2.131 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682131**

Se han realizado 10 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Connection time avg(ms)|Concurrent connections|Connection time max(ms)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|   0.6      |       2.6     |    11.94         | 
|192.168.2.130| 1.16        |       17.2   |    120.82         | 
|192.168.2.131| 2.04        |    11.6      |    94.2       | 

-Desviación estandar

|**Máquina**|Connection time avg(ms)|Concurrent connections|Connection time max(ms)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|  0.0        |       1.74       |  11.13         | 
|192.168.2.130| 0.091          |       20.07      |    86.71        | 
|192.168.2.131| 0.174          |       8.99      |    73.18       | 

##Pruebas OPENLOAD

**Comandos:**
 - Para una máquina servidora se ha ejecutado **openload 192.168.2.128 10 >> openload_1921682128.txt**
 - Para la maquina balanceadora **nginx** se ha ejecutado **openload 192.168.2.130 10 >> openload_1921682128.txt**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **openload 192.168.2.131 10 >> openload_1921682131**

Se han realizado 10 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Total Request|Avg. Response time(s)|Max Response time(s)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|   100388.3      |       0.0035     |   0.0476         | 
|192.168.2.130| 74065.2       |       0.005   |   0.0779         | 
|192.168.2.131| 8355.3        |    0.043      |    0.0579      | 

-Desviación estandar

|**Máquina**|Total Request|Avg. Response time(s)|Max Response time(s)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|  11166.19       |       0.0006       |   0.033         | 
|192.168.2.130| 2507.9          |       0.0      |   0.123         | 
|192.168.2.131| 40.74          |       0.0      |   0.018        | 