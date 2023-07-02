# Проект YaMDb
![workflow status](https://github.com/DashaMalva/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg) <br>
[![Python](https://img.shields.io/badge/-Python-464646?style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646?style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/-Django%20REST%20Framework-464646?style=flat-square&logo=Django%20REST%20Framework)](https://www.django-rest-framework.org/)
[![gunicorn](https://img.shields.io/badge/-gunicorn-464646?style=flat-square&logo=gunicorn)](https://gunicorn.org/)
[![Nginx](https://img.shields.io/badge/-NGINX-464646?style=flat-square&logo=NGINX)](https://nginx.org/ru/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646?style=flat-square&logo=PostgreSQL)](https://www.postgresql.org/)
[![docker](https://img.shields.io/badge/-Docker-464646?style=flat-square&logo=docker)](https://www.docker.com/)

YaMDb - каталог фильмов, книг и музыкальных альбомов с системой рейтингов, отзывов и комментариев к отзывам.
### Описание проекта
Проект YaMDb является учебным и представляет собой каталог художественных произведений различных категорий. Например, произведения могут  делиться на категории ```Книги```, ```Фильмы```, ```Музыка```. Список категорий может быть расширен новыми категориями через интерфейс администратора в Django. Сами произведения в каталоге не хранятся. К произведениям из каталога пользователи могут оставлять отзывы и выставлять оценки. К отзывам пользователи могут оставлять свои комментарии.

### Технологии
Python 3.7<br>
Django 2.2.16<br>
Django rest framework 3.12.4<br>
Gunicorn 20.0.4<br>
Psycopg2 binary 2.9.3

Проект упакован в связанные контейнеры Docker:
- контейнер для Django-проекта (образ python:3.7-slim);
- контейнер для базы данных Postgres (образ postgres:13.0-alpine);
- контейнер для веб-сервера Nginx (nginx:1.21.3-alpine).

### Шаблон наполнения env-файла
В env-файле директории infra должны храниться переменные окружения для работы с базой данных.
Пример наполнения env-файла:
> DB_ENGINE=django.db.backends.postgresql<br>
> DB_NAME=postgres<br>
> POSTGRES_USER=postgres<br>
> POSTGRES_PASSWORD=12345<br>
> DB_HOST=db
> DB_PORT=5432

### Команды для запуска приложения в контейнерах
- Развернуть проект:
```
docker-compose up -d
```
- Выполнить миграции, создать суперпользователя и собрать статику:
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```
### Команды для заполнения базы данными
```
docker-compose exec web python manage.py loaddata fixtures.json 
```

### Возможности YaMDb API
```YaMDb API``` позволяет работать со следующими сущностями:
- Пользователи сервиса;
- Категории произведений;
- Жанры произведений;
- Произведения;
- Отзывы к произведениям;
- Комментарии к отзывам;

```YaMDb API``` позволяет:
- получить список всех пользователей, создать пользователя, получить пользователя по ```username```, изменить данные пользователя по ```username```, удалить пользователя по ```username```, получить данные своей учетной записи, изменить данные своей учетной записи;
- получить список всех произведений; создать отзывы на конкретное произведение с выставлением оценки в диапазоне ```от 1 до 10``` или без оценки; получать информацию об произведении, включая его рейтинг, который рассчитывается как среднее от всех оценок, выставленных пользователями для данного произведения, обновить информацию о произведении, удалять произведение; 
- получать список всех категорий, создать категорию, удалить категорию;
- получить список всех жанров, создать жанр, удалить жанр;
- получить список всех отзывов, создать новый отзыв, получить отзыв, частично обновить отзыв, удалить отзыв;

### Подробная документация по API YaMDb (v1):
Подробная документация по API в формате Redoc доступна по адресу
```http://127.0.0.1:8000/redoc```
### Примеры запросов:
Получение списка произведений [GET]
```
http://127.0.0.1:8000/api/v1/titles/
```
Добавление произведения [POST]
```
http://127.0.0.1:8000/api/v1/titles/
```
Получение списка отзывов к произведению title_id [GET]
```
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/
```
Добавление комментария к отзыву [POST]
```
http://127.0.0.1:8000/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```

### Как развернуть проект на компьютере:
1. Клонировать репозиторий c GitHub
```
$ git clone https://github.com/dvnesteroff/api_yamdb.git
```
2. Создать виртуальное окружение
```
$ python -m venv venv
```
3. Запустить виртуальное окружение
```
$ source venv/Scripts/activate
```
4. Обновить менеджер пакетов pip
```
$ python -m pip install --upgrade pip
```
5. Установить зависимости из requirements.txt
```
$ pip install -r requirements.txt
```
6. Выполнить миграции
```
$ python manage.py migrate
```
7. Выполнить импорт начальных данных (опционально)
```
$ python manage.py load-data
```
8. Запустить проект
```
$ python manage.py runserver
```

### Адреса сервера с работающим приложением
- [Список произведений (API)](http://malvayamdb.hopto.org/api/v1/titles/)
- [Документация API](http://malvayamdb.hopto.org/redoc/)
- [Админ-панель](http://malvayamdb.hopto.org/admin/)


### Лицензия
The MIT License (MIT)

### Авторы проекта
Студенты Яндекс.Практикум, Когорта 12+, курс "API: интерфейс взаимодействия программ": Дарья Матвиевская, Елизавета Лобачевская, Вячеслав Казаков, Денис Нестеров
