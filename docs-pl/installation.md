# Instalacja

## Wymagania

Do pracy Azuriom wymaga tylko ** serwera WWW z PHP ** posiadającego ** 100 MB **
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

Zaleca się także posiadanie ** MySQL / MariaDB lub bazy danych PostgreSQL **.

## Instalacja
AAzuriom można zainstalować na dwa sposoby:

- Automatycznie poprzez plik instalacyjny_ (zalecane dla większości użytkowników) _.
- Ręcznie używając [Kompozytora] (https://getcomposer.org/) _ (zalecany dla profesjonalnych użytkowników i dla tych, którzy chcą pomóc w rozwoju Azuriom) _.

### Automatyczna instalacja

1. Pobierz najnowszą wersję Azuriom z linku [nasza strona] (https://azuriom.com/download).

2. Wypakuj archiwum do głównego źródła swojej witryny.

3. Przejdź do `twojastrona.com / install.php` i postępuj zgodnie z dalszymi krokami instalacji.

### Instalacja ręczna

1. Skopiuj repozytorium GitHub (https://github.com/Azuriom/Azuriom.git) lub [pobierz wersję] (https://github.com/Azuriom/Azuriom/release).

2. Skopiuj plik `.env.example` do` .env` i wprowadź informacje o połączeniu z bazą danych.

3. Ustaw uprawnienia do odczytu/edycji folderów `storage /`, `bootstrap / cache /`, `resources / themes` i` plugins`:
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

7. Skonfiguruj serwer WWW tak, aby wskazywał folder `public /` _ (opcjonalny, ale zalecany) _

8. Skonfiguruj harmonogram _ (opcjonalny, ale zalecany) _:

Konieczne jest skonfigurowanie serwera WWW, aby polecenie `php artisan schedule: run` działało co minutę, na przykład dodając wprowadzenie do Crona:
 ```
* * * * * cd /path-to-azuriom && php artisan schedule:run >> /dev/null 2>&1
 ```
Można to zrobić, modyfikując konfigurację crontab za pomocą polecenia `crontab -e`.
