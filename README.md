# Проект API для YaMDb
[![Python](https://img.shields.io/badge/-Python-464646?style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646?style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/-Django%20REST%20Framework-464646?style=flat-square&logo=Django%20REST%20Framework)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646?style=flat-square&logo=PostgreSQL)](https://www.postgresql.org/)
[![Nginx](https://img.shields.io/badge/-NGINX-464646?style=flat-square&logo=NGINX)](https://nginx.org/ru/)
[![gunicorn](https://img.shields.io/badge/-gunicorn-464646?style=flat-square&logo=gunicorn)](https://gunicorn.org/)
[![docker](https://img.shields.io/badge/-Docker-464646?style=flat-square&logo=docker)](https://www.docker.com/)
[![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646?style=flat-square&logo=GitHub%20actions)](https://github.com/features/actions)
[![Yandex.Cloud](https://img.shields.io/badge/-Yandex.Cloud-464646?style=flat-square&logo=Yandex.Cloud)](https://cloud.yandex.ru/)
[![Django-app workflow](https://github.com/AndreyMurysev/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)]

## Описание:

В redoc указано все для работы с API. Проект создан для изучения DevOps метод и копирует исходный код из командного проекта
[api_yamdb](https://github.com/AndreyMurysev/api_yamdb)  

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

## Пользовательские роли

| Функционал | Авторизованные пользователи |  Неавторизованные пользователи | Администратор  | Модератор |
|:----------------|:---------:|:---------:|:---------:|:---------:|
| Просматривать описания произведений | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Читать отзывы | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Читать комментарии | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Публиковать отзывы | :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| Ставить оценки произведениям | :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| Комментировать отзывы | :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| Редактировать и удалять свои отзывы | :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| Создавать и удалять произведения, категории и жанры. | :x: | :x: | :heavy_check_mark: | :x: |
| Назначать роли пользователям | :x: | :x: | :heavy_check_mark: | :x: |
| Удалять и редактировать любые отзывы и комментарии | :x: | :x: | :heavy_check_mark: | :heavy_check_mark: |

_Суперюзер Django должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера 
 — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер._

## Создание пользователя администратором

Пользователя может создать администратор — через админ-зону сайта или через POST-запрос на специальный эндпоинт api/v1/users/ (описание полей 
запроса для этого случая — в документации). В этот момент письмо с кодом подтверждения пользователю отправлять не нужно.
После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/ , в ответ ему должно прийти 
письмо с кодом подтверждения. Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, 
в ответе на запрос ему приходит token (JWT-токен), как и при самостоятельной регистрации.

## Запуск проекта

- Клонировать репозиторий GitHub (не забываем создать виртуальное окружение и установить зависимости):
[https://github.com/AndreyMurysev/yamdb_final/](https://github.com/AndreyMurysev/yamdb_final/)

- Создать файл .env в папке проекта:
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
- в Docker cоздаем образ :
```
docker build -t yamdb .
```
- Собираем контейнеры:
```
docker-compose up -d
```
- В результате должны быть собрано три контейнер, при введении следующей команды получаем список запущенных контейнеров:  
```
docker ps
```  
_Пример:_  

```
CONTAINER ID   IMAGE                             COMMAND                  CREATED       STATUS       PORTS                               NAMES
ffbe984f7533   nginx:1.19.3                      "/docker-entrypoint.…"   3 weeks ago   Up 3 weeks   0.0.0.0:80->80/tcp, :::80->80/tcp   andrey_murysev_nginx_1
5166bcfb1188   yamdb:latest                      "/bin/sh -c 'gunicor…"   3 weeks ago   Up 3 weeks                                       andrey_murysev_web_1
a9c7a7542ddb   postgres:12.4                     "docker-entrypoint.s…"   3 weeks ago   Up 3 weeks   5432/tcp                            andrey_murysev_db_1
```
_Назначение контейнеров_:  

| IMAGES | NAMES | DESCRIPTIONS
|:----------------:|:---------|:---------:|
| nginx:1.19.3 | yamdb_nginx_1 | контейнер HTTP-сервера |
| postgres:12.4 | yamdb_web_1 | контейнер базы данных |
| andreymurysev/mysite_fod:latest  | yamdb_db_1 | контейнер приложения Django|


- Сделать миграции, создать суперпользователя и собрать статику:
```
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```

- Описание команды для заполнения базы данными
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
**Проект доступен по адресу:**
```
http://127.0.0.1/admin/
```

## Команда разработчиков:
 - https://github.com/AlexeyRudnev
 - https://github.com/Alexander_Niyazov
 - https://github.com/AndreyMurysev


### Для связи с разработчиками:
**email:** _andreimurysev@yandex.ru_  
**telegram:** _@andrey_murysev_  
  
