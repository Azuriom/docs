# Installation

## Prérequis

Pour fonctionner, Azuriom nécessite simplement un **serveur web avec PHP** disposant d'au moins **100 MO**
d'espace disque ainsi que des prérequis suivants :

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

### Installation des pré-requis

Si vous utilisez un VPS ou un serveur dédié, il sera sûrement nécessaire d'installer
vous-même Nginx, PHP et MySQL, cela peut se faire avec les commandes suivantes :
```
apt update && apt upgrade

apt install nginx zip curl

apt install lsb-release apt-transport-https ca-certificates
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
apt update
apt install php7.4 php7.4-fpm php7.4-mysql php7.4-pgsql php7.4-sqlite php7.4-bcmath php7.4-mbstring php7.4-xml php7.4-curl php7.4-zip php7.4-gd
```

Une fois les pré-requis installés, vous devez configurer le serveur web. Pour ce
faire, des explications sont disponibles en bas de cette page.

> {info} Si vous préférez, vous pouvez aussi utiliser ce
[script non-officiel d'installation automatique](https://github.com/AzuriomCommunity/Script-AutoInstall)
qui installera tous les pré-requis automatiquement.

## Hébergeurs

Azuriom peut être installé sur n'importe quel VPS ou serveur dédié ainsi que sur
la majorité des hébergeurs web ayant les prérequis ci-dessus, cependant certains
hébergeurs ne sont pas compatibles ou nécessitent des petites adaptations :
* [000webhost](https://www.000webhost.com/) : Incompatible (La fonction `putenv()` est désactivée).
* [InovaPerf](https://inovaperf.fr/) : Incompatible avec l'offre gratuite (La fonction `fsockopen()` est désactivée).
* [BoxToPlay](https://www.boxtoplay.com/) : Incompatible (Extensions BCMath et XML manquantes)
et très fortement déconseillé pour un serveur MineCraft.
* [mTxServ](https://mtxserv.com/) : Incompatible (La fonction `symlink()` est désactivée).
* [LiveHost](https://www.livehost.fr/) : Incompatible (La fonction `symlink()` est désactivée).
* [Ionos](https://www.ionos.fr/) : En cas d'erreur 500 après l'installation,
    il faut simplement modifier le fichier `.htaccess` à la racine du site et
    rajouter `RewriteBase /` juste au-dessus de la ligne `RewriteEngine On`,
    ce qui doit donner quelque chose comme ça :
    ```
        ...
        </IfModule>

        RewriteBase /
        RewriteEngine On
    
        # Handle Authorization Header
        ...
    ```

Si vous recherchez un hébergeur web associatif **performant à petit prix**, nous
vous recommandons notre partenaire [NiHost](https://www.ni-host.com/).

## Installation

Azuriom peut être installé de deux façons différentes :

- De façon simple & automatique grâce à l'installateur _(recommandé pour la plupart des utilisateurs)_ 
- De façon manuelle via [Composer](https://getcomposer.org/) _(recommandé pour les utilisateurs expérimentés ou souhaitant contribuer à Azuriom)_

### Installation Automatique

1. Télécharger la dernière version de l'installateur d'Azuriom sur [notre site](https://azuriom.com/download).

1. Extraire l'archive à la racine de votre site web.

1. Mettre les droits d'écriture à la racine du serveur web :
    ```
    chmod -R 755 /var/www/azuriom
    ```
    (en remplaçant simplement `/var/www/azuriom` par l'emplacement du site)
    
    Si l'utilisateur actuel n'est pas le même que l'utilisateur du serveur web,
    il peut être nécessaire de changer le propriétaire des fichiers :
    ```
    chown -R www-data:www-data /var/www/azuriom
    ```
    (en remplaçant simplement `/var/www/azuriom` par l'emplacement du site et `www-data` par
    l'utilisateur du serveur web)

1. Se rendre sur `votre-site.fr/install.php` et suivre les étapes de l'installation.

### Installation Manuelle

> {warn} À moins de savoir ce que vous faites il est très fortement recommandé
d'utiliser l'installation automatique !

1. Cloner le [repo GitHub](https://github.com/Azuriom/Azuriom) ou [télécharger une release](https://github.com/Azuriom/Azuriom/releases).
    ```
    git clone https://github.com/Azuriom/Azuriom.git
    ```

1. Copier le fichier `.env.example` vers `.env` et indiquer les informations de connexion à la base de données.

1. Mettre les droits d'écriture aux dossiers `storage/`, `bootstrap/cache/`, `resources/themes` et `plugins`:
    ```
    chmod -R 755 storage bootstrap/cache resources/themes plugins
    ```
    
    Si l'utilisateur actuel n'est pas le même que l'utilisateur du serveur web,
    il peut être nécessaire de changer le propriétaire des fichiers :
    ```
    chown -R www-data:www-data /var/www/azuriom
    ```
    (en remplaçant `var/www/azuriom` par l'emplacement du site et `www-data` par
    l'utilisateur du serveur web)

1. Installer les dépendances avec [Composer](https://getcomposer.org/):
    ```
    composer install
    ```

    * Si vous utilisez Azuriom en production, vous pouvez optimiser l'autoloader de Composer en utilisant cette commande: 
        ```
        composer install --optimize-autoloader --no-dev
        ```

1. Générer la clé secrète :
    ```
    php artisan key:generate
    ```

1. Mettre en place la base de données :
    ```
    php artisan migrate --seed
    ```

1. Créer le symlink pour le stockage
    ```
    php artisan storage:link
    ```

1. Créer un compte administrateur _(Optionnel, mais très utile)_ :
    ```
    php artisan user:create --admin
    ```

1. Configurer votre serveur web pour qu'il pointe vers le dossier `public/` _(Optionnel, mais très fortement recommandé)_

1. Mettre en place le scheduler _(Optionnel, mais recommandé)_:

    Vous devez configurer votre serveur pour que la commande `php artisan schedule:run` soit exécutée toutes les minutes, par exemple en ajoutant une entrée Cron:
    ```
    * * * * * cd /var/www/azuriom && php artisan schedule:run >> /dev/null 2>&1
     ```
    Cela peut être fait en modifiant la configuration de crontab avec la commande `crontab -e`
    (n'oubliez pas de remplacer `/var/www/azuriom` par l'emplacement du site).

## Environnement de développement

Si jamais Azuriom est installé en local pour du développement (par exemple pour
faire des thèmes/plugins), il est très fortement recommandé d'activer le debug
afin de simplifier le développement.
Cela peut se faire très simplement en modifiant ces 2 lignes dans le `.env` à la
racine du site :
```
APP_ENV=local
APP_DEBUG=true
```

Il est également recommandé de désactiver RocketBooster lors du développement.

> {warn} Si jamais un est accessible publiquement, il est très fortement
déconseillé d'activer le debug et de configurer l'environnement de développement.

## Configuration du serveur web

### Apache 2

Si vous utilisez Apache 2, il peut être nécessaire d'activer la réécriture d'url.

Pour cela, commencez par activer le mod "rewrite" avec la commande suivante :
```
a2enmod rewrite
```

Ensuite vous pouverz configurer le site pour autoriser la réécriture d'url.
Il faut simplement modifier le fichier `/etc/apache2/sites-available/000-default.conf`
et y ajouter les lignes suivantes entre les balises `<VirtualHost>` (en remplaçant
`var/www/azuriom` par l'emplacement du site) :
```
<Directory "/var/www/azuriom">
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Pour finir, il faut juste redémarrer Apache 2:
```
service apache2 restart
```

### Nginx

Si vous déployez Azuriom sur un serveur qui utilise Nginx, vous pouvez utiliser
la configuration Nginx suivante :

```
server {
    listen 80;
    server_name example.com;
    root /var/www/azuriom/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Cette config doit être placée dans un site dans `site-available` et non dans le
`nginx.conf`.

Pensez également à remplacer `example.com` par votre domaine, `/var/www/azuriom`
par l'emplacement du site et `php7.4` par votre version de PHP.
