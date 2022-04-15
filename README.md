
# EtherChannel
Notion d'agrégation de liens

### Qu'est-ce l'EtherChannel?

```
EtherChannel est une technologie d'agrégation de liens qui permet d'assembler plusieurs liens physiques.
EtherChannel peut être utilisé sur des liens cuivre en paire torsadée aussi bien que sur fibre optique
monomode et multimode (sur toutes les interfaces définies dans le cadre du standard IEEE 802.3).
Toutes les interfaces groupées doivent cependant fonctionner à la même vitesse
```

#### Avantages

```
-Augmentation de la bande passante
-Redondance des liens
-Tolérance aux pannes
```

### PORTCHANNEL
```
Lorsqu'un EtherChannel est configuré, l'interface virtuelle résultante est appelée un canal de port
(*Port-Channel* abrégé *Po*). Les interfaces physiques sont regroupées dans une interface canal de port.
Tous les liens participant à EtherChannel partagent la même adresse de couche 2 (adresse MAC).
Cela rend EtherChannel transparent aux protocoles réseau, aux applications et aux utilisateurs,
car ils ne voient qu'une seule connexion logique et n'ont pas connaissance des différents liens physiques.
```

_Note: Un EtherChannel peut être de niveau 2 ou niveau 3._<br>
_Note bis: Dans un EtherChannel, tous les ports doivent obligatoirement avoir une vitesse, un paramètre de bidirectionnalité et des informations VLAN identiques. Toute modification d'un port après la création du canal modifie également tous les autres ports du canal._


## PROTOCOLES D'AGRÉGATION DE LIENS

### PAGP
```
Le protocole PAgP est un protocole propriétaire de Cisco qui facilite la création automatique de liaisons
EtherChannel. Quand une liaison EtherChannel est configurée grâce à PAgP, des paquets PAgP sont envoyés
entre les ports compatibles EtherChannel pour négocier la formation d'un canal. Quand PAgP identifie des
liaisons Ethernet associées, il groupe les liaisons dans un EtherChannel. L'EtherChannel est ensuite ajouté
à l'arbre recouvrant comme port unique.

S'il est activé, PAgP gère également l'EtherChannel. Les paquets PAgP sont envoyés toutes les 30 secondes.
PAgP vérifie la cohérence de la configuration et gère les ajouts de liaison et les défaillances entre
deux commutateurs. Il garantit que tous les ports ont le même type de configuration
quand un EtherChannel est créé.
```

### LACP
```
Le protocole de contrôle d'agrégation de liens est une norme IEEE définie dans IEEE 802.3ad. LACP permet
aux périphériques d'envoyer des LACPDU (Link Aggregation Control Protocol Data Units) entre eux pour
établir une connexion d'agrégation de liens. Vous devez toujours configurer le lien d'agrégation sur
chaque périphérique, mais le protocole LACP permet d'éviter l'un des problèmes les plus courants qui
peuvent survenir lors de la configuration de l'agrégation de liens : les paramètres du lien d'agrégation
sont mal configurés. Si les périphériques détectent qu'ils ne peuvent pas établir de connexion d'agrégation
de liens, ils n'essaient pas de l'établir et la liaison s'affiche comme « hors service » dans l'interface
d'administration.

Une autre caractéristique utile du LACP est que lorsqu'une liaison membre cesse d'envoyer des LACPDU
(si le câble est débranché, par exemple), il est retiré du lien d'agrégation.
Cela permet de minimiser la perte de paquets.

Les deux périphériques doivent prendre en charge le LACP pour que vous puissiez configurer un lien d'agrégation
dynamique entre ces périphériques. Nous vous recommandons d'utiliser le LACP au lieu d'un lien d'agrégation
statique chaque fois que les deux périphériques prennent en charge le LACP.
```

## CONFIGURATION
```
Il y a deux façons de configurer EtherChannel. La première est de procéder manuellement(via le mode “on”) en
saisissant une commande sur chaque port à agréger, des deux côtés de la liaison. La seconde est de laisser
faire la configuration automatique, à l'aide de l'un des deux protocoles Port Aggregation Protocol (PAgP) et
Link Aggregation Control Protocol (LACP).
```

![image](https://user-images.githubusercontent.com/83721477/163565061-afd5c399-58ab-4f90-a6c2-e5e75a4f637e.png)

### ETHERCHANNEL LAYER 2
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# channel-group 1 mode on
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
```

### ETHERCHANNEL LAYER 3
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# no ip address
Switch(config-range)# channel-group 1 mode on
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B 
Switch(config-if)# ip address X.X.X.X X.X.X.X
```

### PAGP LAYER 2
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# channel-group 1 mode [ Desirable | Auto ]
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
```

### PAGP LAYER 3
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# no ip address
Switch(config-range)# channel-group 1 mode [ Desirable | Auto ]
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B 
Switch(config-if)# ip address X.X.X.X X.X.X.X
```

### LACP LAYER 2
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# channel-group 1 mode [ Active | Passive ]
Switch(config-range)# channel-protocol lacp
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
```

### LACP LAYER 3
```
Switch(config)# interface range FastEthernet 0/1 -4
Switch(config-range)# no ip address
Switch(config-range)# channel-group 1 mode [ Active | Passive ]
Switch(config-range)# channel-protocol lacp
Switch(config-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# description VERS_SWITCH_B
Switch(config-if)# ip address X.X.X.X X.X.X.X
```
