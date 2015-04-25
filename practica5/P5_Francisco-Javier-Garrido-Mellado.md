#Practica 5. Replicación de bases de datos MySQL
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

##Restaurar dicha copia en la segunda máquina (clonado manual de la BD).

Los pasos que se han seguido para la máquina secundaria:

- *Copiar la base de datos de la máquina principal* mediante el comando **scp root@192.168.2.128:/root/contactosdb.sql /root/**
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/copiabd_en_maqsecundaria.png)
- *Crear la base de datos en la máquina secundaria* mediante el correspondiente **CREATE DATABASE contactos;**

- *Volcado de la base de datos* mediante el comando **mysql -u root -p contactos < /root/contactosdb.sql**

- *Comprobación* mediante el acceso a la base de datos y su correspondiente *select * from datos;* ( visionado de una de las tablas ).


##Realizar la configuración maestro-esclavo de los servidores MySQL para que la replicación de datos se realice automáticamente.

Los pasos que se han seguido son los siguientes:

En la máquina principal:

- Debido a que la versión de *MySQL* es mayor a la 5.5 en el archivo */etc/mysql/my.cnf* solo he hecho ligeras modificaciones.
- Se comenta la linea **bind-address 127.0.0.1**, la linea **log_error=/var/log/mysql/error.log** se deja igual, el identificador de servicio se deja con valor a 1 **server-id=1**
- Se reinicia el servicio mediante el comando **/etc/init.d/mysql restart**

En la máquina de replicación se realiza exactamente lo mismo con el fichero */etc/mysql/my.cnf* con la salvedad de que **server-id=2**.
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/replicacion_maestroesclavo1.png)

Seguidamente se accede a *MySQL* de la principal mediante el comando *mysql -uroot -p*.

Se crea el usuario esclavo mediante la siguiente sentencia:
**CREATE USER esclavo IDENTIFIED BY 'esclavo';**
**GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';**
**FLUSH PRIVILEGES;**
**FLUSH TABLES;**
**FLUSH TABLES WITH READ LOCK;**

y se obtiene los dates de la base de datos ejecutando **SHOW MASTER STATUS;**.

![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/replicacion_enmaestro.png)



A continuación se accede a *MySQL* de la máquina esclava y se ejecuta la siguiente sentencia:
**CHANGE MASTER TO MASTER_HOST='192.168.2.128',**
**MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',**
**MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=501,**
**MASTER_PORT=3306;**
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/replicacion_enesclavo.png)

y se arranca el servicio con la sentencia **START SLAVE;** , se comprueba y por último solo queda desbloquear la base de datos en la máquina principal haciendo **UNLOCK TABLES;** y cualquier cambio que se haga se verá reflejado tal y como ilustro en las imágenes.

Comprobación en la máquina esclava:

![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/Comprobacion_esclavo.png)

Actualización de base de datos maestra:

![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/cambio_en_maestro.png)

Replicación automática en la base de datos esclava:

![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica5/cambios_esclavo.png)




