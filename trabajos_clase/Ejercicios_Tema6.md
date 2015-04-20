#Ejercicio T6.1:
**Aplicar con iptables una política de denegar todo el tráfico en una de las máquinas de prácticas.Comprobar el funcionamiento.**
## FLUSH de reglas
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

Con esto se aplica que se deniegue todo lo entrante/saliente, solo es necesario aplicar este iptable en la máquina deseada.* 
## Establecemos politica por defecto: DROP!!!
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP


**Aplicar con iptables una política de permitir todo el tráfico en una de las máquinas de prácticas.Comprobar el funcionamiento.**
## FLUSH de reglas
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

## Establecemos politica por defecto
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -t nat -P PREROUTING ACCEPT
iptables -t nat -P POSTROUTING ACCEPT


*Con esto se aplica que se acepte todo lo entrante/saliente, solo es necesario aplicar este iptable en la máquina deseada.* 

#Ejercicio T6.2:
**Comprobar qué puertos tienen abiertos nuestras máquinas,su estado, y qué programa o demonio lo ocupa.**
Un ejemplo de lo que pide el enunciado es lo obtenido en mi máquina tras ejecutar *sudo netstat -tulpn*
![img](https://github.com/javiergarridomellado/SWAP2015/blob/master/trabajos_clase/netstat.png)
