# Liaison Site-Serveur

## Introduction

Afin de pouvoir lier votre serveur avec votre site sous Azuriom, vous aurez trois possibilités:

* Par Ping - **C'est la solution la plus limitée**, elle vous permet juste de récupérer 
les joueurs connectés de votre serveur. _(ne permets pas d'exécuter des commandes)_

* Par Rcon - **C'est la solution intermédiaire**, elle vous permet de récupérer les informations 
de votre serveur et d'exécuter des commandes.

* Par Plugin - **C'est la solution optimale**, elle vous permettra d'avoir les fonctionnalités du Rcon, 
mais avec des fonctionnalités plus poussées _(exemple: un système de statistique avancé)_

## Liaison par Ping

Pour pouvoir lier votre serveur avec votre site sous Azuriom par ping, 
vous avez juste à ajouter un nouveau serveur avec comme type de liaison "Ping"
et remplir les informations demandées _(le port par défaut est 25565)_.

## Liaison par Rcon

Pour pouvoir lier votre serveur avec votre site sous Azuriom par Rcon, 
Vous devez:

1. Vous rendre dans le fichier `server.properties` de votre serveur

2. Configurer ce fichier de la façon suivante:
    * Mettre `enable-rcon` en `true`
    * Mettre `rcon.password` avec `votre-mot-de-passe`
    * Mettre `rcon.port` avec `votre-port` _(par défaut 25575)_
    * Sauvegarder et redémarrer votre serveur
   
3. Vous rendre sur votre site et ajouter un nouveau serveur avec comme type de liaison "Rcon"
et remplir les informations demandées. _(le port Rcon par defaut est 25575)_

## Liaison par Plugin

### Qu'est-ce que AzLink ?

AzLink est un plugin de liaison site-serveur spécialement conçu pour Azuriom 
afin de vous permettre de lier votre serveur à votre site de façon simple, rapide et sécurisée.
AzLink supporte Bukkit, BungeeCord, Sponge et Velocity dans le même plugin. Une version legacy est disponible
pour Bukkit 1.7.10.

### Installation

1. Télécharcher AzLink sur [notre site](https://azuriom.com/azlink)

2. Installer le sur votre serveur dans le dossier `Plugin/` et redémarrer votre serveur

3. Vous rendre sur votre site et ajouter un nouveau serveur avec comme type de liaison "AzLink", 
suivre les étapes de liaison et remplir les informations demandées.
