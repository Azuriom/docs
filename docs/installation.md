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

And it's also highly recommended to have a **MySQL/MariaDB or PostgreSQL database**.

## Installation
Azuriom can be installed in two different ways:

- Automatically through the installer _(recommended for most users)_. 
- Manually via [Composer](https://getcomposer.org/) _(recommended for experienced users or those wishing to contribute to Azuriom)_.

### Automatic Installation

1. Download the latest version of Azuriom on [our website](https://azuriom.com/download).

2. Extract the archive at the root of your website.

3. Go to `your-website.com/install.php` and follow the steps of installation.

### Manual Installation

1. Clone the GitHub repository (https://github.com/Azuriom/Azuriom.git) or [download a release](https://github.com/Azuriom/Azuriom/releases).

2. Copy the `.env.example` file to `.env` and specify the database connection information.

3. Set write/read permissions to the `storage/`, `bootstrap/cache/`, `resources/themes` and `plugins` folders:
```
chmod -R 770 storage bootstrap/cache resources/themes plugins
```

4. Generate the secret key:
```
php artisan key:generate
```

5. Install the dependencies with Composer:
```
composer install
```

  *If you are using Azuriom in production, you can optimize the autoloader of Composer using this command:
```
composer install --optimize-autoloader --no-dev
```

6. Setup the database:
```
php artisan migrate --seed
```

7. Create an admin account _(Optional but recommended)_:
```
php artisan artisan user:create --admin
```

8. Configure your web server to point to the `public/` folder _(Optional but highly recommended)_

9. Setup the scheduler _(Optional but recommended)_:

You need to configure your web server to run the `php artisan schedule:run` command every minute, for example by adding a Cron entry:
 ```
* * * * * cd /path-to-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```
This can be done by modifying the crontab configuration with the `crontab -e` command.
