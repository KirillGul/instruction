### [Главная страница Composer](https://getcomposer.org)  
### [Команды Composer для CMD](https://getcomposer.org/doc/03-cli.md)
### [Описание Schema composer.json](https://getcomposer.org/doc/04-schema.md)
### [Packagist - хранилище библиотек для PHP](https://packagist.org/)

# Основы Composer
#### Composer - это менеджер пакетов для PHP

Свойства Composer:
* composer.json, имеет формат данных JSON
* в composer.json происходит инициализация свойст проекта, описание зависимостей в разделах require и require-dev, подключение других репозиториев в разделе repositories и др.
* после установки пакета или инициализации файла composer.json необходимо подключить в место  
`spl_autoload_register()`  
прописать:  
`require __DIR__ . '/../vendor/autoload.php';`
* конфигурирование токена для Github  
`composer config -g github-oauth.github.com <token>`  
\<token> - нужно создать на GitHub в разделе https://github.com/settings/tokens

# Вспомогательный команды
* `composer -V` - просмотр установленной версии composer
* `composer -v` - просмотр установленной версии composer и доступных команд
* `composer help [команда]` - справка по команде

# Основные команды
Перед установкой пакетов или обновлением, необходимо перейти в папку где лежит composer.json, с помощью команды `cd <путь>`
* `composer update [имя библиотеки]` - команда проверит наличие новой версии пакета, и если такой есть, то скачает и установит его. При этом также обновится и composer.lock
* `composer install` - команда устанавливает все заданные нами ранее зависимости (если они еще не установлены) и перегенерирует файл автозагрузки (поставит то что было в composer.lock)
    * `composer install --no-dev` - установит все пакеты, кроме раздела `require-dev`
* composer init - первичная инициализация (нужно перейти в каталог с проектом)
    * Package name (<vendor>/<name>) [guliverco/sanivel.loc] – в composer название пакета состоит из двух частей: vendor (имя производителя) и name (имя пакета). Я вам предлагаю использовать какой-нибудь свой никнейм или фамилию в качестве вендора, а в качестве имени пакета – myproject. В моём случае ответ для этого пункта будет такой: ivashkevich/myproject.
    * Description []: - описание проекта, можно оставить пустым.
    * Author [, n to skip]: - автор проекта. В формате John Smith <john@example.com>
    * Minimum Stability []: - минимальная степень стабильности проекта. Об этом позже, пока оставим пустым.
    * Package Type (e.g. library, project, metapackage, composer-plugin) []: - тип пакета. Пишем project.
    * License []: - тип лицензии, по которой будет распространяться наш код. Пишем proprietary.
    * Would you like to define your dependencies (require) interactively [yes]? – Желаете ли Вы определить зависимости для проекта в интерактивном режиме. То есть, хотим ли мы прямо сейчас загружать библиотеки. Отвечаем no.
    * Would you like to define your dev dependencies (require-dev) interactively [yes]? – То же, что и выше, только для разработческого окружения – снова no.
    * Do you confirm generation [yes] – Подтверждаем ли мы генерацию конфигурационного файла. Жмем yes.
    * Would you like the vendor directory added to your .gitignore [yes]? – Нужно ли добавить папку vendor в исключения для git. Yes.
* composer require вендор/имя_пакета - установка пакета
* composer diagnose - проводиться диагностика окружения composer на ошибки
* composer validate - валидация composer.json файла
* composer self-update - обновление самого Composer
* composer creat-project <название пакета> <папка для установки> - установка пакета в папку
* composer run-script <post-install-cmd или др>

# Установка библиотек (пакетов)
* с помщью командой строки, взяв готовую команду из пакета на портале packagist
`composer require yiisoft/yii2`
* через composer.json - прописав зависимости
> Можно прописать зависимости в раздел require и require-dev (это список пакетов которые нужно загрузить для dev окружения)
```json
"require": {
    "php": ">=7.4.0",
    "yiisoft/yii2": "~2.0.45",
},
"require-dev": {
    "yiisoft/yii2-debug": "*",
},
```

# Семантическое версионирование библиотек (пакетов)
`"[разработчик/]пакет": <указатель>x.y.z`  
###  где:  
* x - мажорная версия, большие изменения пакета (удаление и добавление классов, методов), ломается обратная совместимость  
* y - минорная версия, (добавление но не удаление классов, методов), не ломается обратная совместимость  
* z - патч, правка багов

### <указатель>:  
* \* - получим последнюю версию  
* ^ - можем получать в свой проект минорные версии (будет ставить последнюю минроную версию)  
* ~ - можем получать в свой проект патчи (будет ставить последний патч)  
* <>= - указатель на равенство версии  
можно указывать диапазон `">=6.5.2 <=6.5.4"`
* стабильность проекта:
    * `@dev` - код меняется, для разработчиков (рекомендуют ставить)
    * `@alpha` - получше
    * `@beta` - еще лучше
    * `@rc` - релиз кандитат, очень стабильная 
    * `@stable` - стабильная
```json
"require": {
    "php": "@stable",
},
```

## Теги Composer JSON
* `minimum-stability` - в котором можно указать стабильность (рекомендация ставить dev, т.к. долго ждать выход новых стабильных библиотек) `"minimum-stability": "dev"`
* `prefer-stable` - принимает два значения true и false `"prefer-stable": "true"` - указывает Composer, что необходимо использовать стабильные версии (ставить true)
