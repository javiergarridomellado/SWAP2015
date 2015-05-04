#Practica 6. Discos en RAID
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

#Realizar configuración de RAID1 con dos discos duros.

##Pasos( como root ):

- El primer paso ha sido añadir los dos discos duros, para ello con la máquina virtual apagada se han añadido los que seran los discos *sdb* y *sdc* en el sistema ( *ls /dev/* ).
- El segundo paso con la máquina virtual ya arrancada ha sido instalar el software de *RAID* para Linux, en este caso *mdadm*, para ello se ejecuta el comando *apt-get install mdadm* .
- El tercer paso ha sido comprobar que los dos discos duros estaban en el sistema, esto se comprueba mediante el comando *fdisk -l*.Puede aprovecharse para crear las particiones correspondientes en los dos discos duros añadidos con los comandos *fdisk /dev/sdb* y *fdisk /dev/sdc*.
- Posteriormente se procede al creado del dispositivo raid indicandole el nivel de RAID que se va a usar asi como los discos duros empleados, para ello se introduce el comando *mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc*
- El siguiente paso es darle formato al raid, para ello se introduce *mkfs.ext4 /dev/md0*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/mkfs.png)
- Por último se crea la carpeta donde se va a montar, se ejecuta *mkdir /datos* y *mount /dev/md0 /datos*.
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/mdadmdetail.png)
- En mi caso he añadido que esto último se haga automático al iniciar la máquina, he obtenido el *UUID* ejecutando *ls -l /dev/disk/by-uuid* y con el identificador obtenido he añadido la linea correspondiente al archivo */etc/fstab*. La linea no es mas que lo siguiente:
*UUID=c140a5c8-5094-40cb-b331-667902156ba8 /datos ext4 defaults 0 0*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/conf_fstab.png)

Al reiniciar puede comprobarse que el RAID queda de manera impoluta ( salvo el nombre que motivo que desconozco pasa de md0 a md127 ).
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/reinicio_mdadmdetail.png)

##Simulacion de rotura de un disco duro del RAID

- Para comprobar el funcionamiento del RAID simulando uno de los discos he realizado lo siguiente:

- He desconectado un disco duro de mi servidor Ubuntu. Puede observarse en las imágenes como se monitoriza la desconexión y su posterior conexión.Los pasos seguidos son:
- Se ejecuta el monitor con el comando *watch -n2 cat /proc/mdstat*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/watch.png)
- Con el comando *mdadm --detail /dev/md127* se ve el RAID montado con los correspondientes discos duros activos e inactivos.
- Simulo fallo de un disco duro con el comando *mdadm --manage --set-faulty /dev/md127 /dev/sdb*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/simulacionfallosdb.png)
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/watch_seobservafallodiscoduro.png)
- Elimino el disco duro que he supuesto que falla( si realmente asi fuese tambien habria que realizar el paso anterior) ejecutando *mdadm /dev/md127 -r /dev/sdb*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/eliminardisco.png)
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/solo1discoactivo.png)
- Añado el disco duro que en el paso anterior eliminé ejecutando *mdadm /dev/md127 -a /dev/sdb*
*Nota:con el comando watch se monitoriza la simulación del fallo y su posterior conexión, al tener un solo terminal no puede observarse.*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/anadirdiscodurofallido.png)

