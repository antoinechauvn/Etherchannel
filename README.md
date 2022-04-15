
# EtherChannel
Notion d'agrégation de liens

### Qu'est-ce l'EtherChannel?

```
EtherChannel est une technologie d'agrégation de liens qui permet d'assembler plusieurs liens physiques.
EtherChannel peut être utilisé sur des liens cuivre en paire torsadée aussi bien que sur fibre optique monomode et multimode (sur toutes les interfaces définies dans le cadre du standard IEEE 802.3)
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
Lorsqu'un EtherChannel est configuré, l'interface virtuelle résultante est appelée un canal de port (*Port-Channel* abrégé *Po*).
Les interfaces physiques sont regroupées dans une interface canal de port.
Tous les liens participant à EtherChannel partagent la même adresse de couche 2 (adresse MAC). Cela rend EtherChannel transparent aux protocoles réseau, aux applications et aux utilisateurs, car ils ne voient qu'une seule connexion logique et n'ont pas connaissance des différents liens physiques.
```

_Note: Un EtherChannel peut être de niveau 2 ou niveau 3_

## PROTOCOLES D'AGRÉGATION DE LIENS

### PAGP
```

```

### LACP
```

```

## CONFIGURATION
```
Il y a deux façons de configurer EtherChannel. La première est de procéder manuellement(via le mode “on”) en saisissant une commande sur chaque port à agréger, des deux côtés de la liaison. La seconde est de laisser faire la configuration automatique, à l'aide de l'un des deux protocoles Port Aggregation Protocol (PAgP) et Link Aggregation Control Protocol (LACP).
```

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
