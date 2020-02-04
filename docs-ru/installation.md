# Установка

## Требования

Для работы, Azuriom необходимо лишь наличие **веб-сервера с PHP**, имеющим **100 МБ**
свободного дискового пространства, а также следующее:

 - PHP 7.2 или более поздней версии
 - Модуль перезаписи URL 
 - Разрешения на чтение/редактирование `storage/` и `bootstrap/cache/`.
 - Расширение BCMath PHP 
 - Расширение Ctype PHP 
 - Расширение JSON PHP 
 - Расширение Mbstring PHP 
 - Расширение OpenSSL PHP 
 - Расширение PDO PHP 
 - Расширение Tokenizer PHP 
 - Расширение XML PHP 
 - Расширение cURL PHP 
 - Расширение GD2 PHP 
 - Расширение Zip PHP 

Также рекомендуется иметь **MySQL/MariaDB или базу данных PostgreSQL**.

## Установка
Azuriom может быть установлен двумя способами:

- Автоматически через установочный файл_(рекомендуется дл большинства пользователей)_. 
- Вручную с помощью [Composer](https://getcomposer.org/) _(рекомендуется для профессиональных пользователей и для тех, кто желает помочь развитию Azuriom)_.

### Автоматическая установка

1. Скачайте последнюю версию Azuriom по ссылке [our website](https://azuriom.com/download).

2. Извлеките архив в корневой источник вашего сайта.

3. Перейдите на `your-website.com/install.php` и следуйте дальнейшим шагам установки.

### Самостятельная установка

1. Скопируйте репозиторий GitHub (https://github.com/Azuriom/Azuriom.git) или [скачать релиз](https://github.com/Azuriom/Azuriom/release).

2. Скопируйте файл `.env.example` в `.env` и укажите информацию о соединении с базой данных.

3. Задайте разрешения на чтение/редактирование папок `storage/`, `bootstrap/cache/`, `resources/themes` и `plugins`:
```
chmod -R 770 storage bootstrap/cache resources/themes plugins
```

4. Сгенерируйте секретный код:
```
php artisan key:generate
```

5. Установите зависимости с помощью Composer:
```
composer install
```

  *Если Вы используете Azuriom для работы, Вы можете оптимизировать автозагрузчик Composer с помощью этой команды:
```
composer install --optimize-autoloader --no-dev
```

6. Настройте базу данных:
```
php artisan migrate --seed
```

7. Настройте конфигурацию веб-сервера так, чтобы он указывал на папку `public/`  _(не является обязательным, но рекомендуется)_

8. Настройте планировщик _(не является обязательным, но рекомендуется)_:

Необходимо настройть конфигурацию веб-сервера таким образом, чтобы команда `php artisan schedule:run` запускалась каждую минуту, например, добавив Cron-введение:
 ```
* * * * * cd /path-to-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```
Это можеть быть выполнено путем модификации crontab-конфигурации командой `crontab -e`.

## Конфигурация веб-сервера

### Apache 2

Если Вы используете Apache 2, может потребоваться включение модуля перезаписи URL.

Для этого измените файл `/ etc / apache2 / sites-available / 000-default.conf`
и добавьте следующие строки между тегами `<VirtualHost>` (заменив
`var/www/azuriom` адресом сайта):
```xml
<Directory "/var/www/azuriom">
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Далее необходимо перезапустить Apache 2:
```
service apache2 restart
```

### Nginx

Если Вы размещаете Azuriom на сервере, который использует Nginx, то можно воспользоваться следующей конфигурацией:

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

Просто не забудьте заменить "example.com" на ваш домен и `/ var / www / azuriom`
на адрес сайта.
