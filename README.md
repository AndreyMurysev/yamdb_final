### Описание:
проект корректно запускается по адресу: 51.250.28.118
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», 
«Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Изобразительное.
искусство» или «Ювелирка»).Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и
все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению
может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать
только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку в диапазоне
от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На
одно произведение пользователь может оставить только один отзыв.

### Пользовательские роли
Аноним — может просматривать описания произведений, читать отзывы и комментарии.

Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам
/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки 
произведений. Эта роль присваивается по умолчанию каждому новому пользователю.

Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может 
назначать роли пользователям.

Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера 
— это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

### Создание пользователя администратором
Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей 
запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти 
письмо с кодом подтверждения. Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, 
в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

### Шаблон наполнения env-файла

```
EMAIL_HOST_USER=mail@mail.ru
EMAIL_HOST_PASSWORD=gsdlfghl
SECRET_KEY=p&l%385148kl9(vs
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
ALLOWED_HOSTS='*'

```
### Описание команд для запуска приложения в контейнерах

Создаем образ:
```
docker build -t yamdb .
```
Собираем контейнеры:
```
docker-compose up -d
```
Создаем файлы миграции:
```
docker-compose exec web python manage.py migrate --noinput
```
Создаем администратора:

```
docker-compose exec web python manage.py createsuperuser
```
Собираем файлы статики:
```
docker-compose exec web python manage.py collectstatic --no-input 
```
Проект доступен по адрессу:
```
http://127.0.0.1/admin/
```
### Описание команды для заполнения базы данными
Для переноса данных с файла fixtures.json на PostgreSQL выполним несколько команд:
```
docker-compose exec web python manage.py shell 
```
Выполнить в открывшемся терминале:
```
>>> from django.contrib.contenttypes.models import ContentType
>>> ContentType.objects.all().delete()
>>> quit()
```
```
docker-compose exec web python manage.py loaddata fixtures.json
```

Команда разработчиков:
 - https://github.com/AlexeyRudnev
 - https://github.com/Alexander_Niyazov
 - https://github.com/AndreyMurysev

[![Django-app workflow](https://github.com/AndreyMurysev/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)]
(https://github.com/AndreyMurysev/yamdb_final/actions/workflows/yamdb_workflow.yml)

Авторское право (c) 2021 AAA

Настоящим разрешение предоставляется бесплатно любому лицу, получившему копию
данного программного обеспечения и связанных с ним файлов документации ("Программное обеспечение"), для решения
в Программном обеспечении без ограничений, включая без ограничений права
для использования, копирования, изменения, объединения, публикации, распространения, сублицензии и/или продажи
копии Программного обеспечения, а также для разрешения лицам, которым предоставляется Программное обеспечение
предоставлено для этого при соблюдении следующих условий:

Вышеуказанное уведомление об авторских правах и это уведомление о разрешении должны быть включены во все
копии или существенные части Программного обеспечения.

ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ ПРЕДОСТАВЛЯЕТСЯ "КАК ЕСТЬ", БЕЗ КАКИХ-ЛИБО ГАРАНТИЙ, ЯВНЫХ ИЛИ
ПОДРАЗУМЕВАЕТСЯ, ВКЛЮЧАЯ, НО НЕ ОГРАНИЧИВАЯСЬ ГАРАНТИЯМИ ТОВАРНОЙ ПРИГОДНОСТИ,
ПРИГОДНОСТЬ ДЛЯ ОПРЕДЕЛЕННОЙ ЦЕЛИ И НЕНАРУШЕНИЕ. НИ В КОЕМ СЛУЧАЕ
АВТОРЫ ИЛИ ПРАВООБЛАДАТЕЛИ НЕ НЕСУТ ОТВЕТСТВЕННОСТЬ ЗА ЛЮБЫЕ ПРЕТЕНЗИИ, УБЫТКИ ИЛИ ДРУГИЕ
ОТВЕТСТВЕННОСТЬ, БУДЬ ТО В РЕЗУЛЬТАТЕ ДЕЙСТВИЯ ДОГОВОРА, ДЕЛИКТА ИЛИ ИНЫМ ОБРАЗОМ, ВЫТЕКАЮЩАЯ ИЗ,
ВНЕ ИЛИ В СВЯЗИ С ПРОГРАММНЫМ ОБЕСПЕЧЕНИЕМ ИЛИ ИСПОЛЬЗОВАНИЕМ ИЛИ ДРУГИМИ СДЕЛКАМИ В
ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ.
  
