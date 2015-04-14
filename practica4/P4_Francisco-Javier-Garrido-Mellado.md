#Practica 4. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

##Pruebas AB

**Comandos:**
 - Para una máquina servidora se ha ejecutado **ab -n 100000 -c 100 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**
 - Para la maquina balanceadora **nginx** se ha ejecutado **ab -n 100000 -c 100 http://192.168.2.130/ >>resultados_ab_contra1921682130.txt**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **ab -n 100000 -c 100 http://192.168.2.128/ >>resultados_ab_contra1921682128.txt**

Se han realizado 5 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Time taken for tests(s)|Requests per second|Time per request(ms)|Failed requests|
|:----------:|:---------------:|:-----------------:|:---------------:|:------------:|
|192.168.2.128|   33,1528      |       3025,638     |   0,3316         |  0  |
|192.168.2.130| 40,2122        |       2487,07   |    0,402         |   0  |
|192.168.2.131| 33,2198        |    3012,284      |    0,3322       |   0  |

-Desviación estandar

|**Máquina**|Time taken for tests(s)]|Requests per second|Time per request(ms)|Failed requests
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|  2,137423145       |       180,6480005       |    0,021243823       |  0  |
|192.168.2.130| 0,471661637         |       29,49641504     |    0,00484768       |  0  |
|192.168.2.131| 0,985280011         |       86,39511578      |   0,009705668       |   0  |

##Pruebas HTTPERF

**Comandos:**
 - Para una máquina servidora se ha ejecutado **httperf --server 192.168.2.128 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682128**
 - Para la maquina balanceadora **nginx** se ha ejecutado **httperf --server 192.168.2.130 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682130**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **httperf --server 192.168.2.131 --port 80 --uri /index.html --rate 150 --num-conn 27000 --num-call 1 --timeout 5 >> httperf_1921682131**

Se han realizado 5 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Total connections|replies|Request rate( req/s )|Errors total|
|:----------:|:---------------:|:-----------------:|:---------------:|:--------:|
|192.168.2.128|   27000      |       27000     |    150         | 0 |
|192.168.2.130| 27000      |      27000   |    150         | 0 |
|192.168.2.131| 27000        |    27000      |    150       | 0 |

-Desviación estandar

|**Máquina**|Total connections|replies|Request rate( req/s )|Errors total|
|:----------:|:---------------:|:-----------------:|:---------------:|:---------:|
|192.168.2.128|  0       |       0      |  0         |  0 |
|192.168.2.130| 0          |       0     |    0        |  0  |
|192.168.2.131| 0          |       0      |    0      |   0  |

##Pruebas OPENLOAD

**Comandos:**
 - Para una máquina servidora se ha ejecutado **openload 192.168.2.128 10 >> openload_1921682128.txt**
 - Para la maquina balanceadora **nginx** se ha ejecutado **openload 192.168.2.130 10 >> openload_1921682128.txt**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **openload 192.168.2.131 10 >> openload_1921682131**

Se han realizado 5 medidas para cada máquina siendo su media y su desviación las indicadas en la tabla:

- Media

|**Máquina**|Total TPS|Avg. Response time(s)|Max Response time(s)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|   8230,504      |       0,0034     |   0,0558
|192.168.2.130| 2043,68      |       0,005   |   0,12         | 
|192.168.2.131| 228,716        |    0,043      |    0,0578      | 

-Desviación estandar

|**Máquina**|Total Request|Avg. Response time(s)|Max Response time(s)|
|:----------:|:---------------:|:-----------------:|:---------------:|
|192.168.2.128|  12272,60969      |       0,000894427       |   0,04844791         | 
|192.168.2.130| 52,35157209         |       0      |   0,182658151         | 
|192.168.2.131| 0,708822968         |       0.0      |   0,001788854        | 


![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/ab.png)
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/httperf.png)
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/OpenLoad.png)

