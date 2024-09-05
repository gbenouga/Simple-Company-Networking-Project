La société XYZ est une entreprise en pleine croissance située dans l'est de l'Australie, avec plus de 2 millions de clients dans le monde entier. L'entreprise se spécialise dans la vente et l'achat de produits alimentaires, principalement gérés depuis le siège social. La société envisage d'ouvrir une succursale près du village local de Bonalbo. Par conséquent, elle souhaite faire appel à de jeunes diplômés en informatique pour concevoir le réseau de la succursale. Le réseau de cette succursale doit fonctionner indépendamment de celui du siège. Étant donné qu'il s'agit d'un petit réseau, l'entreprise a les exigences suivantes pour sa mise en œuvre :

    - Un routeur et un commutateur (switch) doivent être utilisés (tous deux des produits CISCO).
    - 3 départements (Admin/IT, Finance/RH, et Service clientèle/Réception).
    - Chaque département doit être dans un VLAN différent.
    - Chaque département doit disposer d'un réseau sans fil pour les utilisateurs.
    - Les dispositifs du réseau doivent obtenir une adresse IPv4 automatiquement.
    - Les dispositifs de tous les départements doivent pouvoir communiquer entre eux.

En supposant que le FAI (fournisseur d'accès Internet) a attribué un réseau de base avec l'adresse 192.168.1.0, en tant que jeune ingénieur réseau récemment embauché, vous êtes chargé de concevoir et de mettre en œuvre un réseau en tenant compte des exigences ci-dessus.



### Conception du Réseau pour la Succursale XYZ

### 1. **Liste des Équipements**
- **Routeur Cisco (Router 1)**
- **Commutateur Cisco (Switch 1)**
- **Point d'Accès Cisco**
- **Ordinateurs pour chaque département**
- **Imprimantes pour chaque département**

### 2. **Topologie du Réseau**
La topologie du réseau est conçue pour inclure trois départements, chacun dans un VLAN distinct. Le routeur assure le routage inter-VLAN, et tous les dispositifs obtiennent des adresses IP automatiquement via DHCP. Le réseau est également configuré pour avoir un accès sans fil pour chaque VLAN.

### 3. **Configuration des VLANs**

**a. Créer les VLANs sur le commutateur :**
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name Admin_IT
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Finance_RH
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name Service_Client
Switch(config-vlan)# exit
```

**b. Attribuer les ports aux VLANs correspondants :**
```bash
Switch(config)# interface range fa0/1 - 5
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10  # Attribuer les ports au VLAN 10
Switch(config-if-range)# exit

Switch(config)# interface range fa0/6 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20  # Attribuer les ports au VLAN 20
Switch(config-if-range)# exit

Switch(config)# interface range fa0/11 - 15
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30  # Attribuer les ports au VLAN 30
Switch(config-if-range)# exit
```

### 4. **Configuration du Routage Inter-VLAN**

**a. Configurer les sous-interfaces sur le routeur :**
```bash
Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface g0/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# exit
```

**b. Activer l'interface principale (g0/0) :**
```bash
Router(config)# interface g0/0
Router(config-if)# no shutdown
Router(config-if)# exit
```

### 5. **Configuration du Serveur DHCP sur le Routeur**

**a. Configurer le DHCP pour chaque VLAN :**
```bash
Router(config)# ip dhcp pool VLAN10_POOL
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp pool VLAN20_POOL
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit

Router(config)# ip dhcp pool VLAN30_POOL
Router(dhcp-config)# network 192.168.30.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.30.1
Router(dhcp-config)# dns-server 8.8.8.8
Router(dhcp-config)# exit
```

### 6. **Configuration du Réseau Sans Fil**

**a. Configurer le point d'accès pour chaque VLAN :**
```bash
AP(config)# interface Dot11Radio0
AP(config-if)# ssid Admin_IT_WIFI
AP(config-ssid)# vlan 10
AP(config-ssid)# authentication open
AP(config-ssid)# exit

AP(config)# interface Dot11Radio0
AP(config-if)# ssid Finance_RH_WIFI
AP(config-ssid)# vlan 20
AP(config-ssid)# authentication open
AP(config-ssid)# exit

AP(config)# interface Dot11Radio0
AP(config-if)# ssid Service_Client_WIFI
AP(config-ssid)# vlan 30
AP(config-ssid)# authentication open
AP(config-ssid)# exit
```

### 7. **Vérification et Test**

**a. Vérifier la configuration du routage inter-VLAN :**
```bash
Router# show ip route
```

**b. Vérifier la configuration des VLANs sur le commutateur :**
```bash
Switch# show vlan brief
```

**c. Vérifier la connectivité entre les VLANs :**
- **Ping entre les hôtes de différents VLANs pour vérifier la communication.**

### 8. **Conclusion**
Avec ces configurations, le réseau de la succursale XYZ est conçu pour être efficace, sécurisé et capable de répondre aux besoins de l'entreprise. Les VLANs assurent la segmentation du réseau, et le routage inter-VLAN permet une communication fluide entre les départements. Le DHCP automatisé et l'accès sans fil offrent une flexibilité aux utilisateurs, permettant au réseau de fonctionner de manière autonome et fiable.