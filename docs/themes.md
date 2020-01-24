# Themes

## Introduction

A theme allows you to fully customize the look and feel of a website using Azuriom.

To install a theme, just put it in the `resources/themes/` folder at
the root of your website.

## Creating a theme

### Structuring a theme

```
themes/ <-- Folder containing all installed themes
| example/ <-- Slug of your theme (name of your theme in lower case)
| | theme.json <-- The main file of your theme containing the various informations
| |  assets/  <-- The folder containing the assets of your theme (css, js, images, svg, etc)
| | views/ <-- The folder containing the views of your theme.
| | config/
| | | config.json
| | | config.blade.php
| | | rules.php
```

### The theme.json file

All themes need to have a `theme.json` file at their root, that is
the only essential element for a theme and it looks like this:
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

> {info} To create a theme you can also use the following command that will
automatically generate the theme directory and the `theme.json` file:
```
php artisan theme:create <theme name>
```

### The views

The views are the heart of a theme, they are the HTML content files of
a theme for the different parts of the website.

Azuriom using [Laravel](https://laravel.com/), views can be made using the
of template Blade. If you don't master Blade it is highly recommended
to read [his documentation](https://laravel.com/docs/6.x/blade), especially since it is quite short.

> {warn} It is highly recommended NOT to use PHP syntax.
when you work with Blade, because Blade does not bring you the traditional
no advantages and only disadvantages.

On the CSS side, it is recommended to use the default framework of the cms which is [Bootstrap 4](https://getbootstrap.com), 
this will make it easier to realize a theme and will be compatible with the new plugins. 
so you don't have to make constant updates.
But you can of course use the CSS framework of your choice.

On the Javascript side, jQuery is not mandatory, only [Axios](https://github.com/axios/axios) is necessary as a dependency!

#### The layout

The layout is the structure of all the pages of a theme. It contains
indeed the metas, assets of a theme, header, footer etc..

To display the content of the current page you can use
`@yield('content')`, and to display the title of the current page you can
use `@yield('title')`.

You can also integrate different elements with
`@include('<name of the view>')`, for example `@include('element.navbar')` for
include the navbar.

### The methods

#### Assets

To have the link to an asset in a theme you can use the function
`theme_asset`: 
```html
<link rel="stylesheet" href="{{ theme_asset('css/style.css') }}">
```

#### The translations

A theme can, if it needs it, load translations.

To do so, just create a `messages.php` file in the `lang/<language>` directory (ex: `lang/en`).
of a theme, you can then display a translation via the
trans: `{{ trans('theme::messages.hello') }}` or via the `@lang` directive: 
`@lang('theme::messages.hello')`.
You can also use `trans_choice` for a translation with
numbers, and `trans_bool` to translate a boolean (will return in English `Yes`).
/`No`.

For more details on translations, you can refer to the
[Laravel documentation](https://laravel.com/docs/6.x/localization).

#### The user

The current user can be retrieved using the `auth()->user()` function.
For more details on authentication, you can refer to the
[Laravel documentation](https://laravel.com/docs/6.x/authentication).

#### Useful functions

You can retrieve a certain number of parameters from the website via the functions
dedicated:

|    Fonction      |               Description                 |
| ---------------- | ----------------------------------------- |
| `site_name()`    | Retrieves the site name                   |
| `site_logo()`    | Allows you to have the website logo link  |
| `favicon()`      | Allows you to have the favicon link       |
| `format_date()`  | Displays a date formatted with the current language. This function takes an instance of `Carbon\Carbon` as a parameter |
| `money_name()`   | Returns the name of the website's currency   |
| `format_money()` | Returns an amount formatted with the website currency |

### Theme setup

You can add a configuration in a theme, to do so you just have to
to create at the root of a theme:
* A `config/config.blade.php` view containing the form for the configuration.
* A `config/rules.php` file containing the different validation rules for
the configuration of a theme.
* A `config.json` file where the theme configuration will be stored, and containing the default values. 

##### Example:

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
