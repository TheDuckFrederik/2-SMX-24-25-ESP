## 1. Instalación del servidor DHCP en Linux. 
### Distros basadas en Debian (Ubuntu):
```
sudo apt install isc-dhcp-server
```
un método alternativo:
```
sudo apt-get install isc-dhcp-server
```
### Distros basadas en Arch:
```
sudo pacman -S isc-dhcp-server
```
### Distros basadas en RHEL:
```
sudo yum install dhcp
```
o en versiones más recientes:
```
sudo dnf install dhcp
```

## 2. Netplan. 
### Editando el archivo: 
##### Accedemos a la carpeta que contiene el netplan:
```
cd /etc/netplan
```
##### Antes de editar el archivo, hacemos una copia (el archivo puede tener diferentes nombres, usa `ls` para ver el archivo. En mi caso es "50-cloud-init.yaml"):
```
sudo cp 50-cloud-init.yaml 50-cloud-init-copy.yaml
```
##### Ahora editamos el archivo:
```
sudo nano 50-cloud-init.yaml
```
##### Para establecer la dirección de la interfaz: 
###### Estructura:
```
network:
	ethernets:
		enp0s3:
			dhcp4: true
	version: 2
```
###### Agregar una interfaz:
Para conocer el nombre de una interfaz, puedes usar el comando:
```
ip a
```
Este comando mostrará todas las interfaces como en el siguiente ejemplo:
```
2: enp0s3:
	inet "ip"
3: enp0s8:
	inet "ip"
```
El "enp0s" seguido de un número es el nombre de una interfaz. Debajo de ella encontraremos la IP y el mapa de dicha interfaz. Para agregar una interfaz al netplan (usa el comando de edición si saliste del editor), agregaremos la interfaz enp0s8 y le daremos una dirección IP:
###### Habilitar DHCP:
```
network:
	ethernets:
		enp0s3:
			dhcp4: true
		enp0s8:
			dhcp4: true
	version: 2
```
###### Asignar manualmente una dirección IP:
```
network:
	ethernets:
		enp0s3:
			dhcp4: true
		enp0s8:
			addresses:
				- 192.168.1.1/24
	version: 2
```
###### Aplicar netplan: Después de hacer cambios en el netplan, debes ejecutar el siguiente comando:
```
sudo netplan apply
```
## 3. Configuración por defecto de ISC-DHCP-SERVER

### Editando el archivo:
##### Accedemos a la carpeta que contiene los valores por defecto:
```
cd /etc/default
```
##### Antes de editar el archivo, hacemos una copia:
```
sudo cp isc-dhcp-server isc-dhcp-server-copy
```
##### Ahora editamos el archivo:
```
sudo nano isc-dhcp-server
```
#### Ahora necesitamos configurar la interfaz para el servidor DHCP.

###### Estructura:
```
INTERFACESv4=""
INTERFACESv6=""
```
###### Agregar una interfaz:
En este caso tenemos "enp0s8", así que la agregaremos a las interfaces v4:
```
INTERFACESv4="enp0s8"
INTERFACESv6=""
```
## 4. Editar la configuración del servidor DHCP:

### Editando el archivo:
##### Accedemos a la carpeta que contiene la configuración:
```
cd /etc/dhcp
```
##### Antes de editar el archivo, hacemos una copia:
```
sudo cp dhcpd.conf dhcpd-copy.conf
```
##### Ahora editamos el archivo:
```
sudo nano dhcpd.conf
```
#### Ahora necesitamos configurar la subred y otras configuraciones de red del servidor DHCP.

###### Estructura:
```
ddns-update-style none;
subnet "xx.xx.xx.xx" netmask "xx.xx.xx.xx" {
	range "xx.xx.xx.xx" "xx.xx.xx.xx";
	option domain-name-server "xx.xx.xx.xx";
	option domain-name "abc.def";
	option routers "xx.xx.xx.xx";
	option subnet-mask "xx.xx.xx.xx";
	default-lease-time "s";
	max-lease-time "s";
}
```
##### Ejemplo: Configuración de 1 subred:

###### Agregar una subred:

	-Subnet: 172.30.1.0/24
	-Rango: 172.30.1.100 - 172.30.1.254
	-DNS IP: 172.30.1.1, 172.30.1.2
	-Nombre de dominio: fristName-lastName.test
	-Router: 172.30.1.1
	-Tiempo de concesión por defecto: 3600
	-Tiempo de concesión máximo: 7200
###### Modificar el archivo:
```
ddns-update-style none;
subnet 172.30.1.0 netmask 255.255.255.0 {
	range 172.30.1.100 172.30.1.254;
	option domain-name-server 172.30.1.1, 172.30.1.2;
	option domain-name nombre-apellido.test;
	option routers 172.30.1.1;
	option subnet-mask 255.255.255.0;
	default-lease-time 3600;
	max-lease-time 7200;
}
```
###### Aplicar la configuración y comprobar que funciona:
Después de hacer cambios en el dhcpd.conf, debes ejecutar los siguientes comandos:
###### Reiniciar el servidor:
```
sudo systemctl restart isc-dhcp-server
```
###### Comprobar si la configuración fue exitosa:
```
sudo systemctl status isc-dhcp-server
```
Si al lado de "Active:" muestra "active (running)", todos los cambios se han aplicado correctamente. Si, en cambio, muestra "failed", algo en la configuración está mal.
###### Para obtener un diagnóstico más detallado, puedes ejecutar el siguiente comando:
```
sudo cat /var/log/syslog | grep dhcpd
```
Si esto arroja un error, ejecuta lo siguiente:
```
sudo cat /var/log/syslog | grep -a dhcpd
```
