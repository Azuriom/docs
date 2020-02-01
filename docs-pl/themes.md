# Motywy

## Wprowadzenie

Motyw pozwala w pełni dostosować wygląd strony za pomocą Azuriom.

Aby zainstalować motyw, po prostu umieść go w folderze `resources / themes /` w katalogu głównym witryny.

## Utwórz motyw

### Struktura motywu

```
themes/ <-- Folder zawierający wszystkie zainstalowane motywy
| example/ <-- Slug Twojego motywu (nazwa twojego motywu małymi literami)
| | theme.json <-- Główny plik Twojego motywu zawierający różne informacje
| |  assets/  <-- Folder z zasobami Twojego motywu (css, js, images, svg itp.)
| | views/ <-- Folder zawierający widoki twojego motywu.
| | config/
| | | config.json
| | | config.blade.php
| | | rules.php
```

### Plik theme.json

Wszystkie motywy powinny mieć plik `theme.json` w katalogu głównym, 
czyli jedyny niezbędny element kompozycji, który wygląda następująco:
```json
{
    "name": "Example",
    "version": "1.0.0",
    "description": "A great theme.",
    "url": "https://azuriom.com",
    "authors": [
        "Azuriom"
    ]
}
```

> {info} Aby utworzyć motyw, możesz także użyć następującego polecenia, 
które automatycznie utworzy katalog motywów i plik `theme.json`:
```
php artisan theme:create <theme name>
```

### Widoki

Widoki są sercem motywu; są to pliki zawartości HTML 
motywu dla różnych części witryny.

W Azuriom przy użyciu widoków [Laravel] (https://laravel.com/) widoki można tworzyć za pomocą Blade.
Jeśli nie masz jeszcze Blade'a, zdecydowanie zalecamy przeczytanie informacji na [jego dokumentacji] (https://laravel.com/docs/6.x/blade).

> {uwaga} Zdecydowanie zaleca się NIE używać składni PHP
podczas pracy z Blade, ponieważ to tylko skomplikuje dalszą pracę.

Jeśli chodzi o CSS, zaleca się korzystanie ze standardowej struktury CMS, którą jest [Bootstrap 4] (https://getbootstrap.com).
Ułatwi to implementację motywu i będzie kompatybilny z nowymi wtyczkami, 
więc nie musisz dokonywać ciągłych aktualizacji.
Ale możesz oczywiście użyć wybranego przez siebie szkieletu CSS.

Jeśli chodzi o JavaScript, jQuery jest opcjonalny, tylko [Axios] (https://github.com/axios/axios) jest potrzebny jako zależność.

#### Układ

Układ tworzy strukturę stron motywu. Zawiera
metadane, zasoby tematyczne, nagłówki, stopki itp.

Aby wyświetlić zawartość na bieżącej stronie, użyj
`@yield ('content')`, i aby wyświetlić tytuł bieżącej strony `@yield ('title')`.

Możesz także zintegrować różne elementy z
`@include ('<nazwa widoku>')`, np. `@include ('element.navbar')`, aby dodać element paska nawigacyjnego.

### Metody

#### Zasoby

Aby uzyskać link do zasobu w motywie, użyj funkcji
`theme_asset`: 
```html
<link rel="stylesheet" href="{{ theme_asset('css/style.css') }}">
```

#### Tłumaczenia

W razie potrzeby możesz pobrać tłumaczenie dla motywu.

Aby to zrobić, utwórz plik `messages.php` w katalogu` lang / <język> `(przykład:` lang / en`).
swojego motywu. Następnie możesz wyświetlić tłumaczenie za pomocą polecenia
trans: `{{trans ('theme :: messages.hello')}}` lub przez `@ lang`:
`@lang ('theme :: messages.hello')`.
Możesz także użyć `trans_choice`
dla tłumaczeń z liczbami oraz `trans_bool` dla tłumaczeń ze zmiennymi binarnymi (wraca do polskiego` Tak`
/ `Nie`).

Aby uzyskać bardziej szczegółowe informacje o tłumaczeniach, możesz skorzystać z linku
[Laravel documentation](https://laravel.com/docs/6.x/localization).

#### Użytkownik

Bieżącego użytkownika można obliczyć za pomocą funkcji `auth () -> user ()`.
Aby uzyskać więcej informacji na temat uwierzytelniania, kliknij tutaj.
[Laravel documentation](https://laravel.com/docs/6.x/authentication).

#### Przydatne funkcje

Możesz uzyskać określoną liczbę parametrów ze strony internetowej, korzystając z przedstawionych funkcji:

|    Funkcja        |              Opis                               |
| ----------------  | ------------------------------------------------|
| `site_name ()`    | Pobiera nazwę strony                            |
| `site_logo ()`    | Pozwala uzyskać link do logo strony             |
| `favicon ()`      | Pozwala uzyskać link do ikony strony            |
| `format_date ()`  | Wyświetla datę sformatowaną w bieżącym języku. Ta funkcja przyjmuje przypadek `Carbon \ Carbon` jako parametr |
| `money_name ()`   | Zwraca nazwę waluty witryny                     |
| `format_money ()` | Zwraca kwotę w formacie waluty witryny          |

### Ustawienie motywu

Możesz dodać konfigurację do motywu. Aby to zrobić, utwórz motywy w katalogu głównym:
* Plik `config / config.blade.php` zawierający formularz konfiguracji.
* Plik `config / rules.php` z różnymi regułami sprawdzania poprawności konfiguracji motywu.
* Plik `config.json` z konfiguracją motywu i oryginalnymi wartościami.

##### Przykład:

config.blade.php
```html
<form action="{{ route('admin.themes.update', $theme) }}" method="POST">
    @csrf

    <div class="form-group">
        <label for="discordInput">{{ trans('theme::carbon.config.discord') }}</label>
        <input type="text" class="form-control @error('discord-id') is-invalid @enderror" id="discordInput" name="discord-id" required value="{{ old('discord-id', config('theme.discord-id')) }}">

        @error('discord-id')
        <span class="invalid-feedback" role="alert"><strong>{{ $message }}</strong></span>
        @enderror
    </div>

    <button type="submit" class="btn btn-primary"><i class="fas fa-save"></i> {{ trans('messages.actions.save') }}</button>
</form>
```

config.json
```json
{
    "discord-id": "625774284823986183."
}
```
