# Instalacja

## Wymagania

Do pracy Azuriom wymaga tylko **serwera WWW z PHP** posiadającego **100 MB**
wolnego miejsca na dysku, a także:

 - PHP 7.2 lub później
 - Moduł przepisywania adresów URL
 - Uprawnienia do odczytu/edycji `storage/` i `bootstrap/cache/`.
 - Rozszerzenie BCMath PHP 
 - Rozszerzenie Ctype PHP 
 - Rozszerzenie JSON PHP 
 - Rozszerzenie Mbstring PHP 
 - Rozszerzenie OpenSSL PHP 
 - Rozszerzenie PDO PHP 
 - Rozszerzenie Tokenizer PHP 
 - Rozszerzenie XML PHP 
 - Rozszerzenie cURL PHP 
 - Rozszerzenie GD2 PHP 
 - Rozszerzenie Zip PHP 

Zaleca się także posiadanie **MySQL/MariaDB lub bazy danych PostgreSQL**.

## Instalacja
AAzuriom można zainstalować na dwa sposoby:

- Automatycznie poprzez plik instalacyjny _(zalecane dla większości użytkowników)_.
- Ręcznie używając [Composer](https://getcomposer.org/) _(zalecany dla profesjonalnych użytkowników i dla tych, którzy chcą pomóc w rozwoju Azuriom)_.

### Automatyczna instalacja

1. Pobierz najnowszą wersję Azuriom z linku [nasza strona](https://azuriom.com/download).

2. Wypakuj archiwum do głównego źródła swojej witryny.

3. Przejdź do `twojastrona.com/install.php` i postępuj zgodnie z dalszymi krokami instalacji.

### Instalacja ręczna

1. Skopiuj repozytorium GitHub (https://github.com/Azuriom/Azuriom.git) lub [pobierz wersję](https://github.com/Azuriom/Azuriom/release).

2. Skopiuj plik `.env.example` do `.env` i wprowadź informacje o połączeniu z bazą danych.

3. Ustaw uprawnienia do odczytu/edycji folderów `storage/`, `bootstrap/cache/`, `resources/themes` i `plugins`:
```
chmod -R 770 storage bootstrap/cache resources/themes plugins
```

4. Wygeneruj tajny kod:
```
php artisan key:generate
```

5. Zainstaluj zależności za pomocą Composer:
```
composer install
```

  *Jeśli używasz Azuriom do pracy, możesz zoptymalizować autoloader Composer za pomocą tego polecenia:
```
composer install --optimize-autoloader --no-dev
```

6. Skonfiguruj bazę danych:
```
php artisan migrate --seed
```
                                                
7. Utwórz konto administratora _(opcjonalnie, ale bardzo przydatne)_:
```
php artisan artisan user:create --admin
```

8. Skonfiguruj serwer WWW tak, aby wskazywał folder `public/` _(opcjonalny, ale zalecany)_

9. Skonfiguruj harmonogram _(opcjonalny, ale zalecany)_:

Konieczne jest skonfigurowanie serwera WWW, aby polecenie `php artisan schedule: run` działało co minutę, na przykład dodając wprowadzenie do Crona:
 ```
* * * * * cd /path-to-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```
Można to zrobić, modyfikując konfigurację crontab za pomocą polecenia `crontab -e`.

## Konfiguracja serwera WWW

### Apache 2

Jeśli używasz Apache 2, może być konieczne włączenie modułu przepisywania adresów URL.

Aby to zrobić, zmodyfikuj plik `/etc/apache2/sites-available/000-default.conf` i dodaj następujące wiersze 
między znacznikami `<VirtualHost>` (zastępując `var/www/azuriom` adresem strony):
```xml
<Directory "/var/www/azuriom">
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Następnie musisz ponownie uruchomić Apache 2:
```
service apache2 restart
```

### Nginx

Jeśli hostujesz Azuriom na serwerze, który używa Nginx, możesz użyć następującej konfiguracji:

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
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
```

Pamiętaj tylko, aby zastąpić `example.com` swoją domeną, a `/var/www/azuriom` adresem swojej witryny.
