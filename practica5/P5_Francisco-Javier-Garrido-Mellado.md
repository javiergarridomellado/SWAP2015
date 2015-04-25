#Practica 4. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

##Realizar copias de seguridad de una BD completa usando mysqldump.

Los pasos que se han seguido para la máquina principal:

- *Creación de la correspondiente base de datos*.
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/create_database.png)
- *Bloqueo de la base de datos* usando el comando **FLUSH TABLES WITH READ LOCK** tras haber accedido a ella.

- *Volcado de la base de datos* usando el comando **mysqldump contactos -u root -p > /root/contactosdb.sql** ( en el terminal ).
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/copia_bd.png)
- *Desbloqueo de la base de datos* usando el comando **UNLOCK TABLES** tras haber accedido a ella.


Los pasos que se han seguido para la máquina secundaria:

- *Copiar la base de datos de la máquina principal* mediante el comando **scp root@192.168.2.128:/root/contactosdb.sql /root/**
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/copiabd_en_maqsecundaria.png)
- *Crear la base de datos en la máquina secundaria* mediante el correspondiente **CREATE DATABASE contactos;**

- *Volcado de la base de datos* mediante el comando **mysql -u root -p contactos < /root/contactosdb.sql**

- *Comprobación* mediante el acceso a la base de datos y su correspondiente *select * from datos;* ( visionado de una de las tablas ).




