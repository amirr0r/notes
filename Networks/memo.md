# Memo network

[Networking Fundamentals](https://www.youtube.com/playlist?list=PL6gx4Cwl9DGBpuvPW0aHa7mKdn_k9SPKO)

- **LAN** : Local Area Network (en français réseau local). Exemple : réseau de l'école est un LAN
- **WAN** : Wide Area Network, (en français réseau étendu), tout ce qu'il y a entre votre routeur et un réseau plus large
- **WLAN** : LAN qui utilise la transmission Wi-Fi. Exemple : hôtels, centre comercial, McDo...

- **MAC** address : identifiant unique et *immuable* du hardware. Exemple : 00:C4:B5:45:B2:43
- **IP** address : identifiant *variable* d'une machine sur un réseau. Exemple (IPv4) : 10.24.12.4

## TCP/IP

- **Protocols** : the rules for communication.

The Internet is based on the **TCP/IP** model.

**1. Application Layer** *ou couche Applicative*:

détermine comment les programmes de votre ordinateur interagissent avec les services de la couche de transport pour afficher les données envoyées ou reçues. Protocoles utilisés : HTTP, SMTP...

**2. Transport Layer** *ou couche Transport* 

vérifie la façon dont les données seront transmises (ports, intégrité des données..). Protocoles utilisés : TCP, UDP...
    
**3. Network Layer** *ou couche Réseau* : 

spécifie comment déplacer les paquets entre les hôtes et entre les réseaux. Protocoles utilisés : IP, ICMP...
    
**4. Link Layer** *ou couche de Liaison* : 

spécifie comment envoyer des données à travers un composant matériel physique.
    

## Hardware

- **Router** : permet aux ordinateurs d’un réseau de communiquer entre eux, mais aussi avec d’autres réseaux, comme votre réseau privé avec internet.

- **Switch** : permet aux ordinateurs d’un réseau de communiquer entre eux.


***

**packets** ou *paquets* sont utilisés pour transmettre des données à travers les réseaux. 
Un paquet se compose : 
- d'un **header** ou *en-tête* = informations sur l'endroit où le paquet va et d'où il vient.
- et d'un **payload** ou *charge utile*  = donnée réelle en cours de transfert.

Même si nous savons où nous envoyons nos données via des adresses IP, elles ne sont pas assez spécifiques pour envoyer nos données à certains processus ou services. Les services (tels que HTTP) utilisent un canal de communication via les [**ports**](https://fr.wikipedia.org/wiki/Port_(logiciel)).

***
## Subnetting

Pourquoi diable faisons-nous des sous-réseaux? Le sous-réseau est utilisé pour segmenter les réseaux et contrôler le flux de trafic au sein de ce réseau. Un hôte sur un sous-réseau ne peut donc pas interagir avec un autre hôte sur un sous-réseau différent.

### CIDR

Vous pouvez voir les sous-réseaux notés en notation **CIDR**, où un sous-réseau tel que **10.42.3.0/255.255.255.0** est écrit comme **10.42.3.0/24**.

Rappelez-vous qu'une adresse IP se compose de 4 octets ou 32 bits, CIDR indique la quantité de bits utilisés comme préfixe de réseau. Donc, 123.12.24.0**/23** signifie que les **23** premiers bits sont utilisés.
