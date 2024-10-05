# Compte-rendu-TP1-UNIX

Compte rendu TP1 UNIX

Daniela Stinca


Installation Machine virtuelle


Après avoir téléchargé mini.iso depuis le :
https://ftp.lip6.fr/pub/linux/distributions/debian/dists/stable/main/installer-amd64/current/images/netboot/

J'ai créé une nouvelle image virtuelle appelée "LicencePro2024". J'ai dû supprimer et recréer la machine plusieurs fois, car je commetais des erreurs lors de l'installation en cochant les mauvaises options et en ne respectant pas les bonnes procédures.

Configuration SSH sur la Machine Virtuelle
1. Passage en mode root
La première étape consiste à accéder aux privilèges d'administration. J'ai utilisé la commande suivante :
<pre> 
  su - 
</pre>
Cette commande permet de passer en mode root en demandant le mot de passe du compte administrateur. Cela m'a permis d'obtenir les droits nécessaires pour effectuer les modifications système.
2. Recherche du paquet SSH
Ensuite, j'ai recherché le paquet nécessaire pour installer le serveur SSH en utilisant la commande :
<pre>
  apt search ssh
</pre>
Cette commande effectue une recherche dans les dépôts pour trouver les paquets relatifs à SSH, qui permet les connexions distantes. Le paquet que nous recherchions est openssh-server.

3. Installation du serveur SSH
Une fois le paquet identifié, j'ai procédé à son installation avec la commande suivante :
apt install openssh-server
Cette commande installe le serveur SSH, qui permet de configurer des connexions distantes sécurisées vers la machine virtuelle.
4. Modification du fichier de configuration SSH
Après l'installation du serveur SSH, j'ai modifié sa configuration pour permettre les connexions root. J'ai ouvert le fichier de configuration SSH avec l'éditeur de texte nano :
<pre>
  nano /etc/ssh/sshd_config
</pre>
À l'intérieur de ce fichier, j'ai cherché la ligne suivante :
<pre>
  #PermitRootLogin prohibit-password
</pre>
Cette ligne, par défaut, est commentée désactive les connexions root avec un mot de passe. Pour autoriser ces connexions, j'ai modifié la ligne comme suit :
<pre>
  PermitRootLogin yes
</pre>
En retirant le #, j'ai activé la ligne, et en changeant prohibit-password par yes, j'ai permis les connexions root avec un mot de passe via SSH.
5. Redémarrage du service SSH
Après avoir modifié la configuration, j'ai redémarré le service SSH pour que les modifications prennent effet :
systemctl restart ssh
Redémarrer le service SSH permet de recharger le fichier de configuration mis à jour et d'appliquer les nouvelles règles. Désormais, il est possible de se connecter à distance en tant que root avec un mot de passe.
_____________________________________________________________________________________________________________________________
Connexion à la Machine Virtuelle depuis la Machine Hôte
1. Vérification de l'adresse IP de la machine virtuelle
Pour se connecter à la machine virtuelle depuis la machine hôte, il est nécessaire de connaître l'adresse IP de la machine virtuelle. J'ai utilisé la commande suivante pour afficher les interfaces réseau et trouver l'adresse IP :
ip a
La commande ip a affiché toutes les interfaces réseau et leurs adresses IP associées. 
J'ai repéré l'adresse IP sous l'interface réseau correspondante qui sera utilisée dans la commande SSH.

J'ai configuré une redirection de port dans VirtualBox pour permettre l'accès SSH à ma machine virtuelle depuis l'hôte. Pour cela, j'ai redirigé le port 2222 sur la machine hôte vers le port 22 de la machine virtuelle.

Configuration de la redirection de port :

Port hôte : 2222
Port invité : 22

Après avoir complété la redirection de port, j'ai redémarré la machine virtuelle pour m'assurer que les paramètres de réseau et le serveur SSH étaient bien pris en compte.

Ensuite j'ai créé une paire de clés SSH pour faciliter les connexions sécurisées. Pour ce faire, j'ai utilisé la commande suivante dans le terminal de ma machine hôte (Windows) :ssh-keygen -t rsa

Dans un terminal sur ma machine hôte (Windows), j'ai utilisé la commande SSH pour me connecter à la machine virtuelle. Voici la commande utilisée :
<pre>ssh root@127.0.0.1 -p 2222</pre>

________________________________________________________________________________________________________________________________________________________
Vérification des Paquets Installés et Configuration Système
Pour connaître le nombre total de paquets installés sur la machine virtuelle, j'ai utilisé la commande suivante :
<pre>dpkg -l | wc -l</pre>
La commande dpkg -l liste tous les paquets installés sur le système, et la commande wc -l permet de compter le nombre total de lignes. Cela me permet de vérifier que 349 paquets sont installés sur la machine.

![aad](https://github.com/user-attachments/assets/b7a5fe83-5c80-4685-b2f2-3590c54eb80b)



2. Vérification de l'utilisation de l'espace disque
Pour connaître l'espace disque utilisé sur la machine virtuelle, j'ai exécuté la commande suivante :
<pre>
  df -h
</pre>
La commande df -h affiche l'utilisation de l'espace disque de manière lisible. Elle montre combien d'espace est utilisé et combien est disponible sur chaque partition du disque. 


![image](https://github.com/user-attachments/assets/64313d1c-6a31-449f-a4a3-e8e6719447df)



3. Affichage des locales
Pour vérifier la configuration de la langue locale sur la machine, j'ai utilisé cette commande :
<pre>
  echo $LANG
</pre>
La commande echo $LANG affiche la variable d'environnement LANG, qui indique la langue et la localisation configurée sur le système. Cela permet de savoir dans quel langage le système affiche ses messages et traite les formats régionaux.
4. Vérification du nom de la machine
Pour connaître le nom (hostname) de la machine virtuelle, j'ai utilisé la commande suivante :
hostname
La commande hostname retourne le nom actuel de la machine.  (serveur1)

5. Vérification du domaine de la machine
Si la machine est configurée avec un domaine, je peux l'afficher à l'aide de la commande suivante :
<pre>
  hostname -d
</pre>
Cela donne : ufr-info-p6.jussieu.fr

La commande <pre>
hostname -d
</pre> affiche le nom de domaine auquel la machine est associée, si elle est configurée dans un domaine. 

________________________________________________________________________________________________________________________________________________________
Vérification des Dépôts APT et des Comptes Utilisateurs
1. Vérification des dépôts APT activés
Pour identifier les dépôts APT configurés sur la machine, j'ai utilisé la commande suivante :
<pre>
  cat /etc/apt/sources.list | grep -v -E '^#|^$'
</pre>
La commande cat /etc/apt/sources.list affiche le contenu du fichier qui contient les sources de dépôts APT pour l'installation de paquets. Ensuite, la commande grep -v -E '^#|^$' filtre les résultats pour exclure les lignes commentées (celles qui commencent par #) et les lignes vides. Cela permet d'obtenir une liste claire des dépôts actifs, nécessaires pour installer des paquets à partir d'Internet.
![image](https://github.com/user-attachments/assets/d09ddd55-a9f2-4714-9338-86c10340292b)

2. Vérification des comptes dans /etc/shadow
Pour voir les comptes ayant un mot de passe configuré sur la machine, j'ai utilisé la commande suivante :
<pre>
  cat /etc/shadow | grep -vE ':\*:|:!\*:'
</pre>
Le fichier /etc/shadow contient les informations de sécurité pour les comptes utilisateurs, y compris les mots de passe chiffrés. La commande grep -vE ':\*:|:!\*:' exclut les lignes qui contiennent :* ou :!, ce qui signifie que ces comptes n'ont pas de mot de passe configuré. Ainsi, cette commande permet de lister uniquement les comptes avec un mot de passe valide.

3. Vérification des comptes utilisateurs dans /etc/passwd
Pour afficher les comptes utilisateurs, à l'exclusion des comptes associés à des services ou des comptes qui ne peuvent pas se connecter directement, j'ai exécuté la commande suivante :
<pre>
  cat /etc/passwd | grep -vE 'nologin|sync'
</pre>
Le fichier /etc/passwd contient les informations de base sur tous les comptes utilisateurs du système. La commande grep -vE 'nologin|sync' filtre les résultats pour exclure les comptes avec des shells de connexion nologin (ceux qui ne peuvent pas se connecter) et le compte sync. Cela permet d’obtenir une liste claire des utilisateurs qui ont la possibilité de se connecter à la machine.

________________________________________________________________________________________________________________________________________________________
Vérification des Partitions et de l'Utilisation de l'Espace Disque
1. Vérification des partitions avec <pre>fdisk -l</pre>
J'ai utilisé la commande suivante pour afficher la table des partitions et les informations sur le disque :
<pre>fdisk -l</pre>
La commande fdisk -l permet de lister toutes les partitions présentes sur les disques connectés à la machine. Elle fournit des informations détaillées, telles que :
La taille de chaque partition
Le type de système de fichiers
Les points de montage Cela m'a permis d'avoir un aperçu clair de la structure des partitions sur le système.
![image](https://github.com/user-attachments/assets/e86c36f8-2335-4268-8789-5454a987b7f7)

2. Analyse détaillée des partitions avec fdisk -x
Pour obtenir des détails supplémentaires sur la géométrie et les structures de disque, j'ai utilisé la commande :
<pre>fdisk -x</pre>
La commande fdisk -x offre une analyse plus approfondie que fdisk -l. Elle fournit des informations avancées sur la disposition physique des disques, telles que :
Le nombre de cylindres
Le nombre de têtes et de secteurs. Cette information est utile pour examiner la structure des partitions et pour effectuer des opérations de partitionnement plus complexes si nécessaire.


3. Vérification de l'utilisation de l'espace disque avec df -h
J'ai également vérifié l'utilisation de l'espace disque sur la machine avec la commande :
<pre>df -h</pre>
La commande df -h affiche une vue d'ensemble de l'utilisation de l'espace disque sur toutes les partitions. Avec l'option -h, les tailles sont présentées de manière lisible pour l'humain. Cela permet d'identifier combien d'espace est utilisé et combien reste disponible sur chaque partition, ce qui est essentiel pour la gestion de l'espace disque.

________________________________________________________________________________________________________________________________________________________
3. Aller plus loin
Preseed :
Le preseed est un fichier de configuration qui permet d'automatiser l'installation de Debian et dérivés. Cela sert à répondre automatiquement aux questions posées durant l'installation comme les paramètres de partitionnement, la configuration réseau, etc.

Rescue Mode : 
Si le mot de passe root est oublié, on peut redémarrer la machine virtuelle en mode "rescue" ou "recovery" et suivre les étapes pour changer le mot de passe :

-Redémarrer la machine.
-Accèder au mode "rescue" dans GRUB.
-Une fois dans le mode "rescue", on utilise la commande suivante pour réinitialiser le mot de passe root :
passwd root
Redimensionner une partition : 
Pour redimensionner la partition racine sans réinstaller, on peut utiliser gparted ou les outils resize2fs et fdisk. Il faut démonter la partition, redimensionner avec fdisk, puis ajuster avec resize2fs.

