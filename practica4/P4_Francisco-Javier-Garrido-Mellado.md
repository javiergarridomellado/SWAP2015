#Practica 4. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

Los resultados de estas pruebas corresponden a la ejecución de *AB*, *HTTPERF* y *OpenWebLoad* contra tres máquinas, una será la que aloje la página web, otra un balanceador *Nginx* y por último un balanceador *haproxy*. La página utilizada es una estática muy básica, por tanto, los resultados obtenidos solo son de utilidad en este ambito. Por otra parte mi compañero ha realizado las pruebas con el *script* proporcionado por el profesor, estos pueden consultarse en la siguiente url *https://github.com/hugobarzano/swap2015.git*

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

|**Máquina**|Time taken for tests(s)|Requests per second|Time per request(ms)|Failed requests|
|:----------:|:---------------:|:-----------------:|:---------------:|:------------:|
|192.168.2.128|  2,137423145  | 180,6480005 | 0,021243823 |  0  |
|192.168.2.130| 0,471661637  |  29,49641504     | 0,00484768  |  0  |
|192.168.2.131| 0,985280011  |  86,39511578  |   0,009705668   |   0  |

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

##Conclusiones:
**AB:**
-En estas mediciones donde se ha usado una página estática básica como web se obtiene unos resultados *Time taken for tests* , *Requests per second* y *Time per requests* muy parecidos para una sola máquina y el balanceador *haproxy*, siendo peores para el balanceador *nginx*.Sin embargo, mi compañero **Hugo Barzano** tras haber aplicado el *script proporcionado por el profesor* si obtiene unos resultados mejores en los dos balanceadores que en la máquina sola. Otro dato curioso es que sin usar *scripts* no se registra ningun *Failed Requests* , no siendo asi en el caso de mi compañero de grupo.

**HTTPERF:**
-En este caso los resultados no son nada convincentes, tras haber consultado al profesor he decidido dejarlos por su indicación ya que hay muchos compañeros con el mismo resultado, este no es otro que obtener el mismo numero de *Total connections*, *replies*, *Request rate( req/s )* y *Errors total* en las tres casos estudiados. El resultado de mi compañero con el script es practicamente el mismo tambien, siendo ligeramente mejor con los balanceadores.

**OpenLoad:**
En este caso el resultado es favorable para una máquina sola, registrandose un mayor número de *Transactions Per Second* en esta, pero tambien obteniendose una mayor desviación. Nuevamente, al aplicar el script el resultado es mejor en los balanceadores que en la máquina sola.

##Añadido:
**Siege**

**Comandos:**
 - Para una máquina servidora se ha ejecutado **siege -b -t60s http://192.168.2.128/**
 - Para la maquina balanceadora **nginx** se ha ejecutado **siege -b -t60s http://192.168.2.130/**
 - Para la maquina balanceadora **haproxy** a se ha ejecutado **siege -b -t60s http://192.168.2.131/**

Se ha obtenido para una máquina el siguiente resultado:
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/siege1921682128.png)

Para la máquina balanceadora *nginx* :
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/siege_nginx.png)

Para la máquina balanceadora *haproxy* :
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/siege_haproxy.png)

La gráfica obtenida es la siguiente:

![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica4/resultados_siege.png)

Puede verse como ahora se obtiene un resultado ligeramente mejor en el servidor haproxy, y en los otros dos casos parecidos.

##Conclusion Final:
Tras haber ejecutado ab y siege tanto en máquinas que devuelven una página estática como una dinámica ( en el trabajo realizado y en la parte práctica de mi compañero Hugo Barzano ) he llegado a la conclusión de lo acertado que es el uso de un balanceador cuando se devuelve una página pesada o de tipo dinámico, en este caso los resultados que ofrece el balanceador frente a una máquina sola si son muy a tener en cuenta, cosa que no ocurra cuando se sirve una página estatica de un tamaño muy reducido como puede observarse en las mediciones.
