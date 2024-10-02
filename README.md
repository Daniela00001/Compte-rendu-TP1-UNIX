# Compte-rendu-TP1-UNIX

Compte rendu TP1 UNIX

Daniela Stinca
Installation Machine virtuelle
Après avoir téléchargé mini.iso depuis le :
https://ftp.lip6.fr/pub/linux/distributions/debian/dists/stable/main/installer-amd64/current/images/netboot/

Configuration SSH sur la Machine Virtuelle
1. Passage en mode root
La première étape consiste à accéder aux privilèges d'administration. J'ai utilisé la commande suivante :
su -
Cette commande permet de passer en mode root en demandant le mot de passe du compte administrateur. Cela m'a permis d'obtenir les droits nécessaires pour effectuer les modifications système.
2. Recherche du paquet SSH
Ensuite, j'ai recherché le paquet nécessaire pour installer le serveur SSH en utilisant la commande :
apt search ssh
Cette commande effectue une recherche dans les dépôts pour trouver les paquets relatifs à SSH, qui permet les connexions distantes. Le paquet que nous recherchions est openssh-server.

3. Installation du serveur SSH
Une fois le paquet identifié, j'ai procédé à son installation avec la commande suivante :
apt install openssh-server
Cette commande installe le serveur SSH, qui permet de configurer des connexions distantes sécurisées vers la machine virtuelle.
4. Modification du fichier de configuration SSH
Après l'installation du serveur SSH, j'ai modifié sa configuration pour permettre les connexions root. J'ai ouvert le fichier de configuration SSH avec l'éditeur de texte nano :
nano /etc/ssh/sshd_config
À l'intérieur de ce fichier, j'ai cherché la ligne suivante :
#PermitRootLogin prohibit-password
Cette ligne, par défaut, est commentée désactive les connexions root avec un mot de passe. Pour autoriser ces connexions, j'ai modifié la ligne comme suit :
PermitRootLogin yes
En retirant le #, j'ai activé la ligne, et en changeant prohibit-password par yes, j'ai permis les connexions root avec un mot de passe via SSH.
5. Redémarrage du service SSH
Après avoir modifié la configuration, j'ai redémarré le service SSH pour que les modifications prennent effet :
systemctl restart ssh
Redémarrer le service SSH permet de recharger le fichier de configuration mis à jour et d'appliquer les nouvelles règles. Désormais, il est possible de se connecter à distance en tant que root avec un mot de passe.
________________________________________
Connexion à la Machine Virtuelle depuis la Machine Hôte
1. Vérification de l'adresse IP de la machine virtuelle
Pour se connecter à la machine virtuelle depuis la machine hôte, il est nécessaire de connaître l'adresse IP de la machine virtuelle. J'ai utilisé la commande suivante pour afficher les interfaces réseau et trouver l'adresse IP :
ip a
La commande ip a affiché toutes les interfaces réseau et leurs adresses IP associées. 
J'ai repéré l'adresse IP sous l'interface réseau correspondante qui sera utilisée dans la commande SSH.
2. Connexion SSH à la machine virtuelle
Une fois l'adresse IP identifiée, j'ai effectué la connexion depuis la machine hôte à la machine virtuelle en utilisant la commande suivante :
ssh root@127.0.0.1
La commande SSH permet de se connecter à distance à une machine via le protocole sécurisé SSH.
3. Résultat de la connexion
Une fois la commande exécutée, le système m'a demandé le mot de passe du compte root de la machine virtuelle. 







Vérification des Paquets Installés et Configuration Système
Pour connaître le nombre total de paquets installés sur la machine virtuelle, j'ai utilisé la commande suivante :
dpkg -l | wc -l
La commande dpkg -l liste tous les paquets installés sur le système, et la commande wc -l permet de compter le nombre total de lignes. Cela me permet de vérifier que 349 paquets sont installés sur la machine.
![image](https://github.com/user-attachments/assets/20d32bd1-4bfc-45df-bf60-913d67c6784b)
