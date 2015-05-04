#Practica 5. Replicación de bases de datos MySQL
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

##Realizar configuración de RAID1 con dos discos duros.

#Pasos( como root ):

- El primer paso ha sido añadir los dos discos duros, para ello con la máquina virtual apagada se han añadido los que seran los discos *sdb* y *sdc* en el sistema ( *ls /dev/* ).
- El segundo paso con la máquina virtual ya arrancada ha sido instalar el software de *RAID* para Linux, en este caso *mdadm*, para ello se ejecuta el comando *apt-get install mdadm* .
- El tercer paso ha sido comprobar que los dos discos duros estaban en el sistema, esto se comprueba mediante el comando *fdisk -l*.Puede aprovecharse para crear las particiones correspondientes en los dos discos duros añadidos con los comandos *fdisk /dev/sdb* y *fdisk /dev/sdc*.
- Posteriormente se procede al creado del dispositivo raid indicandole el nivel de RAID que se va a usar asi como los discos duros empleados, para ello se introduce el comando *mdadm -C /dev/md0 --level=raid1 --raid-devices=2 /dev/sdb /dev/sdc*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/mkfs.png)
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/mdadmdetail.png)
- El siguiente paso es darle formato al raid, para ello se introduce *mkfs.ext4 /dev/md0*
- Por último se crea la carpeta donde se va a montar, se ejecuta *mkdir /datos* y *mount /dev/md0 /datos*.
- En mi caso he añadido que esto último se haga automático al iniciar la máquina, he obtenido el *UUID* ejecutando *ls -l /dev/disk/by-uuid* y con el identificador obtenido he añadido la linea correspondiente al archivo */etc/fstab*. La linea no es mas que lo siguiente:
*UUID=c140a5c8-5094-40cb-b331-667902156ba8 /datos ext4 defaults 0 0*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/conf_fstab.png)

Al reiniciar y comprobar puede observarse que queda el RAID de manera impoluta ( salvo el nombre que motivo que desconozco pasa de md0 a md127 ).
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/practica6/reinicio_mdadmdetail.png)
