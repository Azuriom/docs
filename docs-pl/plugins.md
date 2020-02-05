# Wtyczki

## Wprowadzenie

Wtyczki dodają nowe, specjalne funkcje do Twojej witryny. Wiele
z których są dostępne na naszej stronie [Market](https://azuriom.com/market)
ale możesz także utworzyć własny, jeśli nie znalazłeś tego, czego potrzebujesz.

## Tworzenie wtyczek

Przed przystąpieniem do tworzenia wtyczki zalecamy zapoznanie się z informacjami pod linkiem
[Laravel documentation](https://laravel.com/docs/).

### Struktura wtyczki

```
plugins/  <-- Folder z zainstalowanymi różnymi wtyczkami
|  example/  <-- Slug Twojego wtyczki (nazwa wtyczki jest pisana małymi literami)
|  |  plugin.json  <-- Główny plik Twojego tematu zawierający różne informacje
|  |  assets/  <-- Folder z zasobami Twojej wtyczki (css, js, images, svg itd.)
|  |  database/
|  |  |  migrations/ <-- Folder zawierający migracje wtyczki
|  |  resources/
|  |  |  lang/  <-- Folder zawierający tłumaczenia Twojej wtyczki
|  |  |  views/  <-- Folder zawierający widoki wtyczki
|  |  routes/ <-- Folder zawierający różne trasy wtyczki
|  |  src/ <-- Folder zawierający źródła wtyczki
```

### Plik plugin.json

Plik `plugin.json` jest wymagany do pobrania wtyczki
i zawiera różne informacje o niej:
```json
{
    "name": "Example",
    "version": "1.0.0",
    "description": "A great plugin.",
    "url": "https://azuriom.com",
    "authors": [
        "Azuriom"
    ],
    "providers": [
        "\\Azuriom\\Plugin\\Example\\Providers\\ExampleServiceProvider",
        "\\Azuriom\\ Plugin\\Example\\Providers\\RouteServiceProvider"
    ]
}
```

> {info} Aby utworzyć wtyczkę, możesz użyć następującego polecenia
,które automatycznie utworzy folder wtyczki i wiele plików
domyślnie:
```
php artisan plugin:create <nazwa wtyczki>
```

### Trasy

Trasy umożliwiają powiązanie adresu URL z określoną akcją.

Są one przechowywane w katalogu `routes` w katalogu głównym wtyczki.

Więcej informacji na temat działania tras można znaleźć tutaj.
[Dokumentacja Laravel](https://laravel.com/docs/6.x/routing).

Przykład:
```php
Route::get('/support', 'SupportController@index')->name('index');
```

> {warn} Nie używaj tras z zakończeniami,
ponieważ nie są one kompatybilne z niektórymi optymalizacjami CMS.

### Widoki

Widoki są widoczną częścią wtyczki; są to pliki treści HTML
wtyczki do wyświetlenia strony.

W Azuriom przy użyciu widoków [Laravel](https://laravel.com/) widoki można tworzyć za pomocą Blade.
Jeśli nie masz jeszcze Blade'a, zdecydowanie zalecamy przeczytanie informacji na [jego dokumentacji](https://laravel.com/docs/6.x/blade).

> {warn} Zdecydowanie zaleca się NIE używać składni PHP
podczas pracy z Blade, ponieważ to tylko skomplikuje dalszą pracę.

Aby wyświetlić widok, użyj `view('<plugin slug>::<view name>')`,
lub, na przykład, `view('support::Tickets.index')`, aby wyświetlić widok `tickets.index`
wtyczki.

### Kontrolery

Kontrolery są centralną częścią wtyczki, znajdują się w folderze
`src/Controllers` w katalogu głównym wtyczki i są odpowiedzialni za
przekształcenie żądania w odpowiedź, która zostanie odesłana do użytkownika.

Aby uzyskać więcej informacji o działaniu kontrolerów, możesz skorzystać z łącza
[Dokumentacja Laravel](https://laravel.com/docs/6.x/controllers).

przykład:
```php
<?php

namespace Azuriom\Plugin\Support\Controllers;

use Azuriom\Http\Controllers\Controller;
use Azuriom\Plugin\Support\Models\Ticket;

class TicketController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        // We're picking up all the tickets
        $tickets = Ticket::all();

        // We're return a view, pass him the tickets...
        return view('support::tickets.index', [
            'tickets' => $tickets,
        ]);
    }
}
```

### Modele

Szablony są pozycją w tabeli bazy danych i umożliwiają interakcję z tą bazą danych.

Możesz także ustanowić różne relacje między modelami,
na przykład `ticket` może zawierać `user` i `category`, a także `comments`.

Możesz znaleźć więcej informacji o modelach (zwanych także Eloquent na Laravel) pod adresem
[Dokumentacja Laravel](https://laravel.com/docs/6.x/eloquent).

```php
<?php

namespace Azuriom\Plugin\Support\Models;

use Azuriom\Models\Traits\HasTablePrefix;
use Azuriom\Models\Traits\HasUser;
use Azuriom\Models\User;
use Illuminate\Database\Eloquent\Model;

class Ticket extends Model
{
    use HasTablePrefix;
    use HasUser;

    /**
     * The table prefix associated with the model.
     *
     * @var string
     */
    protected $prefix = 'support_';

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = [
        'subject', 'category_id',
    ];

    /**
     * The user key associated with this model.
     *
     * @var string
     */
    protected $userKey = 'author_id';

    /**
     * Get the user who created this ticket.
     */
    public function author()
    {
        return $this->belongsTo(User::class, 'author_id');
    }

    /**
     * Get the category of this ticket.
     */
    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    /**
     * Get the comments of this ticket.
     */
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}
```

### Dostawcy usług

Sercem wtyczki są dostawcy usług, którzy są wywoływani na etapie inicjalizacji
Laravel i pozwalają na zapisywanie różnych części wtyczki (widoki, tłumaczenia, oprogramowanie pośrednie itp.).

Dostawcy usług powinni zostać dodani do podsekcji `provider` części części`plugins.json`:
```json
{
    "providers": [
        "\\Azuriom\\Plugin\\Support\\Providers\\SupportServiceProvider"
    ]
}
```

Więcej informacji o usługodawcach można znaleźć tutaj.
[Laravel documentation](https://laravel.com/docs/6.x/providers).

```php
<?php

namespace Azuriom\Plugin\Support\Providers;

use Azuriom\Extensions\Plugin\BasePluginServiceProvider;

class SupportServiceProvider extends BasePluginServiceProvider
{
    /**
     * Register any plugin services.
     *
     * @return void
     */
    public function register()
    {
        $this->registerMiddlewares();

        //
    }

    /**
     * Bootstrap any plugin services.
     *
     * @return void
     */
    public function boot()
    {
        // $this->registerPolicies();

        $this->loadViews();

        $this->loadTranslations();

        $this->loadMigrations();

        $this->registerRouteDescriptions();

        $this->registerAdminNavigation();

        //
    }
}
```


### Migracje

Migracje umożliwiają tworzenie, modyfikowanie lub usuwanie tabel w bazie danych.
Można je znaleźć w folderze `database/migrations`.

Możesz znaleźć więcej informacji na temat migracji, klikając poniższy link.
[Laravel documentation](https://laravel.com/docs/6.x/migrations).

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateSupportTicketsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('support_tickets', function (Blueprint $table) {
            $table->increments('id');
            $table->string('subject');
            $table->unsignedInteger('author_id');
            $table->unsignedInteger('category_id');
            $table->timestamp('closed_at')->nullable();
            $table->timestamps();

            $table->foreign('author_id')->references('id')->on('users');
            $table->foreign('category_id')->references('id')->on('support_categories');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('support_tickets');
    }
}
```

### Tłumaczenia

Tłumaczenia pozwalają tłumaczyć wtyczki (świetnie!). Oni są
w katalogach `resources/lang` w katalogu głównym wtyczki, w
folder językowy (`en`, `fr` itp.)

Więcej informacji o tłumaczeniach można znaleźć tutaj
[Dokumentacja Laravela](https://laravel.com/docs/6.x/localization).

Aby otrzymać przelew, możesz użyć polecenia
`trans('<nazwa wtyczki>::<nazwa pliku>.<wiadomość>')`, na przykład
`trans('support::messages.tickets.home')` aby wyświetlić komunikat `tickets.home`,
w pliku `messages.php` wtyczki pomocniczej:
```php
<?php

return [
  'tickets' => [
    'home' => 'Your tickets',
  ],
];
```
