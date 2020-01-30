# Плагины

## Введение

Плагины добавляют новые, особенные черты вашему сайту. Многие 
из них доступны на нашем сайте [Market](https://azuriom.com/market)
, но Вы также можете создать свой, если не нашли то, что Вам нужно.

## Создание плагина

Перед тем как пристпить к созданию плагина, рекомендуем ознакомиться с информацией по ссылке
[Laravel documentation](https://laravel.com/docs/).

### Структурирование плагина

```
plugins/  <-- Папка с различными установленными плагинами
|  example/  <-- Slug вашего плагина (название вашего плагина в нижнем регистре)
|  |  plugin.json  <-- Основной файл вашей темы, содержащий различную информацию
|  |  assets/  <-- Папка с активами вашего плагина (css, js, images, svg и т.д.)
|  |  database/
|  |  | migrations/ <-- Папка, содержащая миграции вашего плагина
|  |  resources/
|  |  |  lang/  <-- Папка, содержащая переводы вашего плагина
|  |  |  views/  <-- Папка, содержащая виды вашего плагина
|  |  routes/ <-- Папка, содержащая различные маршруты вашего плагина
|  |  src/ <-- Папка, содержащая источники вашего плагина
```

### Файл plugin.json

Файл `plugin.json` необходим для загрузки плагина 
и содержит различную информацию о нем:
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

> {инфо} Для создания плагина Вы можете использовать следующую команду, которая будет
автоматически создавать папку плагина и множество файлов
по дефолту:
```
php artisan plugin:create <название плагина>
```

### Маршруты

Маршруты позволяют связать URL-адрес с определенным действием.

Они хранятся в каталоге маршрутов в корне плагина.

Для получения дополнительной информации о том, как работают маршруты, Вы можете прочитать здесь
[Laravel documentation](https://laravel.com/docs/6.x/routing).

Пример:
```php
Route::get('/support', 'SupportController@index')->name('index');
```

> {предупреждение} Пожалуйста, не следует использовать маршруты с завершениями,
поскольку они не совместимы с некоторыми оптимизациями CMS.

### Представления

Представления являются видимой частью плагина, они являются файлами содержимого HTML
плагина для отображения страницы.

В Azuriom, использующим представления [Laravel](https://laravel.com/), представления могуть быть сделаны при помощи Blade.
Если Вы еще не владеете Blade, то мы настоятельно рекомендуем ознакомиться с информацией по ссылке [its documentation](https://laravel.com/docs/6.x/blade).

> {предупреждение} Настоятельно рекомендуется НЕ ИСПОЛЬЗОВАТЬ синтаксис PHP
при работе с Blade, поскольку это лишь усложнит дальнейшую работу.

Для отображения представления, используйте `view('<plugin slug>::<название представления>')`,
или, как пример,`view('support::tickets.index')` для отображения представления `tickets.index` 
поддерживающего плагина.

### Контроллеры

Контроллеры являются центральной частью плагина, они расположены в папке
`src/Controllers` в корне плагина и отвечают за
преобразование запроса в ответ, который будет отправлен обратно пользователю.

Для получения дополнительной информации о работе контроллеров, Вы можете проконсультироваться по ссылке
[Laravel documentation](https://laravel.com/docs/6.x/controllers).

пример:
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

### Модели

Шаблоны представляют собой запись в таблице базы данных и позволяют Вам
взаимодействовать с этой базой.

Вы также можете установить различные отношения модели,
Например,`ticket` может иметь `user` и `category`, а также `comments`.

Вы можете найти больше информации о моделях (также называемых Eloquent на Laravel) по ссылке
[Laravel documentation](https://laravel.com/docs/6.x/eloquent).

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

### Сервис-провайдеры

Сервис-провайдеры - это сердце плагина, они вызываются на этапе инициализации
Laravel и позволяют сохранять различные части плагина (представления, переводы, промежуточное программное обеспечение и т.д.).

Сервис-провайдеры должны быть добавлены в подчасть `providers` части `plugins.json`:
```json
{
    "providers": [
        "\\Azuriom\\Plugin\\Support\\Providers\\SupportServiceProvider"
    ]
}
```

Больше информации о сервис-провайдерах Вы можете найти здесь
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


### Миграции

Миграции позволяют создавать, изменять или удалять таблицы в базе данных.
Их можно найти в папке `database/migrations`.

Вы можете найти больше информации о миграциях перейдя по ссылке ниже
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

### Переводы

Переводы позволяют переводить плагины (замечательно!). Они находятся
в`resources/lang`-дирекции в корне плагина, в 
языковой папке (`en`, `fr` и т.д.)

Вы можете найти больше информации о переводах по ссылке
[Laravel documentation](https://laravel.com/docs/6.x/localization).

Для получения перевода Вы можете использовать команду
`trans('<name of your plugin>::<filename>.<message>')`, например
`trans('support::messages.tickets.home')` для отображения сообщения `tickets.home`,
в файле `messages.php` вспомогательного плагина:
```php
<?php

return [
  'tickets' => [
    'home' => 'Your tickets',
  ],
];
```
