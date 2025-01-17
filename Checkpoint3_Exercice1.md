# Exercice 1 : Manipulations pratiques sur VM Windows

## Partie 1 : Gestion des utilisateurs

L'utilisateur Kelly Rhameur a quitté l'entreprise.
Elle est remplacée par Lionel Lemarchand

Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs de société que Kelly Rhameur.
Kelly

1. Tout d'abord je retrouve Kelly Rhameur dans l'outils **Active Directory Users ans Computers**. Grâce à la commande sous Powershell, je constate en lisant son **DistinguishedName** qu'elle fait partie des Unités Organisationnelles suivantes : "LabUsers", "DirectionDesRessourcesHumaines". Je constate aussi qu'elle appartient au groupe "GrpUsersDirectionDesRessourcesHumaines", son manager est "Camille Martin", sa compagnie "CyberOps"

```
Get -ADUser -Filter "Name -like '*Rhameur*'"
```

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 104857.png" width=600></P>

2. Je peux mainteant créer mon nouvel utilisateur. Je clic-droit sur l'OU `DirectionsDesRessourcesHumaines` ==> `New` ==> `User`
   Je rentre son nom, son prénom.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 101302.png" width=600></P>

Je l'ajoute au même groupe "GrpUsersDirectionDesRessourcesHumaines". Clic-droit sur l'utilisateur Lionel Marchand ==> `Add to group`

Q.1.1.2 Créer une OU DeactivatedUsers et déplace le compte désactivé de Kelly Rhameur dedans.

1. Clic-droit sur `LabUsers` ==> `New...` ==> `Organizationnal Unit`

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 101648.png" width=500></P>

1. Clic-droit sur "Kelly Rhameur" ==> `Move...` ==> `DesactivatedUsers`

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 102043.png" width=500></P>

Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.

Aller dans le groupe concerné, sélectionner "Kelly.Rhameur", cliquer sur `Remove`, `Ok` à la question êtes-vous sûr, puis `Apply`.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 102539.png" width=400></P>

Q.1.1.4 Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.

## Partie 2 : Restriction utilisateurs

Q.1.2.1 Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 103048.png" width=400></P>

Q.1.2.2 De même, bloquer sa connexion au seul ordinateur CLIENT01.

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 103255.png" width=400></P>

Q.1.2.3 Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.

## Partie 3 : Lecteurs réseaux

Q.1.3.1 Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.

1. Ouvrir la fenêtre dans _Tools_ ==> _Group Policy Management_
2. Puis créer une nouvelle GPO "USERS_Mount_Disks"

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 104126.png" width=400></P>

3. Supprimer de l'onglet _Scope_ Authenticated Users

<P ALIGN="center"><IMG src=".\Ressources\Capture d'écran 2025-01-17 103725.png" width=400></P>
