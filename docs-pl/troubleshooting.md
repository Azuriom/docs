# Rozwiązanie błędu

Niektóre błędy mogą wystąpić na pewnym etapie, niekoniecznie związane z CMS.
Oto najczęstsze błędy i sposoby ich rozwiązywania!

## Typowe błędy

### Strona główna otwiera się, ale inne podają Błąd 404

Przepisywanie adresów URL nie jest aktywowane, wystarczy je aktywować (patrz następne pytanie).

### Przepisywanie adresów URL nie jest dostępne w Apache 2
ВMusisz zmienić plik `/etc/apache2/sites-available/000-default.conf` i dodaj następujące między tagami `<VirtualHost>`:
```xml
<Directory "/var/www/html">
  AllowOverride All
</Directory>
```

Następnie uruchom ponownie Apache2:
```
service apache2 restart
```

### Błąd 500 podczas rejestracji

Nawet jeśli konto zostało utworzone poprawnie, ten problem może wystąpić, 
jeśli konfiguracja wysyłania wiadomości e-mail jest nieprawidłowa. 
Sprawdź swoje ustawienia e-mail w panelu administracyjnym swojej witryny.

### Plik nie ładuje się (w przypadku obrazu)

Może się to zdarzyć, jeśli prześlesz zdjęcie o wadze przekraczającej maksymalne dopuszczalne PHP (domyślnie 2 MB).

Możesz zmienić maksymalne wartości, ładując konfigurację 
PHP (w `php.ini`) i zmieniając następujące wartości:
```
upload_max_filesize = 10M
post_max_size = 10M
```
