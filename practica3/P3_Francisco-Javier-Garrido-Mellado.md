#Practica 3. Balanceo de carga
- FRANCISCO JAVIER GARRIDO MELLADO 
- HUGO BARZANO CRUZ

**Configuracion:** Para la configuración de los balanceadores nginx y haproxy se ha procedido a la instalación de cada uno de ellos en una maquina distinta.

**Datos a tener en cuenta:** 
- IP de la máquina principal 192.168.2.128
- IP de la máquina de respaldo 192.168.2.129 

##-Configuración de la máquina NGINX
En esta máquina se ha instalado un Ubuntu Server 12.04 con lo básico, unicamente se ha instalado los paquetes SSH por si en un futuro fuese necesario.Posteriormente se importa la clave del repositorio de la siguiente manera:

	cd /tmp/
	wget http://nginx.org/keys/nginx_signing.key
	apt-key add /tmp/nginx_signing.key
	rm -f /tmp/nginx_signing.key

A continuación he escrito directamente en **/apt/source.list** usando vi las siguientes lineas para que admita los repositorios de estas direcciones cuando se proceda a la instalación de nginx:

	deb http://nginx.org/packages/ubuntu/ lucid nginx
	deb-src http://nginx.org/packages/ubuntu/ lucid nginx

Y se procede a la instalación haciendo previamente un update:

	apt-get update
	apt-get install nginx

Hecho esto se configura el archivo **/etc/nginx/conf.d/default.conf**, las lineas de este archivo para mi máquina son las siguientes:

	upstream apaches {
		server 192.168.2.128 weight=2;
		server 192.168.2.129 weight=1;
		keep alive 3
	}

	server{
		listen 80;
		server_name ubuntuBalanceador;
		access_log /var/log/nginx/			
		ubuntuBalanceador.access.log;
		error_log /var/log/nginx/ubuntuBalanceador.error.log;
		root /var/www/;

	location /
	{
		proxy_pass http://apaches;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_http_version 1.1;
		proxy_set_header Connection "";
	}
	}

Despues de la configuración se ha procedido a reiniciar el servicio ejecutando **service nginx restart**

Se ha comprobado usando **curl http://192.168.2.130/** (direccion de la máquina balanceadora nginx)

##-Configuración de la máquina HAPROXY
Para la configuración de la máquina balanceadora haproxy se ha procedido creando al igual que antes una nueva máquina con un Ubuntu Server unicamente con los paquetes SSH, para la instalación de haproxy se ha ejecutado la orden:

		apt-get install haproxy

A continuación se ha procedido con vi a editar el archivo **/etc/haproxy/haproxy.cfg**

Las lineas de configuración que se han utilizado son las siguientes:

	global
		daemon
		maxconn 256
	defaults
		mode http
		contimeout 4000
		clitimeout 42000
		srvtimeout 43000



	frontend http-in
		bind *:80
		default_backend servers

	backend servers
		server ubuntu 192.168.2.128:80 maxconn 32
		server ubuntu2 172.168.2.129:80 maxconn 32

Por ultimo se ha añadido al archivo **/etc/rc.local** la siguiente linea:

		/usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg

Y se ha comprobado usando curl lanzadolo a **http://192.168.2.131/**









