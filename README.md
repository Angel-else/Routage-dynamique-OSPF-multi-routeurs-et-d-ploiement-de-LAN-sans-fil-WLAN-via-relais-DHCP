# Routage dynamique OSPF multi routeurs et déploiement de LAN sans fil WLAN via relais DHCP.
Ce projet consiste en la conception et la configuration d'une infrastructure réseau multi-sites simulée sur Cisco Packet Tracer. L'objectif principal était de mettre en place une communication fluide par connexion sans fil et automatique entre trois segments réseaux distincts tout en centralisant la gestion des adresses IP.

# Fonctionalités techniques

Routage Dynamique OSPF. Interconnexion de 3 routeurs Cisco 2901 formant une topologie en boucle (redondance). Configuration d'interfaces Loopback sur chaque routeur pour garantir un Router-ID stable et indépendant des interfaces physiques. Convergence rapide et réacheminement automatique du trafic en cas de panne de lien.
- Gestion Centralisée des Adresses IP avec un serveur DHCP. Un routeur principal configuré en serveur DHCP Central. Définition de 3 pools d'adresses distincts (192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24). Exclusion des adresses IP statiques.
- Relais DHCP. Activation du relais DHCP sur les interfaces LAN des routeurs distants via la commande ip helper-address. Conversion des requêtes Broadcast des clients sans fil/filaires distants en requêtes Unicast vers le serveur central.
- Extension Sans Fil Multi-AP (WLAN). Déploiement de multiples points d'accès (Access Points) connectés aux switchs de distribution. Segmentation logique/physique par zone (AP-DG, AP-ADMIN, AP-EXEC, AP-GIZ, AP-SECRE, etc.)
- Connexion automatique des terminaux mobiles (Laptops, Smartphones, Imprimantes Wi-Fi) avec obtention d'une IP dynamique distante.

# Configurations Clés (Cisco)
# Configuration OSPF 
Router(config-if)# ip address 1.1.1.1 255.255.255.255
Router(config-if)# exit
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 192.168.30.0 0.0.0.255 area 0
Router(config-router)# network 10.0.0.0 0.0.0.3 area 0

# Configuration du Serveur DHCP (Routeur Central)Router(config)
ip dhcp excluded-address 192.168.10.1 192.168.10.10
Router(config)# ip dhcp pool LAN_SITE_1
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8

# Configuration du Relais DHCP 
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.30.1 255.255.255.0
Router(config-if)# ip helper-address 192.168.10.1
Router(config-if)# no shutdown

# Tests et Validation des test d'attribution DHCP 
Les PCs et imprimantes Wi-Fi sur les sites distants reçoivent correctement une IP de leur pool respectif. Test de Connectivité (Ping) : Validation d'u ping de bout en bout entre un hôte de AP-GIZ (Site 3) et un hôte de AP-ADMIN (Site 2).Table de Routage OSPF : Vérification des adresses voisines (show ip ospf neighbor) et des routes apprises (show ip route ospf).
# Fichiers inclus dans ce dépôt Réseau_OSPF_DHCP_WLAN.pkt
Le fichier de simulation Cisco Packet Tracer.topology.png : La capture d'écran de la topologie réseau complète.
