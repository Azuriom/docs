# Installation

## Prérequis

Pour fonctionner, Azuriom nécessite simplement un **serveur web avec PHP** disposant d'au moins **100 MO**
d'espace disque ainsi que des prérequis suivants:

 - PHP 7.2 ou plus récent
 - Réécriture d'URL
 - Extension PHP BCMath
 - Extension PHP Ctype
 - Extension PHP JSON
 - Extension PHP Mbstring
 - Extension PHP OpenSSL
 - Extension PHP PDO
 - Extension PHP Tokenizer
 - Extension PHP XML
 - Extension PHP cURL
 - Extension PHP GD2
 - Extension PHP Zip

Il est également très fortement recommandé de posséder **une base de données MySQL/MariaDB ou PostgreSQL**.

## Installation

Azuriom peut être installé de deux façons différentes:

- De façon automatique grâce à l'installateur _(recommandé pour la plupart des utilisateurs)_ 
- De façon manuelle via [Composer](https://getcomposer.org/) _(recommandé pour les utilisateurs experimentés ou souhaitant contribuer à Azuriom)_

### Installation Automatique

1. Télécharger la dernière version d'Azuriom sur [notre site](https://azuriom.com/download).

2. Extraire l'archive à la racine de votre site web.

3. Se rendre sur `votre-site.fr/install.php` et suivre les étapes de l'installation.

### Installation Manuelle

1. Cloner le repos GitHub (https://github.com/Azuriom/Azuriom.git) ou [télécharger une release](https://github.com/Azuriom/Azuriom/releases).

2. Copier le fichier `.env.example` vers `.env` et indiquer les informations de connexion à la base de données

3. Mettre les droits d'écriture aux dossiers `storage/`, `bootstrap/cache/`, `resources/themes` et `plugins`:
```
chmod -R 770 storage bootstrap/cache resources/themes plugins
```

4. Générer la clé secrète:
```
php artisan key:generate
```

5. Installer les dépendances avec Composer:
```
composer install
```

  * Si vous utilisez Azuriom en production, vous pouvez optimiser l'autoloader de Composer en utilisant cette commande: 
 ```
composer install --optimize-autoloader --no-dev
 ```

6. Mettre en place la base de données:
 ```
php artisan migrate --seed
 ```

7. Créer un compte administrateur _(Optionnel, mais très utile)_:
```
php artisan artisan user:create --admin
```

8. Configurer votre serveur web pour qu'il pointe vers le dossier `public/` _(Optionnel, mais très fortement recommandé)_

9. Mettre en place le scheduler _(Optionnel, mais recommandé)_:

Vous devez configurer votre serveur pour que la commande `php artisan schedule:run` soit exécutée toutes les minutes, par exemple en ajoutant une entrée Cron:
 ```
* * * * * cd /chemin-vers-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```
Cela peut être fait en modifiant la configuration de crontab avec la commande `crontab -e`.
