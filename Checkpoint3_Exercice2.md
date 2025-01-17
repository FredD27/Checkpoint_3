# Exercice 2 : Manipulations pratiques sur VM Linux

## Partie 1 : Gestion des utilisateurs

Q.2.1.1 Sur le serveur, créer un compte pour ton usage personnel.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 105851.png" width=400></P>

Q.2.1.2 Quelles préconisations proposes-tu concernant ce compte ?
Ce nouvel utilisateur doit faire parti des sudoers afin qu'il ait les privilèges nécéssaires aux manipulations suivantes.

## Partie 2 : Configuration de SSH

Un serveur SSH est lancé sur le port par défaut.
Il est possible de s'y connecter avec n'importe quel compte, y compris le compte root.

Q.2.2.1 Désactiver complètement l'accès à distance de l'utilisateur root.

1. Editer le fichier `/etc/ssh/sshd_config`

```bash
nano /etc/ssh/sshd_config
```

Enlever le # pour décommenter la ligne `PermitRootLogin prohibit-password`

Q.2.2.2 Autoriser l'accès à distance à ton compte personnel uniquement.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 110731.png" width=400></P>

Q.2.2.3 Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe

- Supprimer le # pour désactiver le commentaire de la ligne `PubkeyAuthentication yes`
- Laisser la ligne `PasswordAuthentication yes` commentée pour que l'option reste désactivée

## Partie 3 : Analyse du stockage

Q.2.3.1 Quels sont les systèmes de fichiers actuellement montés ?

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 112454.png" width=600></P>

Les différents sytèmes de fichiers sont :

- ext2
- ext4
- lvm
- swap

Q.2.3.2 Quel type de système de stockage ils utilisent ?

- Raid
- Volume Group : vg

Q.2.3.3 Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID

1. Dans VirtualBox, ajouter le nouveau disque. Le nouveau disque `sdb` de 8G apparait bien

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 113324.png" width=400></P>

2. On constate que le Raid1 est bien endommagé :

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 113724.png" width=400></P>

3. Ajout du disque `sdb` au Raid existant

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 114353.png" width=600></P>

4. Vérification, le disque est bien reconstruit

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 114722.png" width=600></P>

Q.2.3.4 Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.

1. Création d'un Logical Volume de 2G

```bash
lvcreate -L 2G -n lv_save_datas /dev/mdp05
```

2. Monter le volume

3. Ajouter la ligne dans le fichier `etc/fstab` pour qu'il soit monté automatiquement à chaque démarrage

```
/dev/md0p5/lv_save_datas /mnt/<...> ext4 defaults 0 2
```

Q.2.3.5 Combien d'espace disponible reste-t-il dans le groupe de volume ?

## Partie 4 : Sauvegardes

Le logiciel bareos est installé sur le serveur.
Les composants bareos-dir, bareos-sd et bareos-fd sont installés avec une configuration par défaut.

Q.2.4.1 Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.

## Partie 5 : Filtrage et analyse réseau

Q.2.5.1 Quelles sont actuellement les règles appliquées sur Netfilter ?

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 125737.png" width=600></P>

Q.2.5.2 Quels types de communications sont autorisées ?

- "tcp dport 22 accept" ==> connexion ssh autorisée en sortie
- "ip protocol icmp accept" ==> le ping est autorisé
- "ipv6-icmp accept" ==> le ping avec une adresse IPv6 est accepté

Q.2.5.3 Quels types sont interdit ?

Tout est autorisé car il n'y aucune règle qui été ajoutée pour finir la chaîne par un `Drop`.

Q.2.5.4 Sur nftables, ajouter les règles nécessaires pour autoriser bareos à communiquer avec les clients bareos potentiellement présents sur l'ensemble des machines du réseau local sur lequel se trouve le serveur.
Rappel : Bareos utilise les ports TCP 9101 à 9103 pour la communication entre ses différents composants.
`nft add rule inet_filter_table in_chain dport 9101 accept`

## Partie 6 : Analyse de logs

Q.2.6.1 Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun :

Obtenir une liste des connexions au serveur

```bash
cat /var/log/auth.log
```

On peut lister les échecs de connexions avec la commande `lastb` avec la date et l'heure de la tentative

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 122432.png" width=600></P>

l'utilisateur "root" ou "wilder" a tenté de se connecter avec l'ip **10.0.0.199**, vu avec la commande `last`
