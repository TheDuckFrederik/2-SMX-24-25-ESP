### Protecció física (passiva):

- Control d'accés: Implementa dispositius de control d'accés com etiquetes RFID o reconeixement biomètric per assegurar que només el personal autoritzat pugui accedir a les instal·lacions.

- Càmeres de seguretat: Instal·la càmeres en punts clau per vigilar el lloc i garantir la seguretat dels equips.

- Equip protegit: Utilitza armaris per protegir l’equip, especialment els ordinadors i les ulleres de realitat virtual, amb la finalitat de prevenir robatoris o danys físics.
### Protecció cibernètica i de la informació:

- Autenticació multifactor (MFA): És important que tant els ordinadors locals com els servidors Linux requereixin autenticació MFA per accedir-hi.

- Encriptació de la informació: Desenvolupa un sistema d'encriptació per salvaguardar dades sensibles derivades de la creació de gràfics 3D per ordinador o equips de realitat virtual.

- Sistema de protecció contra intrusions i tallafocs (IDS/IPS): Col·loca tallafocs en els servidors i ordinadors amb Windows, i afegeix sistemes de detecció d'intrusos per supervisar i evitar atacs.

- Administració d'actualitzacions i correccions: Desenvolupa mètodes per mantenir el programari actualitzat i segur davant vulnerabilitats de seguretat.
### Gestió d'electricitat i potència:

- Dispositius SAI/UPS: Són necessaris per garantir la protecció d'ordinadors i servidors davant talls de corrent, evitant la pèrdua de dades i danys als equips.

- Regulació de la temperatura i flux d'aire: Donat l’alt rendiment dels ordinadors de renderització 3D, és fonamental tenir un sistema de refrigeració adequat per prevenir el sobreescalfament.

- Determinar la capacitat elèctrica necessària: Verifica que la instal·lació elèctrica estigui preparada per suportar la demanda d'energia d'aquests dispositius d'alt rendiment.
### Configuració de maquinari especificada:

- Configurar l'ordinador local (Windows) amb mesures de seguretat, incloent configurar el tallafocs de Windows, activar l'antivirus, habilitar el bloqueig automàtic de la sessió i aplicar una política de contrasenyes segures.

- Servidor Linux: Configura SSH per accedir de manera segura
### Potència total necessària:

- Ordinadors: 3500W

- Servidors: 1000W

- Control d'accés i càmeres: 70W

- Refrigeració: 1500W

- Potència total = 6070W
### Temps de suport del SAI:

Depenent del temps que vulguis que els equips es mantinguin en funcionament durant un tall de corrent, necessitaràs ajustar la capacitat del SAI. Un [SAI](https://www.eaton.com/us/en-us/skuPage.9PX10KSP.html) Eaton 9PX10KSP de 10,000 VA podria ser suficient per mantenir aquests equips en funcionament durant uns minuts, suficient per apagar-los de manera segura o restaurar el corrent.

### Maquines Virtuals:

Hem conectat dues maquines virtuals de Ubuntu Server 22.04, hem utilitzat el servei Samba per compartir una carpeta a una xarxa local amb credencials. Al servidor la carpeta esta a:
```
/srv/NetCarp
```
I nomes tenen permisos els usuaris "Aleix" i "Unai". Les credencials al client funcionen diferent ja que per poder conectar-te a la carpeta necesites accedir amb l'usuari "Aleix" o "Unai". No cal pero aquesta es la comenda per conectar-les:
```
sudo mount -t cifs //10.0.0.6/NetCarp /mnt/NetCarp -0 username=Aleix
```

```
sudo mount -t cifs //10.0.0.6/NetCarp /mnt/NetCarp -0 username=Unai
```
No cal tornar a executar aquesta comanda ja que quan l'executem la carpeta es monta fins que es desconecti. La majoria de problemes que hem tingut han sigut intentant conectar la carpeta al client. Pero un cop fet ha funcionat sense problema.
