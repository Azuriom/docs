# Installation

## Requirements

To work, Azuriom simply requires a **web server with PHP** having at least **100 MB**
of disk space and the following requirements:

 - PHP 7.2 or newer
 - URL Rewrite
 - Write/Read permissions on `storage/` and `bootstrap/cache/`.
 - BCMath PHP Extension
 - Ctype PHP Extension
 - JSON PHP Extension
 - Mbstring PHP Extension
 - OpenSSL PHP Extension
 - PDO PHP Extension
 - Tokenizer PHP Extension
 - XML PHP Extension
 - cURL PHP Extension
 - GD2 PHP Extension
 - Zip PHP Extension

It's also highly recommended having a **MySQL/MariaDB or PostgreSQL database**.

### Requirements installations

If you are using a VPS or a dedicated server, it will probably be necessary to install
yourself Nginx, PHP and MySQL. This can be done with the following commands :
```
apt update && apt upgrade

apt install nginx zip curl

apt install lsb-release apt-transport-https ca-certificates
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
apt update
apt install php7.4 php7.4-fpm php7.4-mysql php7.4-pgsql php7.4-sqlite php7.4-bcmath php7.4-mbstring php7.4-xml php7.4-curl php7.4-zip php7.4-gd
```

> {info} If you prefer, you can also use this unofficial 
[auto-install script](https://github.com/AzuriomCommunity/Script-AutoInstall)
which will install all the prerequisites automatically.

## Installation
Azuriom can be installed in two different ways:

- Automatically through the installer _(recommended for most users)_. 
- Manually via [Composer](https://getcomposer.org/) _(recommended for experienced users, or those wishing to contribute to Azuriom)_.

### Automatic Installation

1. Download the latest version of Azuriom on [our website](https://azuriom.com/download).

1. Extract the archive at the root of your website.

1. Set write/read permissions to the `storage/`, `bootstrap/cache/`, `resources/themes` and `plugins` folders:
    ```
    chmod -R 755 storage bootstrap/cache resources/themes plugins
    ```
    
    If the current user is not the web server user, it may be
    necessary to change the owner of the files:
    ```
    chown -R www-data:www-data /var/www/azuriom
    ```
    (replace `var/www/azuriom` with the site location and `www-data`
    with the web server user)

1. Go to `your-website.com/` and follow the steps of installation.

### Manual Installation

> {warn} Warning, unless you know what you are doing it is highly recommended
using the automatic installation!

1. Clone the [GitHub repository](https://github.com/Azuriom/Azuriom) or [download a release](https://github.com/Azuriom/Azuriom/releases).
    ```
    git clone https://github.com/Azuriom/Azuriom.git
    ```

1. Copy the `.env.example` file to `.env` and specify the database connection information.

1. Set write/read permissions to the `storage/`, `bootstrap/cache/`, `resources/themes/` and `plugins/` folders:
    ```
    chmod -R 755 storage bootstrap/cache resources/themes plugins
    ```
    
    If the current user is not the web server user, it may be
    necessary to change the owner of the files:
    ```
    chown -R www-data:www-data /var/www/azuriom
    ```
    (replace `var/www/azuriom` with the site location and `www-data`
    with the web server user)

1. Install the dependencies with [Composer](https://getcomposer.org/):
    ```
    composer install
    ```
    
    *If you are using Azuriom in production, you can optimize the autoloader of Composer using this command:
        ```
        composer install --optimize-autoloader --no-dev
        ```

1. Generate the secret key:
    ```
    php artisan key:generate
    ```

1. Setup the database:
    ```
    php artisan migrate --seed
    ```

1. Create the storage symlink
    ```
    php artisan storage:link
    ```

1. Create an admin account _(Optional but recommended)_:
    ```
    php artisan user:create --admin
    ```

1. Configure your web server to point to the `public/` folder _(Optional but highly recommended)_

1. Setup the scheduler _(Optional but recommended)_:
    You need to configure your web server to run the `php artisan schedule:run` command every minute, for example by adding a Cron entry:
     ```
    * * * * * cd /var/www/azuriom && php artisan schedule:run >> /dev/null 2>&1
     ```
    This can be done by modifying the crontab configuration with the `crontab -e` command
    (just replace `var/www/azuriom` by the site location).

## Web server configuration

### Apache 2

If you are using Apache 2, it may be necessary to enable url rewriting.

To do this, first enable the "rewrite" mod:
```
a2enmod rewrite
```
 
Then you need to modify the `/etc/apache2/sites-available/000-default.conf` file
and add the following lines between the `<VirtualHost>` tags (replacing
`var/www/azuriom` by site location) to allow URL rewrite:
```
<Directory "/var/www/azuriom">
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Finally, you just need to restart Apache 2:
```
service apache2 restart
```

## Nginx

If you are deploying Azuriom on a server that uses Nginx, you can use
the following configuration:
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

Just remember to replace `example.com` with your domain, `/var/www/azuriom`
with the location of the site and `php7.4` with your PHP version.
