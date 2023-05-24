<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Зміст**  *згенеровано з [DocToc](https://github.com/thlorenz/doctoc)*

- [Налаштувати бібліотеку](#customizing-library)
  - [`власний` каталог](#the-custom-directory)
  - [Styles](#styles)
  - [Текст, мова та брендинг](#text-language-and-branding)
  - [Середня програма](#middleware)
  - [Клієнт кешу](#cache-client)
  - [Розмітка](#layouts)
  - [Автентифікація](#authentication)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Налаштувати бібліотеку

## `власний` каталог

The `custom` directory відображає структуру каталогів `сервера`. Для певних файлів можна створити заміни для довільної реалізації. Користувальницькі програми також підтримуються.

Шлях `настроюваний` з використанням всіх насінин буде мати наступну структуру:
```
custom/
├── package.json       // for any npm modules required for custom code
├── cache
│   └── store.js
├── layouts
│   └── categories
│       ├── default.ejs // override for default content layout
|       └── outer-space.ejs // override for paths beginning with "outer-space", for example 🚀
├── middleware
│   ├── middleware1.js // pre/postload exports will be included as middleware
│   └── middleware2.js
├── strings.yaml
├── styles
│   ├── _theme.scss
│   └── _custom.scss
└── userAuth.js        // authentication middleware overrides
```

Демо каталогу `користувальницький` [доступний на GitHub](https://github.com/nytimes/library-customization-example).

## Styles
Перевизначення змінної Sass може бути розташовано в `custom/styles/_theme.scs`. Це дозволяє встановити власні значення для кольорів і шрифтів у всьому додатку. приклад файлу теми може виглядати наступним чином:

```scss
// Імпорт шрифтів
@import url('https://fonts.googleapis. om/css? amily=Alfa+Slab+One|Open+Sans:400,400,700,700,700i&subset=latin-ext');

 // Приклад світлової теми
 $font-branding: "Alpha Slab One", крива;
 $font-заголовок: "Відкриті санси", арифметична, привітання, санс-засічки;
 $font-sans: "Відкриті сани", без засіків;
 $masthead-фону:          $gray-10;
 $main- фон на домашній сторінці:     $white;
 $main-домашній іконний колір:     $black;
 $main-homepage-icon-border:    $black;
 $main-color-page-text-:         $gray-35;
 $main-page-btn-color:          $gray-45;
 $search-container-фон: #e6e2e2;
 $search-контейнер-підказка:       $gray-70;
 $btn-домашнє тло:      $white;
 $btn-homepage-border:          $black;
 $btn-<unk> фон:       $white;
 $btn-<unk> фоновий фон: $accent;
 $btn- границя:           $black;
 $btn-<unk> text-color:       $black;
 $btn-<unk> text-hover:       $black;
 $btn-user-initial:             $gray-50;
 $elem-link-color:              $gray-50;
 $elem-active-link-color:       $gray-50;
 $elem-link-link:             $accent;
 $nav-text-color:               $gray-40;
 ```


Якщо ви хочете використовувати тему за замовчуванням, але хочете перевизначити деякі стильні, ви можете додати `_custom. css` файл у папку стилів. Там, ви можете розмістити склянки і перевизначити будь-які стилі, які ви бачите поруч із ним.

## Текст, мова та брендинг
Ім'я сайту, логотип і більша частина тексту веб-сайту може бути змінена з файлу `strings.yaml`. Будь-яке значення у `config/strings.yaml` може бути перевизначено розташуванням значення для тієї ж клавіші в `custom/strings. aml`, з користувацькою стрічкою, функцією Javascript або шляхом до зображень.

## Модал зображення
Зображення можуть відкривати/розширено в модні/діалоговому вікні. На наведенні зображення курсор перетвориться в стиль курсора, і покаже кнопку "розгорнути". Після натискання по натисканню зображення буде центром сторінки, і мінімізація відображатиметься піктограма. Стилі для модального вигляду можна знайти в `/styles/partials/core/_fur. css` так само як і одна лінія у `/styles/partials/core/_base.scs` , що додає вказівник до зображення у вказівнику. Всі HTML/JS для зображення модалу включені в файл [`/layouts/partials/imgModal.ejs`](../layouts/partials/imgModal.ejs) Модал зображення потім імпортується в макет за замовчуванням для вмісту, який можна знайти [тут](../layouts/categories/default.ejs#L29).

## Середня програма
Можна додати Middleware до початку або кінця запиту циклу, розмістивши файли в `користувач/middleware`. Ці файли можуть експортувати `попередній завантаження` і `postload` функцій. These functions must be valid middleware with params `(res, req, next)`.

`експорт` буде доданий до початку циклу запиту, а `експорт` додається майже до кінця.


## Клієнт кешу
За замовчуванням бібліотека використовує вбудований кеш пам'яті. Клієнт кешу може бути записаний для використання в його місці.

Клієнт кешу може бути використаний шляхом розміщення файлу `store.js` в каталог `custom/cache` . Цей файл повинен експортувати об’єкт з методами
- `set(key, value, callback)`, де `callback` приймає `(err, успіх)`
- `get(key, callback)`, де `callback` приймає `(помилкове, значення)`

## Розмітка
Стандартні макети і шаблони включені в рівень `макетів` каталогу, і [автоматично затягуються](https://github.com/nytimes/library/pull/200) присутністю дзеркального файлу в теці налаштованих/макетів.

За допомогою цієї теки можна замінити стандартні шаблони, спрацьовуючи вашу копію бібліотеки, щоб налаштувати розмітку навколо сайту, також включаючи спеціальні елементи шаблону для конкретних підрозділів вашого сайту.

Наприклад, щоб перевизначити розмітку на вашій сторінці пошуку, ви можете включити `custom/layouts/pages/search. js` файл, щоб замінити вихід [цей шаблон за замовчуванням](https://github.com/nytimes/library/blob/master/layouts/pages/search.ejs#L23:L23).

Якщо у вас є папка у вашому Shared Drive/Теки, яка була викликана "Outer Space", створення шаблону на `custom/layouts/categories/outer-space. js` будуть служити означенням для всіх сторінок вкладених під цією секцією вашого сайту, заміна [цей шаблон за замовчуванням](https://github.com/nytimes/library/blob/master/layouts/categories/default.ejs) для відповідних шляхів.

При перевизначенні стандартних шаблонів включено до бібліотеки, зазвичай ідеєю є копіювання базового шаблону в власне розташування, після чого необхідно налаштувати ефект, поки ви досягнете бажаного ефекту. Це допомагає гарантувати, що важливі частини меблів сайту не видаляються випадково.

## Автентифікація
За замовчуванням бібліотека використовує Google oAuth та [`паспорт`](http://www.passportjs.org/) для автентифікації користувачів. Different authentication systems can be used by overriding `custom/userAuth.js`, and can easily be implemented using a different [`passport` strategy](http://www.passportjs.org/packages/) that fits needs of your organization.

This file must export an express router or middleware that contains all authentication logic. Логіка, що розташована в цьому файлі, запускається рано в заданому ланцюжку, дозволяє переконатися, що користувач буде перевірений на автентичність перед тим, як він зможе отримати доступ до вмісту сайту.
