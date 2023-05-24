Бібліотека ![Підтримувані версії вузлів](https://img.shields.io/badge/dynamic/json?color=informational&label=node&query=%24.engines.node&url=https%3A%2F%2Fraw.githubusercontent.com%2Fnytimes%2Flibrary%2Fmain%2Fpackage.json) [![Тести](https://github.com/nytimes/library/actions/workflows/test.yaml/badge.svg)](https://github.com/nytimes/library/actions/workflows/test.yaml)
========

Сайт документації для спільних новин за допомогою Google Docs.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Зміст**

- [Демо сайт & Керівництво користувачів](#demo-site--user-guide)
- [Зв'яжіться з нами](#contacting-us)
- [Спільнота](#community)
- [Зробити внесок](#contributing)
- [Питання](#questions)
- [Робочий процес в розробці](#development-workflow)
  - [Тести](#tests)
- [Персоналізація](#customization)
- [Розгортання програми](#deploying-the-app)
  - [Використання Heroku](#using-heroku)
  - [Використання Google App Engine](#using-google-app-engine)
  - [Використання Docker](#using-docker)
  - [Стандартне розгортання](#standard-deployment)
- [Структура додатку](#app-structure)
  - [Сервер](#server)
  - [Перегляди](#views)
  - [Doc parsing](#doc-parsing)
  - [Список дисків](#listing-the-drive)
  - [Авторизація](#auth)
  - [Автентифікація користувача](#user-authentication)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Демо сайт & Керівництво користувачів

Документація про те, як почати роботу з Бібліотекою проводиться як робоча (лише читання) демо на Heroku. Зверніться до сайту для більш детальних інструкцій, ніж про читання, як отримати максимальну віддачу від бібліотеки: https://nyt-library-demo.herokuapp.com.

## Зв'яжіться з нами

Подобається бібліотека? Повідомте нам про це, [приєднавшись до нашої групи Google](https://groups.google.com/forum/#!forum/nyt-library-community) і викинь рядок. Також ви будете в курсі останніх функцій бібліотеки у наших коментарях, які будуть відправлені до цього списку.

## Спільнота

До цього часу деякі з організацій використовують бібліотеку.

[Магазин](https://www.marketplace.org/)

[Нью-Йорк таймс](http://nytimes.com)

[Північно-Західний Лицар](https://knightlab.northwestern.edu)

[Зоряний трибун](http://www.startribune.com)

[БДЖОЗ](https://www.wbez.org)

[Департамент історії та графіки в Лос-Анджелесі](https://twitter.com/palewire/status/1326220493762883585)

## Зробити внесок

Дивіться [CONTRIBUTING.md](https://github.com/nytimes/library/blob/master/CONTRIBUTING.md) для отримання інформації про те, як запропонувати код та/або документацію на GitHub або [демо сайті](https://nyt-library-demo.herokuapp.com).

## Питання

Якщо у вас є питання про те, як отримати вашу копію бібліотеки, що працює, [приєднатись до нашої групи Google](https://groups.google.com/forum/#!forum/nyt-library-community)</a> і дайте нам знати, до чого ви працюєте. Ми також спостерігаємо за каналом #proj-library в газеті Nerdery Slack. Ми зробимо все можливе, щоб відповісти на твої запитання.

## Робочий процес в розробці

1. Клонувати і `cd` до репозиторію:

   `git clone git@github.com:nytimes/library.git && cd бібліотеки`


2. З консолі Google API, створіть або оберіть проект, а потім створіть сервісний обліковий запис із роллю користувача Cloud Datastore користувача. Він повинен мати API доступ до Диска і хмаринкових даних. Зберігати ці облікові дані в `сервері/.auth.json`.

   - Щоб використовувати oAuth, необхідно також створити облікові дані oAuth.
   - Щоб використовувати Cloud Datastore API для читання історії, вам потрібно додати до вашого `GCP_PROJECT_ID`.

3. Встановити залежності:

   `встановлення npm --no-optional`

4. Створення файлу `.env` в корені проекту. Приклад `.env` може виглядати як

```bash
# Вузьке середовище (розробка чи виробництво)
NODE_ENV=development
# Облікові дані Google oAuth
GOOGLE_CLIENT_ID=123456-abcdefg.apps.googlecontent. om
GOOGLE_CLIENT_SECRET=abcxyz12345
GCP_PROJECT_ID=library-demo-1234
# розділений комами список затверджених доменів доступу або адрес електронною поштою (підтримується регулярний вираз).
APPROVED_DOMAINS="nytimes.com,dailypennsylvanian.com,(.*)?ar.org,demo.user@demo.site. du"
SESSION_SECRET=supersecretvalue

# Команда Google drive
# команда або ("папка", якщо використовувати папку замість командного драйву)
DRIVE_TYPE=team
# ID диска або спільна папка. Рядок випадкових номерів і букв в в кінці вашого командного диску або посилання на папку.
ПІДТВЕРДЖЕНО=0123456ABCDEF
```
Make sure to not put any comments in the same line as `DRIVE_TYPE` and `DRIVE_ID` vars.

Переконайтеся, що ви поділилися основним приводом або текою з електронної пошти, пов'язаної з створеним сервісним обліковим записом на 2.

**Будьте обережні!** Встановлення NODE_ENV на `розробки` змінює побудовану в поведінці для аутентифікації сайту, щоб дозволити аккаунтам відмінним від APPROVED_DOMAINS. **Ніколи не використовуйте NODE_ENV=розклад для вашого розгорнутого сайту, тільки локально.**

5. Запустити програму:

   `npm запустити годинник`

Зараз додаток має працювати на `localhost:3000`. Зверніть увагу, що Бібліотека вимагає Node v8 або вище.

### Тести
Ви можете запускати функціональні і одиниці тестів, які протестують аналіз аналізу HTML та логіку маршрутизації з `npm тестом`. Звіт покриття може бути сформований запуском `npm run test:cover`.

Тест розбору HTML на основі [підтримується формат, що підтримує документ](https://docs.google.com/document/d/12bUE9b4_aF_IfhxCLdHApN8KKXwa4gQUCGkHzt1FyRI/edit).  Для завантаження свіжої копії HTML після виконання редагувань, запустіть `тест / utils/updateSupportedFormats.js`.

## Персоналізація
Стилі, текст, логіка кешування та посередні програми можна налаштувати на відповідність брендингу вашої організації. Сюди буде показано [налаштування зчитування](https://github.com/nytimes/library/blob/master/custom/README.md).

Зразок персоналізації надано в [nytimes/library-customization-example](https://github.com/nytimes/library-customization-example).


## Розгортання програми
Куди б ви не розгорнули бібліотеку, ви ймовірно хочете [налаштувати обліковий запис служби Google](https://console.cloud.google.com/iam-admin/serviceaccounts) і [OAuth 2. клієнт](https://developers.google.com/identity/protocols/OAuth2) Налаштуйте свій сервісний обліковий запис із доступом до API та хмарні дані.

Якщо ви бажаєте розгорнути бібліотеку з налаштуваннями, створіть git репозиторій з файлами, які ви хотіли б включити. Встановіть змінну оточення `CUSMIZATION_GIT_REPO` у змінну клонування URL. Файли в репозиторії і пакетів, вказані в `package.json` будуть включені в інсталяцію вашої бібліотеки.

Для більш докладної інструкції, ознайомтеся з розділом "Початок роботи сайту": https://nyt-library-demo.herokuapp.com/get-start"

### Використання Heroku

Ця кнопка може швидко розгорнути на Heroku: [![Розгортання](https://www.herokucdn.com/deploy/button.svg)](https://nyti.ms/2CrT2y2)

Встановіть `GOOGLE_APPLICATION_JSON`, `GOOGLE_CLIENT_ID`та `GOOGLE_CLIE_SECRET` з значеннями службового облікового запису та клієнта Oautch. Додати *<your-heroku-app-url\>. om* як авторизований домен в загальній налаштуваннях екрану згоди OAuth і потім додати *http://<your-heroku-app-url\>. om/auth/redirect* як зворотний URL в налаштуваннях облікових даних OAuth.

### Використання Google App Engine
Також можна розгорнути бібліотеку в GAE, використовуючи `app.yaml`. Зверніть увагу, що для використання Google App Engine. Детальні інструкції надаються на [демо сайт](https://nyt-library-demo.herokuapp.com/get-started/deploying-library-google-app-engine).

### Використання Docker [![Докергуб](https://img.shields.io/docker/v/nytimes/library?logo=docker)](https://hub.docker.com/r/nytimes/library/tags)
Бібліотека може використовуватися як базове зображення для розгортання за допомогою Docker. This allows you to automate building and deploying a custom version of Library during Docker's build phase. Якщо ви створите репозиторій зі вмістом `користувацької` папки, ви можете розгорнути бібліотеку з цього репозиторію з Dockerfile наступним чином:

```Dockerfile
FROM nytimes/library

# копіювати користувацькі файли в власний репозиторій бібліотеки
. ./custom/

# перемістити до тимчасової папки встановити власні пакети npm
WORKDIR /usr/src/tmp
COPY package*. son .npmrc ./
RUN npm
# копіювати модулі вузлів, необхідні користувацькими модулями вузла
RUN yes | cp -rf . node_modules/* /usr/src/app/node_modules

# повернутися до каталогу додатків і побудувати
WORKDIR /usr/src/app
RUN npm run build

# start app
CMD [ "npm", "почати" ]
```

### Стандартне розгортання
Бібліотека - це стандартна програма для вузолу, тому її можна розгорнути просто на будь-якому місці. Якщо ви шукаєте розгортання на стандартну VPS, [Підручники Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-16-04) є чудовим ресурсом.

## Структура додатку

### Сервер
Основна вхідна точка для додатку є `index.js`.

Цей файл містить сервер для експресів, який реагуватиме на запити документації в налаштованому командному диску або спільній папці. Крім того, в ній міститься логіка про видачу 404 і вибір шаблону для використання базуючись на шляху.

### Перегляди
Перегляди (макети) розташовані в теці `макетів` папки. Вони використовують розширення `.ejs` який використовує синтаксис, подібний до підкреслення шаблонів.

Стилі основних представлень для `стилів` , що містить файли Sass. Ці файли скомпільовано з CSS і розміщено в `public/css`.

### Doc parsing
Додаток HTML і розбір обробляється `docs.js`. `fetchDoc` приймає ідентифікатор Google doc і зворотній виклик, потім передає HTML документа до зворотнього виклику після завантаження та обробки.

### Список дисків
Обробка вмісту теки NYT обробляється `list.js`. Там є дві експортовані функції:

* `getTree` - це асинхронний виклик, який повертає вкладену хеш (дерево) теки Google Drive на картах дітей. Використовується сервером для визначення чи є маршрут дійсним чи ні.

* `getMeta` синхронно повертає хеш ідентифікаторів Google Doc з об'єктами метаданих які були збережені в ході заповнення дерева. Ці метадані включають редагування історії, авторів документів і батьківських тек.

Дерево і метадані файлу переселяються в пам'ять на інтервалі (наразі 60). Виклик getTree кілька разів не поверне свіжі дані.

### Авторизація

Автентифікація з Google Drive v3 api обробляється файлом auth.js, що надає єдиний спосіб `getAuth`. `getAuth` поверне вже інстанційований клієнт аутентифікації або створить свіжий. Виклик `getAuth` декілька разів не призведе до нового клієнта автентифікації, якщо облікові дані закінчилися; ми повинні зробити це в усті. s файл згодом для автоматичного оновлення облікових даних на якомусь інтервалі для запобігання їх закінчення терміну.

### Автентифікація користувача

В даний час бібліотека підтримує як Slack, так і методи Google Oauth. Оскільки сайти бібліотеки зазвичай мають бути внутрішніми для набору обмежених користувачів, Oauth з вашою організацією наполегливо рекомендується. Щоб використовувати Slack Oauth, вкажіть стратегію Oauth у файлі `.env` , як також:
```
# Slack потрібно використовувати з великої кількості символів для символів Passport.js slack oauth http://www.Pasortjs.org/packages/passport-slack-oauth2/
OATH_STRATEGY=Slack
```
Ви маєте надати облікові дані Slack, що ви можете зробити шляхом створення додатку Library Oauth у вашому робочому просторі Slack. Після створення програми збережіть `CLIENT_ID` та `CLIENT_SECRET` у вашому файлі `.env`:
```
SLACK_CLIENT_ID=1234567890abcdefg
SLACK_CLIENT_SECRET=09876544321qwerty
```
Вам необхідно додати URL для зворотнього виклику до вашого додатку Slack, щоб дозволити Slack перенаправляти назад у бібліотеку після перевірки автентичності користувача.
