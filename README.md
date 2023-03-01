# Конвенция наименований в Laravel

Для упрощения взаимодействия в Laravel придуманы правила наименований. Правила нужны везде, а если правила не только знать но и соблюдать, то это даёт значительный прирост в скорости разработки.

Laravel наш помощник, но он будет помогать лишь в том случае, если вы работаете по его правилам! Применяя конвенцию наименований Laravel поможет автоматически находить нужные зависимые сущности!

По большей части конвенция наименований охватывает слой взаимодействия с базой данных.

Применение конвенции наименований в Laravel = простой, чистый и лаконичный код.

Погнали!
----

# Оглавление
- [Общие правила](#terms)
- [Таблицы базы данных](#tables)
  - [Связующие таблицы BelongsToMany](#tables-pivot)
  - [Поля](#tables-columns)
  - [Primary key](#tables-pk)
  - [Foreign keys](#tables-fk)
- [Models](#models)
  - [Константы](#models-constants)
  - [Свойства(Поля таблицы)](#models-attributes)
  - [Методы](#models-methods)
  - [Acccessors/Mutators](#models-am)
  - [Local scopes](#models-scopes)
  - [Отношения](#models-relations)
- [Миграции](#migrations)
- [Фабрики](#factories)
- [Политики/Policy](#policies)
- [Директории](#directories)
- [Controllers](#controllers)
- [Routes](#routes)
- [Blade](#blade)
- [Json response](#json)

<a name="terms" />

### Общие правила
1. Всегда в названиях используйте точное значение слова на английском. Не надо использовать транслит.<br> 
   *Если не знаете перевод слова или как оно правильно пишется - просто найдите его перевод. Заодно и словарный запас расширите)*
2. Избегайте ошибок в написании.
3. Старайтесь подбирать название так, чтобы сразу было понятно, что именно содержится в именовании.
4. Не сокращайте названия без острой необходимости.
   ```php
   // плохо
   $acc
   $conn
   $descr
   prepResp()
   regEvtHandler()
   updCom()
      
   // хорошо
   $account
   $connection
   $description
   prepareResponse()
   registerEventHandler()
   updateComment()
   ```



<a name="tables" />

### Таблицы базы данных


Для оформлении названий таблиц принято использовать имена во множественном числе с нотацией snake_case:

> snake_case — стиль написания составных слов, при котором несколько слов разделяются символом подчеркивания. Слова как бы ползут по строке, в результате получается дли_и_инное, как змея, название. В данном гайде snake_case всегда в нижнем регистре


**Примеры**:
- `users`
- `products`
- `order_products`

**Исключение**

Связующие таблицы для отношений `BelongsToMany`.

<a name="tables-pivot" />

#### Связующие таблицы BelongsToMany

Применяем snake_case + имена двух таблиц, которые будем связывать, в единственном числе. В качестве разделителя между именами таблиц используем символ нижнего подчёркивания `_`.

Сортируются сущности между разделителем в алфавитном порядке.

Разберём на примере: есть таблицы `users` и `tasks`, приводим обе таблицы в единственное число и разделяем через подчеркивание. Буква `t` в алфавите следует раньше `u`, поэтому результат связи двух таблиц будет - `task_user`.

Ну или пример посложнее: `order_products` и `properties`! Результат - `order_product_property`.

**Примеры**:
- `task_user`
- `event_place`
- `order_product_property`

<a name="tables-columns" />

#### Поля таблицы
Все просто - snake_case. И не забывайте общие правила о которых мы писали в одноименном разделе

**Примеры**:
- `created_at`
- `seo_title`

<a name="tables-pk" />

#### Primary Key
По умолчанию Laravel ожидает `id`.

<a name="tables-fk" />

#### Foreign Keys

snake_case в формате - имя таблица в единственном числе, далее подчеркивание `_` и далее Primary key ('id').
Пример - страна пользователя, где все страны в таблице `countries`, пользователь в таблице `users`
имеет foreign key для связи со страной - `country_id`, формат `{связанная_таблица_в_единственном_числе}_{primary_key_связанной_таблицы}`

**Примеры**:
- `country_id`
- `order_product_id`

<a name="models" />

### Models
PascalCase в единственном числе

> PascalCase — это стиль написания имен, при котором составные слова названия идентификатора (в том числе и первое слово) пишутся слитно, и каждое новое слово начинается с большой буквы. Пример: MyVar, MyBestProgramm, WorkArray. 
> Паскаль нотация используется для названий классов и публичных полей данных, а также именования процедур и функций. 

Если мы соблюдали конвенцию наименований для таблиц, то Laravel автоматически определит таблицу для модели. Как? Приведет вашу модель в snake_case в нижнем регистре добавит подчеркивания '_' и переведёт в множественное число. Получится имя таблицы.

В итоге `User` будет ссылаться к таблице `users`, `OrderProduct` к `order_products`

**Проблемы нарушения конвенции наименований**:
- Если название таблицы указано не в snake_case (или название таблицы отличается от названия модели) потребуется указать в модели свойство, помогающее определить таблицу: `protected $table = 'tableName';`
- тоже самое в случае с primary key - `protected $primaryKey = 'primaryKey';`
- и еще масса побочных указаний для отношений)

**Директория по умолчанию**

`app/Models`

**Примеры**:
- `User`
- `Product`
- `OrderProduct`

<a name="models-constants" />

#### Константы

При оформлении названия используем UPPER_CASE с разделителем подчеркивания.

**Примеры**:
```php
class Document
{
    const STATUS_ACTIVE = 1;
    const STATUS_DELETE = 2;
    const STATUS_ARCHIVE = 9;
}
```

<a name="models-attributes" />

#### Свойства (Поля таблицы)
Свойства модели ссылаются на поля таблицы и имеют точно такие же наименования в snake_case.

Становятся доступными за счет магических php методов. Обращаясь к свойствам модели, Laravel автоматически
соотнесет их с полями в таблице (если такие существуют).

**Примеры**:
- `$model->created_at`
- `$model->seo_title`

<a name="models-methods" />

#### Методы (не только в моделях, везде :))

При оформлении названия используем camelCase:

> camelCase - стиль написания составных слов, при котором несколько слов пишутся слитно без пробелов, при этом каждое слово внутри фразы пишется с прописной буквы. Начинается с маленькой буквы. И получается верблюд (camel) с его горбами.

**Примеры**:
- `getSomething()`

<a name="models-am" />

#### Accessors/Mutators
Наименования поля в таблице приводится к camelCase и изменяется в методе
[Отлично расписано в официальной документации](https://laravel.com/docs/eloquent-mutators#accessors-and-mutators)

<a name="models-scopes" />

#### Local Scopes
camelCase с префиксом scope
[Отлично расписано в официальной документации](https://laravel.com/docs/eloquent#local-scopes)

<a name="models-relations" />

#### Отношения
Все просто - camelCase для модели с которой мы формируем отношение. Если отношение к одной записи, тогда именование оформляем в единственном числе, а если несколько - то во множественном числе!

Рассмотрим примеры для модели `User` c разным типом отношений к модели `Country`.

**Примеры**:
- `belongsTo, hasOne, hasOneThrough, morphOne` = `country()`
- `belongsToMany, hasMany, hasManyThrough, morphMany, morphToMany` = `countries()`

**Проблемы нарушения конвенции наименований**:
- Может потребоваться указать `foreign_key` `return $this->hasOne(Phone::class, 'foreign_key');`
- Может потребоваться указать `foreign_key` и `local_key` `return $this->hasOne(Phone::class, 'foreign_key', 'local_key');`
- Может потребоваться указать и связующую таблицу и ключи `return $this->belongsToMany(Role::class, 'role_user', 'user_id', 'role_id');`

<a name="migrations" />

### Миграции
`php artisan make:migration migration_name`

Команда выше просто создаст пустой файл миграции, где методы up и down будут пустыми.

```php
public function up()
{
  //
}

public function down()
{
  //
}
```

Но мы также можем влиять на содержимое миграций, если будем придерживаться правил наименования, тем самым упрощая нам жизнь.

`php artisan make:migration create_users_table`

Мы дали понять, что хотим создать таблицу users - `create_{Имя_таблицы}_table`

Ключевое здесь `create_{Имя_таблицы}`, а вот table в конце можно пропустить

В итоге получили:

```php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->timestamps();
    });
}

public function down()
{
    Schema::dropIfExists('users');
}
```


`php artisan make:migration add_column_to_users_table`

В данном примере мы сообщаем Laravel о нашем желании модифицировать таблицу и ключевое здесь `to_{Имя_таблицы}`, а вот table в конце можно пропустить

```php
public function up()
{
    Schema::table('users', function (Blueprint $table) {
        //
    });
}

public function down()
{
    Schema::table('users', function (Blueprint $table) {
        //
    });
}
```

<a name="factories" />

### Фабрики
**Директория по умолчанию**

`database/factories`
Взаимодействовать с фабриками мы можем посредством метода `factory()` в моделях. Модель автоматически определит к какому классу фабрики ей ссылаться если соблюдать конвенцию наименований.

Те же правила что и для модели, только добавляем суффикс `Factory`!
Формат - `{ИмяМодели}Factory`

**Примеры**:
- Модель `User`, а фабрика `UserFactory`
- Модель `OrderProduct`, фабрика `OrderProductFactory`

<a name="policies" />

### Политики (Policy)
**Директория по умолчанию**

`app/Policies`

Laravel может автоматически определить политику для модели при соблюдении конвенции наименований.
В таком случае политику не требуется регистрировать "вручную".

Используем те же правила, что и для модели, только добавляем суффикс `Policy`!
Формат - `{ИмяМодели}Policy`

**Примеры**:
- Модель `User`, а политика `UserPolicy`
- Модель `OrderProduct`, политика `OrderProductPolicy`

# Практика наименований остальных сущностей

Никак не влияют на взаимодействие с Laravel, но делают код понятнее для всех кто будет с ним взаимодействовать.

<a name="directories" />

### Директории
PascalCase во множественном числе.

**Примеры**:
- `Models`
- `QueryBuilders`
- `Filters`

Laravel дает свободу в расположении сущностей и наименовании директорий.

Как пример, если мы используем `DDD подход`, то нас никто не ограничивает перенести всю бизнес логику в директорию `src`. Также перенести в `src` `/app` и назвать скажем `App`

Но сами группы абстракций в хороших практиках именуются как в примере выше.

<a name="controllers" />

### Controllers
**Директория по умолчанию**

`app/Http/Controllers`

PascalCase в единственном числе c суффиксом Controller.

**Примеры**
- `CatalogController`
- `ProductController`
- `OrderController`
- `BlogController`

<a name="routes" />

### Routes
snake_case либо kebab-case, пользуйтесь здравым смыслом выбирая между единственным либо множественным числом.
`apiResource` и `resource` указываются во множественном числе, несмотря на то, что контроллер остается в единственном числе.


> kebab-case стиль написания в котором слова в нижнем регистре разделяют символом дефиса. Можно представить, что слова при этом как бы насаживают на шампур — вот и получается шашлык (kebab).

**Примеры**
- `Route::get('catalog', CatalogController::class)`
- `Route::get('categories', CategoryController::class)`
- `Route::resource('users', UserController::class)`
- `Route::apiResource('users', UserController::class)`

<a name="blade" />

### Blade
#### views директории
snake_case во множественном числе, несмотря на то, что контроллер остается в единственном числе.

#### blade шаблоны
snake_case

<a name="json" />

### Json response
Ключи в snake_case
