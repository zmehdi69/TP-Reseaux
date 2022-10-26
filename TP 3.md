# TP3 : On va router des trucs


### 1. Echange ARP

🌞**Générer des requêtes ARP**
```
ping 10.3.1.12
```
```
PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=0.497 ms
64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.988 ms
64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.815 ms
64 bytes from 10.3.1.12: icmp_seq=4 ttl=64 time=0.829 ms
```
Table ARP de Jonh (10.3.1.11) :
```
10.3.1.12 dev enp0s8 lladdr 08:00:27:11:ec:34 STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:3f DELAY
```
Table ARP de Marcel (10.3.1.12) :
```
10.3.1.11 dev enp0s8 lladdr 08:00:27:ce:b5:ed STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:3f DELAY
```
Adresse MAC de Jonh (10.3.1.11) :
```
08:00:27:ce:b5:ed
```
Adresse MAC de Marcel (10.3.1.12) :
```
08:00:27:11:ec:34
```
 Commande pour voir la MAC de `marcel` dans la table ARP de `john`
 ```
 ip n s
```
  Commande pour afficher la MAC de `marcel`, depuis `marcel`
```
ip a
```
### 2. Analyse de trames

🌞**Analyse de trames**
```
sudo tcpdump -i enp0s8 -w capturetrame.pcap
```
```
sudo ip -s -s neigh flush all
[sudo] password for zmehdi:
10.3.1.12 dev enp0s8 lladdr 08:00:27:11:ec:34 used 781/780/761 probes 1 STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:04 ref 1 used 6/0/6 probes 0 REACHABLE
```
```
sudo ip -s -s neigh flush all
[sudo] password for zmehdi:
10.3.1.11 dev enp0s8 lladdr 08:00:27:ce:b5:ed used 762/762/724 probes 4 STALE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:04 ref 1 used 7/0/7 probes 0 REACHABLE
```
```
 ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=64 time=0.711 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=64 time=0.809 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=64 time=0.726 ms
64 bytes from 10.3.1.11: icmp_seq=4 ttl=64 time=0.807 ms
64 bytes from 10.3.1.11: icmp_seq=5 ttl=64 time=0.751 ms
^C
--- 10.3.1.11 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4100ms
```

🦈 **Capture réseau `tp3_arp.pcapng`** qui contient un ARP request et un ARP reply

[ma capture de trame arp](tp3_arp.pcap)

## II. Routage

Vous aurez besoin de 3 VMs pour cette partie. **Réutilisez les deux VMs précédentes.**

| Machine  | `10.3.1.0/24` | `10.3.2.0/24` |
|----------|---------------|---------------|
| `router` | `10.3.1.254`  | `10.3.2.254`  |
| `john`   | `10.3.1.11`   | no            |
| `marcel` | no            | `10.3.2.12`   |

> Je les appelés `marcel` et `john` PASKON EN A MAR des noms nuls en réseau 🌻

```schema
   john                router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └───┘    └─────┘    └───┘    └─────┘
```

### 1. Mise en place du routage

🌞**Activer le routage sur le noeud `router`**
```
 sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s8 enp0s9
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[zmehdi@localhost ~]$ sudo firewall-cmd --get-active-zone
public
  interfaces: enp0s8 enp0s9
[zmehdi@localhost ~]$ sudo firewall-cmd --add-masquerade --zone=public
Warning: ALREADY_ENABLED: masquerade already enabled in 'public'
success
[zmehdi@localhost ~]$ sudo firewall-cmd --add-masquerade --zone=public --permanent
Warning: ALREADY_ENABLED: masquerade
success
```
🌞**Ajouter les routes statiques nécessaires pour que `john` et `marcel` puissent se `ping`**
```
ip r s
10.3.1.0/24 dev enp0s8 proto kernel scope link src 10.3.1.11 metric 100
10.3.2.0/24 via 10.3.1.254 dev enp0s8 proto static metric 100
```
```
ip r s
10.3.1.0/24 via 10.3.2.254 dev enp0s8 proto static metric 100
10.3.2.0/24 dev enp0s8 proto kernel scope link src 10.3.2.12 metric 100
```
```
 ping 10.3.1.11
PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=63 time=0.616 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=63 time=1.74 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=63 time=1.64 ms
64 bytes from 10.3.1.11: icmp_seq=4 ttl=63 time=1.72 ms
```
```
 ping 10.3.2.12
PING 10.3.2.12 (10.3.2.12) 56(84) bytes of data.
64 bytes from 10.3.2.12: icmp_seq=1 ttl=63 time=0.765 ms
64 bytes from 10.3.2.12: icmp_seq=2 ttl=63 time=1.64 ms
64 bytes from 10.3.2.12: icmp_seq=3 ttl=63 time=1.26 ms
64 bytes from 10.3.2.12: icmp_seq=4 ttl=63 time=1.02 ms
```
### 2. Analyse de trames

🌞**Analyse des échanges ARP**

- videz les tables ARP des trois noeuds
- effectuez un `ping` de `john` vers `marcel`
  - **le `tcpdump` doit être lancé sur la machine `john`**
- essayez de déduire un les échanges ARP qui ont eu lieu
  - en regardant la capture et/ou les tables ARP de tout le monde
- répétez l'opération précédente (vider les tables, puis `ping`), en lançant `tcpdump` sur `marcel`
- **écrivez, dans l'ordre, les échanges ARP qui ont eu lieu, puis le ping et le pong, je veux TOUTES les trames** utiles pour l'échange

Par exemple (copiez-collez ce tableau ce sera le plus simple) :

| ordre | type trame  | IP source | MAC source              | IP destination | MAC destination            |
|-------|-------------|-----------|-------------------------|----------------|----------------------------|
| 1     | Requête ARP | x         | `marcel` `AA:BB:CC:DD:EE` | x              | Broadcast `FF:FF:FF:FF:FF` |
| 2     | Réponse ARP | x         | ?                       | x              | `marcel` `AA:BB:CC:DD:EE`    |
| ...   | ...         | ...       | ...                     |                |                            |
| ?     | Ping        | ?         | ?                       | ?              | ?                          |
| ?     | Pong        | ?         | ?                       | ?              | ?                          |

> Vous pourriez, par curiosité, lancer la capture sur `marcel` aussi, pour voir l'échange qu'il a effectué de son côté.

🦈 **Capture réseau `tp3_routage_marcel.pcapng`**

### 3. Accès internet

🌞**Donnez un accès internet à vos machines**

- ajoutez une carte NAT en 3ème inteface sur le `router` pour qu'il ait un accès internet
- ajoutez une route par défaut à `john` et `marcel`
  - vérifiez que vous avez accès internet avec un `ping`
  - le `ping` doit être vers une IP, PAS un nom de domaine
- donnez leur aussi l'adresse d'un serveur DNS qu'ils peuvent utiliser
  - vérifiez que vous avez une résolution de noms qui fonctionne avec `dig`
  - puis avec un `ping` vers un nom de domaine

🌞**Analyse de trames**

- effectuez un `ping 8.8.8.8` depuis `john`
- capturez le ping depuis `john` avec `tcpdump`
- analysez un ping aller et le retour qui correspond et mettez dans un tableau :

| ordre | type trame | IP source          | MAC source              | IP destination | MAC destination |     |
|-------|------------|--------------------|-------------------------|----------------|-----------------|-----|
| 1     | ping       | `marcel` `10.3.1.12` | `marcel` `AA:BB:CC:DD:EE` | `8.8.8.8`      | ?               |     |
| 2     | pong       | ...                | ...                     | ...            | ...             | ... |

🦈 **Capture réseau `tp3_routage_internet.pcapng`**

## III. DHCP

On reprend la config précédente, et on ajoutera à la fin de cette partie une 4ème machine pour effectuer des tests.

| Machine  | `10.3.1.0/24`      | `10.3.2.0/24` |
|----------|--------------------|---------------|
| `router` | `10.3.1.254`       | `10.3.2.254`  |
| `john`   | `10.3.1.11`        | no            |
| `bob`    | PAS POUR LE MOMENT | no            |
| `marcel` | no                 | `10.3.2.12`   |

```schema
   john               router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └─┬─┘    └─────┘    └───┘    └─────┘
   dhcp        │
  ┌─────┐      │
  │     │      │
  │     ├──────┘
  └─────┘
```

### 1. Mise en place du serveur DHCP

🌞**Sur la machine `john`, vous installerez et configurerez un serveur DHCP** (go Google "rocky linux dhcp server").

- installation du serveur sur `john`
- créer une machine `bob`
- faites lui récupérer une IP en DHCP à l'aide de votre serveur
  - utilisez le mémo toujours, section "Définir une IP dynamique (DHCP)"


🌞**Améliorer la configuration du DHCP**

- ajoutez de la configuration à votre DHCP pour qu'il donne aux clients, en plus de leur IP :
  - une route par défaut
  - un serveur DNS à utiliser
- récupérez de nouveau une IP en DHCP sur `bob` pour tester :
  - `bob` doit avoir une IP
    - vérifier avec une commande qu'il a récupéré son IP
    - vérifier qu'il peut `ping` sa passerelle
  - il doit avoir une route par défaut
    - vérifier la présence de la route avec une commande
    - vérifier que la route fonctionne avec un `ping` vers une IP
  - il doit connaître l'adresse d'un serveur DNS pour avoir de la résolution de noms
    - vérifier avec la commande `dig` que ça fonctionne
    - vérifier un `ping` vers un nom de domaine

### 2. Analyse de trames

🌞**Analyse de trames**

- lancer une capture à l'aide de `tcpdump` afin de capturer un échange DHCP
- demander une nouvelle IP afin de générer un échange DHCP
- exportez le fichier `.pcapng`
- repérez, dans les trames DHCP observées dans Wireshark, les infos que votre serveur a fourni au client
  - l'IP fournie au client
  - l'adresse IP de la passerelle
  - l'adresse du serveur DNS que vous proposez au client

🦈 **Capture réseau `tp3_dhcp.pcapng`**
