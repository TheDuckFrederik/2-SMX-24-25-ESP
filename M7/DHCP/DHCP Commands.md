## 1. Installing DHCP server on Linux.
### Debian-based distros:
```
sudo apt install isc-dhcp-server
```
an alternative method:
```
sudo apt-get install isc-dhcp-server
```
### Arch-based distros:
```
sudo pacman -S isc-dhcp-server
```
### RHEL-based distros:
```
sudo yum install dhcp
```
or on newer:
```
sudo dnf install dhcp
```

## 2. Netplan.
### Editing the file:
##### We access the folder that contains the netplan:
```
cd /etc/netplan
```
##### Before editing the file, make a copy (the file can be named different things, make an ls to see the file. In my case its "50-cloud-init.yaml"):\
```
sudo cp 50-cloud-init.yaml 50-cloud-init-copy.yaml
```
##### Now we edit the file:
```
sudo nano 50-cloud-init.yaml
```
##### To set the interface address:
###### Structure:
```
network:
	ethernets:
		enp0s3:
			dhcp4: true
	version: 2
```
###### Adding an interface:
To know the name of an interface, you can use the command:
```
ip a
```
This command will show all of the interfaces like in the following example:
```
2: enp0s3:
	inet "ip"
3: enp0s8:
	inet "ip"
```
The "enp0s" followed by a number is the name of an interface, bellow it we can find the ip and map of said interface. To add a interface to the netplan, (use the edit command if you exited the editor), we will add the interface enp0s8 and give it an IP address:
###### Enabling DHCP:
```
network:
	ethernets:
		enp0s3:
			dhcp4: true
		enp0s8:
			dhcp4: true
	version: 2
```
###### Manually giving it an IP address:
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
