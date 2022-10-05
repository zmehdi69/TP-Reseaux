

# TP1 - Premier pas réseau

**🌞 Affichez les infos des cartes réseau de votre PC**

```
ipconfig /all
```

```
Carte réseau sans fil Wi-Fi, F4-7B-09-A6-81-0A, 10.33.19.192
```
```
Carte Ethernet Ethernet, 08-8F-C3-36-61-47, pas d'adresse IP
```

**🌞 Affichez votre gateway**
```
ipconfig /all
```
```
Passerelle par défaut. . . . . . . . . : 10.33.19.254
```

**🌞 Déterminer la MAC de la passerelle**
```
arp -a
```
```
00-c0-e7-e0-04-4e
```

### En graphique (GUI : Graphical User Interface)

**🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)**

```
Panneau de configuration -> Réseau et internet -> Centre réseau et partage -> Wi-fi(WiFi@YNOV) -> Détails -> Détails de connexion réseau
```


## 2. Modifications des informations

### A. Modification d'adresse IP (part 1)  

🌞 Utilisez l'interface graphique de votre OS pour **changer d'adresse IP** :
```
Panneau de configuration -> Réseau et internet -> Centre réseau et partage -> Wi-fi(WiFi@YNOV) -> Propriétés -> Protocole Internet version 4 (TCP/IPv4)
```
🌞 **Il est possible que vous perdiez l'accès internet.** 
```
si l'adresse ip choisie existe déjà, on perd l'accès à internet
```

# II. Exploration locale en duo

## 3. Modification d'adresse IP

🌞 **Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau**
```
Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.225(en double)
Masque de sous-réseau. . . . . . . . . : 255.255.255.0
```


🌞 **Vérifier à l'aide d'une commande que votre IP a bien été changée**
```
ipconfig /all
```

🌞 **Vérifier que les deux machines se joignent**
```
ping 10.10.10.213

Envoi d’une requête 'Ping'  10.10.10.213 avec 32 octets de données :
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=3 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
```


🌞 **Déterminer l'adresse MAC de votre correspondant**
```
 arp -a

Interface : 10.33.19.192 --- 0xb
  Adresse Internet      Adresse physique      Type
  10.33.17.98           24-ee-9a-5a-77-60     dynamique
```


## 4. Utilisation d'un des deux comme gateway

🌞**Tester l'accès internet**
```
ping 1.1.1.1

Envoi d’une requête 'Ping'  1.1.1.1 avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=24 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=24 ms TTL=55
Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=55
```
🌞 **Prouver que la connexion Internet passe bien par l'autre PC**
```
 tracert 1.1.1.1

Détermination de l’itinéraire vers one.one.one.one [1.1.1.1]
avec un maximum de 30 sauts :

  1     2 ms     2 ms     4 ms  10.33.19.254
  2     3 ms     3 ms     2 ms  137.149.196.77.rev.sfr.net [77.196.149.137]
  3    11 ms     7 ms     7 ms  108.97.30.212.rev.sfr.net [212.30.97.108]
  4    21 ms    21 ms    18 ms  222.172.136.77.rev.sfr.net [77.136.172.222]
  5    19 ms    20 ms    20 ms  221.172.136.77.rev.sfr.net [77.136.172.221]
  6    22 ms    21 ms    23 ms  221.10.136.77.rev.sfr.net [77.136.10.221]
  7    22 ms    22 ms    20 ms  221.10.136.77.rev.sfr.net [77.136.10.221]
  8    19 ms    22 ms    26 ms  141.101.67.254
  9    23 ms    24 ms    22 ms  172.71.132.2
 10    20 ms    20 ms    23 ms  one.one.one.one [1.1.1.1]
``` 

## 5. Petit chat privé

Here we go :

🌞 **sur le PC *serveur*** 
```
ping 10.10.10.210

        Envoi d’une requête 'Ping'  10.10.10.210 avec 32 octets de données :
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128

        Statistiques Ping pour 10.10.10.210:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
        Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
        PS C:\Users\pc\Downloads\netcat-1.11> .\nc.exe -l -p 8888
        mec
        ftg
        tg
        bouffon
        fdp
        chu mort ca marche
        c bi1
```
🌞 **sur le PC *client*** 
```
ping 10.10.10.213

Envoi d’une requête 'Ping'  10.10.10.213 avec 32 octets de données :
Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128

Statistiques Ping pour 10.10.10.213:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
PS C:\Users\cedri\Desktop\netcat-1.11> ^C
PS C:\Users\cedri\Desktop\netcat-1.11> .\nc.exe 10.10.10.213 8888
ftg
mec
tg
bouffon
fdp
chu mort ca marche
c bi1
```
---

🌞 **Visualiser la connexion en cours**
```
netstat -a -n -b
 TCP    10.10.10.225:8888      10.10.10.210:54361     ESTABLISHED
 [nc.exe]
```
🌞 **Pour aller un peu plus loin**

- si vous faites un `netstat` sur le serveur AVANT que le client `netcat` se connecte, vous devriez observer que votre serveur `netcat` écoute sur toutes vos interfaces
```
.\nc.exe -l -p 8888
```
```
 netstat -a -n -b | select-string 8888

  TCP    0.0.0.0:8888           0.0.0.0:0              LISTENING
```
- il est possible d'indiquer à `netcat` une interface précise sur laquelle écouter
  - par exemple, on écoute sur l'interface Ethernet, mais pas sur la WiFI
```
.\nc.exe -l -p 8888 -s 10.10.10.225
```
```
netstat -a -n -b | select-string 8888

  TCP    10.10.10.225:8888      0.0.0.0:0              LISTENING
  ```


## 6. Firewall

Toujours par 2.

Le but est de configurer votre firewall plutôt que de le désactiver

🌞 **Activez et configurez votre firewall**

- autoriser les `ping`
  - configurer le firewall de votre OS pour accepter le `ping`
  - aidez vous d'internet
  - on rentrera dans l'explication dans un prochain cours mais sachez que `ping` envoie un message *ICMP de type 8* (demande d'ECHO) et reçoit un message *ICMP de type 0* (réponse d'écho) en retour
- autoriser le traffic sur le port qu'utilise `nc`
  - on parle bien d'ouverture de **port** TCP et/ou UDP
  - on ne parle **PAS** d'autoriser le programme `nc`
  - choisissez arbitrairement un port entre 1024 et 20000
  - vous utiliserez ce port pour communiquer avec `netcat` par groupe de 2 toujours
  - le firewall du *PC serveur* devra avoir un firewall activé et un `netcat` qui fonctionne
  
# III. Manipulations d'autres outils/protocoles côté client

## 1. DHCP

Bon ok vous savez définir des IPs à la main. Mais pour être dans le réseau YNOV, vous l'avez jamais fait.  

C'est le **serveur DHCP** d'YNOV qui vous a donné une IP.

Une fois que le serveur DHCP vous a donné une IP, vous enregistrer un fichier appelé *bail DHCP* qui contient, entre autres :

- l'IP qu'on vous a donné
- le réseau dans lequel cette IP est valable

🌞**Exploration du DHCP, depuis votre PC**

- afficher l'adresse IP du serveur DHCP du réseau WiFi YNOV
- cette adresse a une durée de vie limitée. C'est le principe du ***bail DHCP*** (ou *DHCP lease*). Trouver la date d'expiration de votre bail DHCP
- vous pouvez vous renseigner un peu sur le fonctionnement de DHCP dans les grandes lignes. On aura un cours là dessus :)

> Chez vous, c'est votre box qui fait serveur DHCP et qui vous donne une IP quand vous le demandez.

## 2. DNS

Le protocole DNS permet la résolution de noms de domaine vers des adresses IP. Ce protocole permet d'aller sur `google.com` plutôt que de devoir connaître et utiliser l'adresse IP du serveur de Google.  

Un **serveur DNS** est un serveur à qui l'on peut poser des questions (= effectuer des requêtes) sur un nom de domaine comme `google.com`, afin d'obtenir les adresses IP liées au nom de domaine.  

Si votre navigateur fonctionne "normalement" (il vous permet d'aller sur `google.com` par exemple) alors votre ordinateur connaît forcément l'adresse d'un serveur DNS. Et quand vous naviguez sur internet, il effectue toutes les requêtes DNS à votre place, de façon automatique.

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**

🌞 Utiliser, en ligne de commande l'outil `nslookup` (Windows, MacOS) ou `dig` (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

- faites un *lookup* (*lookup* = "dis moi à quelle IP se trouve tel nom de domaine")
  - pour `google.com`
  - pour `ynov.com`
  - interpréter les résultats de ces commandes
- déterminer l'adresse IP du serveur à qui vous venez d'effectuer ces requêtes
- faites un *reverse lookup* (= "dis moi si tu connais un nom de domaine pour telle IP")
  - pour l'adresse `78.73.21.21`
  - pour l'adresse `22.146.54.58`
  - interpréter les résultats
  - *si vous vous demandez, j'ai pris des adresses random :)*

# IV. Wireshark

**Wireshark est un outil qui permet de visualiser toutes les trames qui sortent et entrent d'une carte réseau.**

On appelle ça un **sniffer**, ou **analyseur de trames.**

![Wireshark](./pics/wireshark.jpg)

Il peut :

- enregistrer le trafic réseau, pour l'analyser plus tard
- afficher le trafic réseau en temps réel

**On peut TOUT voir.**

Un peu austère aux premiers abords, une manipulation très basique permet d'avoir une très bonne compréhension de ce qu'il se passe réellement.

➜ **[Téléchargez l'outil Wireshark](https://www.wireshark.org/).**

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

- un `ping` entre vous et votre passerelle
- un `netcat` entre vous et votre mate, branché en RJ45
- une requête DNS. Identifiez dans la capture le serveur DNS à qui vous posez la question.
- prenez moi des screens des trames en question
- on va prendre l'habitude d'utiliser Wireshark souvent dans les cours, pour visualiser ce qu'il se passe

# Bilan

**Vu pendant le TP :**

- visualisation de vos interfaces réseau (en GUI et en CLI)
- extraction des informations IP
  - adresse IP et masque
  - calcul autour de IP : adresse de réseau, etc.
- connaissances autour de/aperçu de :
  - un outil de diagnostic simple : `ping`
  - un outil de scan réseau : `nmap`
  - un outil qui permet d'établir des connexions "simples" (on y reviendra) : `netcat`
  - un outil pour faire des requêtes DNS : `nslookup` ou `dig`
  - un outil d'analyse de trafic : `wireshark`
- manipulation simple de vos firewalls

**Conclusion :**

- Pour permettre à un ordinateur d'être connecté en réseau, il lui faut **une liaison physique** (par câble ou par *WiFi*).  
- Pour réceptionner ce lien physique, l'ordinateur a besoin d'**une carte réseau**. La carte réseau porte une adresse MAC  
- **Pour être membre d'un réseau particulier, une carte réseau peut porter une adresse IP.**
Si deux ordinateurs reliés physiquement possèdent une adresse IP dans le même réseau, alors ils peuvent communiquer.  
- **Un ordintateur qui possède plusieurs cartes réseau** peut réceptionner du trafic sur l'une d'entre elles, et le balancer sur l'autre, servant ainsi de "pivot". Cet ordinateur **est appelé routeur**.
- Il existe dans la plupart des réseaux, certains équipements ayant un rôle particulier :
  - un équipement appelé *passerelle*. C'est un routeur, et il nous permet de sortir du réseau actuel, pour en joindre un autre, comme Internet par exemple
  - un équipement qui agit comme **serveur DNS** : il nous permet de connaître les IP derrière des noms de domaine
  - un équipement qui agit comme **serveur DHCP** : il donne automatiquement des IP aux clients qui rejoigne le réseau
  - **chez vous, c'est votre Box qui fait les trois :)**
