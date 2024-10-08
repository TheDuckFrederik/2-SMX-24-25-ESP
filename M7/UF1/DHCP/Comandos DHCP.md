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
