# Połączenie z serwerem

## Wprowadzenie

Niektóre funkcje, takie jak wyświetlanie graczy online lub
wykonanie polecenia wymaga połączenia twojego serwera ze twoją stroną. 
W Azuriomie możesz to zrobić na trzy różne sposoby:

* By Ping - **Jest to najprostsze, ale także najbardziej ograniczone rozwiązanie**, które pozwala uzyskać
tylko informacje o graczach na serwerze. _(ale nie pozwala na wykonywanie poleceń)_

* By Rcon - **To rozwiązanie jest trochę przeciętne**, pozwala uzyskać informacje
o swoim serwerze i wykonywać polecenia.

* By Plugin - **Najbardziej optymalne rozwiązanie**, pozwoli ci mieć funkcjonalność Rcon
ale na bardziej zaawansowanym poziomie _(przykład: zaawansowany system statystyk)_

## Połączenie przez Ping

Aby móc połączyć serwer z witryną Azuriom za pomocą polecenia ping,
musisz tylko dodać nowy serwer z `Ping` jako typem łącza
i wypełnić wymagane informacje _(domyślny port Minecraft-`25565`)_.

## Połączenie przez Rcon

Aby móc połączyć serwer z witryną Azuriom za pośrednictwem Rcon,
musisz wykonać następujące czynności:

1. Przejdź do pliku `server.properties` swojego serwera

2. Skonfiguruj plik w następujący sposób:
    * Ustaw opcję `enable-rcon` na `true`.
    * Ustaw `rcon.password` na`twoje-hasło`.
    * Zainstaluj `rcon.port` jako `twój-port` _(domyślnie `25575`)_
    * Wykonaj kopię zapasową i uruchom ponownie serwer
   
3. Przejdź do swojej witryny i dodaj nowy serwer z linkiem typu `Rcon`
i wprowadź wymagane informacje. _(domyślny port Rcon-`25575`)_.

## Połączenie przez Plugin 

### Co to jest AzLink ?

AzLink to wtyczka do łączenia witryn z serwerami, opracowana specjalnie dla Azuriom,
dzięki czemu możesz szybko i łatwo połączyć swój serwer ze stroną.

AzLink obecnie obsługuje Bukkit, BungeeCord, Sponge i Velocity w jednej wtyczce. Dodano starszą wersję
dla Bukkit 1.7.10.

### Instalacja

1. Pobierz AzLink pod linkiem [nasza strona](https://azuriom.com/azlink)

2. Zainstaluj go w folderze `plugin/` i zrestartuj serwer.

3. Przejdź do swojej witryny i dodaj nowy serwer z linkiem typu "AzLink",
postępuj zgodnie z instrukcjami podanymi na łączu i wprowadź niezbędne informacje.
