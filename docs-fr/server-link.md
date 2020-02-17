# Liaison Site-Serveur

## Introduction

Certaines fonctionnalités comme afficher les joueurs en
ligne ou exécuter des commandes nécessitent de relier votre serveur à votre
site web, sur Azuriom cela peut être fait de 3 facons différentes:

* Par Ping - **C'est la solution la plus simple, mais aussi la plus limitée**:
elle permet juste de récupérer les joueurs connectés du serveur
_(ne permet pas d'exécuter des commandes)_.

* Par Rcon - **C'est la solution intermédiaire**, elle permet de récupérer les informations 
de du serveur et d'exécuter des commandes.

* Par Plugin - **C'est la meilleure solution**, elle permet d'avoir les fonctionnalités du Rcon
tout en ajoutant des fonctionnalités plus poussées _(exemple: un système de statistique avancé)_.

## Liaison par Ping

Pour pouvoir lier un serveur avec un site sous Azuriom par ping, 
il faut justeajouter un nouveau serveur avec comme type de liaison "Ping"
et remplir les informations demandées _(le port par défaut de Minecraft est `25565`)_.

## Liaison par Rcon

Pour pouvoir lier un serveur avec un site sous Azuriom par Rcon, il faut:

1. Vous rendre dans le fichier `server.properties` de votre serveur

2. Configurer ce fichier de la façon suivante:
    * Mettre `enable-rcon` en `true`
    * Mettre `rcon.password` avec `votre-mot-de-passe`
    * Mettre `rcon.port` avec `votre-port` _(par défaut `25575`)_
    * Sauvegarder et redémarrer votre serveur
   
3. Vous rendre sur votre site et ajouter un nouveau serveur avec comme type de liaison "Rcon"
et remplir les informations demandées _(le port Rcon par defaut est `25575`)_.

## Liaison par plugin (AzLink)

### Qu'est-ce que AzLink ?

AzLink est un plugin de liaison site-serveur spécialement conçu pour et par Azuriom 
afin de vous permettre de lier un serveur à un site sous Azuriom de façon simple,
rapide et sécurisée.

Actuellement AzLink supporte Bukkit/Spigot, BungeeCord, Sponge et Velocity dans le même plugin.
Une version legacy est disponible pour les serveurs utilisant Bukkit 1.7.10.

### Installation

1. Télécharcher AzLink sur [notre site](https://azuriom.com/azlink)

2. Installer AzLink sur le serveur en le mettant dans le dossier `plugins/`
(ou `mods/` pour Sponge) et redémarrer le serveur.

3. Aller sur le site et ajouter un nouveau serveur avec comme type de liaison "AzLink", 
suivre les étapes de liaison et remplir les informations demandées.
