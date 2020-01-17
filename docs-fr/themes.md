# Thèmes

## Introduction

Un thème permet de personnaliser entièrement l'apparence d'un site utilisant Azuriom.

## Structuration d'un thème

Pour installer un thème il suffit de mettre celui-là dans le dossier `themes/` à
la racine de votre site.

```
themes/  <-- Dossier contenant tous les thèmes installés
|  example/  <-- Slug de votre thème (nom de votre thème en minuscule)
|  |  theme.json  <-- Le fichier principal de votre thème contenant les différentes informations
|  |  views/  <-- Le dossier contenant les vues de votre thème
|  |  config/
|  |  |  config.json
|  |  |  config.blade.php
|  |  |  rules.php
```

## Création d'un thème

### Le fichier theme.json

Tous les thèmes ont besoin d'avoir un fichier `theme.json` à leur racine, c'est
le seul élément indispensable pour un thème et il ressemble à ça:
```json
{
    "name": "Exemple",
    "version": "1.0.0",
    "description": "Un super thème",
    "url" : "https://azuriom.com",
    "authors": [
        "Azuriom"
    ]
}
```

> {info} Pour créer un thème vous pouvez également utiliser la commande suivante qui va
générer automatiquement le dossier du thème ainsi que le fichier `theme.json`:
```
php artisan theme:create <nom du thème>
```

### Les vues

Les vues sont le coeur de votre thème, ce sont les fichiers content l'HTML de
votre thème pour les différentes parties du site.
Azuriom utilisant [Laravel](https://laravel.com/), les vues peuvent être faites en utilisant le moteur
de template Blade. Si vous ne maitrisez pas Blade il est très vivement recommandé
de lire [sa documentation](https://laravel.com/docs/6.x/blade), d'autant plus que celle-ci est assez courte.

> {warn} Il est très vivement recommandé de ne PAS utiliser la syntaxe PHP
traditionnelle lorsque vous travaillez avec Blade, en effet celle-ci n'apporte
aucun avantage et seulement des inconvénients.

Côté CSS, il est recommandé d'utiliser framework par défaut du cms qui est [Bootstrap 4](https://getbootstrap.com/), 
cela permettra de réaliser plus facilement votre thème et sera compatible avec les nouveaux plugins 
ce qui vous évitera de faire des mises à jour constamment.
Mais vous pouvez bien évidemment utiliser le framework CSS de votre choix.

Côté Javascript, jQuery n'est pas obligatoire, seul [Axios](https://github.com/axios/axios) est nécessaire comme dépendance!

#### Le layout

Le layout est la structure de l'ensemble des pages de votre thème. Il contient
en effet les metas, assets de votre thème, header, footer etc.

Pour afficher le contenu de la page actuelle, vous pouvez utiliser
`@yield('content')`, et pour afficher le titre de la page actuelle vous pouvez
utiliser `@yield('title')`

Également vous pouvez intégrer différents éléments avec
`@include('<nom de la vue>')`, par exemple `@include('element.navbar')` pour
inclure la navbar.

### Les méthodes

#### Assets

Pour avoir le lien vers un asset de votre thème vous pouvez utiliser la fonction
`theme_asset`: 
```html
<link rel="stylesheet" href="{{ theme_asset('css/style.css') }}">
```

#### Les traductions

Un thème peut, si il en a besoin, charger des traductions.

Pour cela il suffit de créer un fichier `messages.php` dans le dossier `lang/<lang>` (ex: `lang/fr`)
de votre thème, vous pouvez ensuite affichez une traduction via la fonction
trans: `{{ trans('theme::messages.hello') }}` ou via la directive `@lang`: 
`@lang('theme::messages.hello')`.
Vous pouvez également utiliser `trans_choice` pour une traduction comportant des
nombres, et `trans_bool` pour traduire un boolean (retournera en français `Oui`
/ `Non`).

Pour plus de détails sur les traductions, vous pouvez vous référer à la
[documentation de Laravel](https://laravel.com/docs/6.x/localization).

#### L'utilisateur

L'utilisateur actuel peut être récupéré grâce à la fonction `auth()->user()`.
Pour plus de détails sur l'authentification, vous pouvez vous référer à la
[documentation de Laravel](https://laravel.com/docs/6.x/authentication).

#### Fonctions utiles

Vous pouvez récupérer un certain nombre de paramètres du site via des fonctions
dédiées:

|    Fonction      |             Description                |
| ---------------- | -------------------------------------- |
| `site_name()`    | Permet de récupérer le nom du site     |
| `site_logo()`    | Permet d'avoir le lien du logo du site |
| `favicon()`      | Permet d'avoir le lien de la favicon   |
| `format_date()`  | Affiche une date formatée avec la langue actuelle. Cette fonction prend en paramètre une instance de `Carbon\Carbon` |
| `money_name()`   | Retourne le nom de la monnaie du site  |
| `format_money()` | Retourne un montant formaté avec la monnaie du site |

### Configuration du thème

Vous pouvez ajouter une configuration dans votre thème, pour cela il vous suffit
de créer à la racine de votre thème:
* Une vue `config/config.blade.php` contenant le form pour la configuration
* Un fichier `config/rules.php` contenant les différentes règles de validation pour
la configuration de votre thème.
* Un fichier `config.json` ou sera stocké la configuration du thème, et contenant les valeurs par défaut 

##### Exemple:

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
    "discord-id": "625774284823986183"
}
```
