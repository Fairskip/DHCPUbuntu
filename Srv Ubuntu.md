# 1. Réseau Interne

Oracle VM VirtualBox-gestionnaire de machines > ServerU > Configuration > Réseau > Adapter 1 > Mode d'accès réseau > Réseau interne.

![réseau](https://github.com/Fairskip/DHCPUbuntu/blob/main/Reseau%20Interne.jpg)

<br>

# 2. Installation du serveur isc dhcp

Après avoir pris un snapshot, ouvrir le terminal et : 

```
sudo apt install isc-dhcp-server -y
```

<br>

# 3. Donner un IP Statique au serveur

1. Paramètre > Réseau > Filaire > Réglage > IPv4 > Manuel

ou icone **Filaire** sous forme de pile se situant en haut à droite de l'écran > Réglage > IPv4 => cocher **Manuel**

Adresse IPv4 | Masque de sous-réseau | DNS
:---: | :---: | :---:
172.20.0.254 | 255.255.0.0 | 172.20.0.254  

2. Entrer les données > Appliquer

![result](https://github.com/Fairskip/DHCPUbuntu/blob/main/IP%20Statique%20r%C3%A9sultat.jpg)

<br>

# 4. Définir les interfaces DHCPD

```
Sudo nano /etc/default/isc-dhcp-server
```

<br>

Modifier et Ajouter : 

```
INTERFACESv4="enp0s3"
```

<br>

# 5. Configurer DHCP Server

### 1. dhcpd.conf  

  
```
sudo nano /etc/dhcp/dhcpd.conf
```

<br>

Ajouter pour le bail des IP : 

```
default-lease-time 3600; 
max-lease-time 7200;
authoritative;
```

<br>

Ajouter un routeur, son masque sous-réseau et une étendue : 

```
subnet 172.20.0.0 netmask 255.255.255.0 {
option routers 172.20.0.254;
option subnet-mask 255.255.255.0;
range 172.20.0.100 172.20.0.200
}
```

<br>

Réserver une adresse IP pour une machine host (windy) avec son adresse mac.

```
host windy {
hardware ethernet 08:00:27:0c:e3:27;
fixed-address 172.20.0.10;
}
```

![dhcpd.conf](https://github.com/Fairskip/DHCPUbuntu/blob/main/Serveur%20Linux_dhcpd_config.png)

<br>

### 2. Lancement du Serveur et l'activer

```
sudo service isc-dhcp-server restart
```

<br>

# 6. Demande d'IP au serveur DHCP  

### 1. Fairskip-virtualbox (Host Ubuntu)

![Ubuntu](https://github.com/Fairskip/DHCPUbuntu/blob/main/SRV-U_HostU%20get%20IP.png)

<br>

### 2.Windy (Host windows)

![Windy](https://github.com/Fairskip/DHCPUbuntu/blob/main/SRV-U_HostW%20get%20IP.png)
