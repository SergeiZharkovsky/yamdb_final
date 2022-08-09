# **YaMDb Project**

[![Django-app workflow](https://github.com/SergeiZharkovsky/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/SergeiZharkovsky/yamdb_final/actions/workflows/yamdb_workflow.yml)

Ссылка на проект: https://84.252.141.85/admin/

### _Контейнер для проекта "api_yamdb"_

## Описание

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title). Произведения делятся на категории: "Книги", "Фильмы", "Музыка". Список категорий (Category) может быть расширен.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть **произведения**: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.

Произведению может быть присвоен **жанр (Genre)** из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.  

Благодарные или возмущённые пользователи оставляют к произведениям текстовые **отзывы (Review)** и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — **рейтинг** (целое число). На одно произведение пользователь может оставить только один отзыв.

## Технологии
- Python 3.7.0
- Django 2.2.16
- Django Rest Framework 3.12.4
- gunicorn 20.0.4
- nginx 1.21.3
- PostgreSQL 13.0

## Как запустить проект:
###### 1. Клонируйте репозиторий:
```
git clone
```
###### 2. Создайте файл .env с переменными окружения для работы с базой данных:
```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```
###### 3. Перейдите в директорию с файлом docker-compose.yaml и запустите контейнеры:
```
cd infra
docker-compose up -d --build
```
###### 4. После успешного запуска контейнеров выполните миграции в проекте:
```
docker-compose exec web python manage.py migrate
```
###### 5. Создайте суперпользователя:
```
docker-compose exec web python manage.py createsuperuser
```
###### 6. Соберите статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```
###### 7. Создайте дамп (резервную копию) базы данных:
```
docker-compose exec web python manage.py dumpdata > fixtures.json
```
Для остановки контейнеров и удаления всех зависимостей воспользуйтесь командой:
```
docker-compose down -v
```
## Примеры запросов к API:
- Получение списка всех произведений (доступно без токена).

_Запрос:_
```
GET yamdb.com/api/v1/titles/
```
_Пример ответа:_
```
[
  {
    "count": 0,
    "next": "string",
    "previous": "string",
    "results": [
      {
        "id": 0,
        "name": "string",
        "year": 0,
        "rating": 0,
        "description": "string",
        "genre": [
          {
            "name": "string",
            "slug": "string"
          }
        ],
        "category": {
          "name": "string",
          "slug": "string"
        }
      }
    ]
  }
]
```
- Добавление нового отзыва к произведению (доступно аутентифицированным пользователям).

_Запрос:_
```
POST yamdb.com/api/v1/titles/{title_id}/reviews/
```
_Содержимое запроса:_
```
{
  "text": "string",
  "score": 1
}
```
_Пример ответа:_
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "score": 10,
  "pub_date": "2019-08-24T14:15:22Z"
}
```
- Добавление комментария к отзыву (доступно аутентифицированным пользователям).

_Запрос:_
```
POST yamdb.com/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
_Содержимое запроса:_
```
{
  "text": "string"
}
```
_Пример ответа:_
```
{
  "id": 0,
  "text": "string",
  "author": "string",
  "pub_date": "2019-08-24T14:15:22Z"
}
```
- Удаление пользователя по имени пользователя (доступно администратору).

_Запрос:_
```
DELETE yamdb.com/api/v1/users/{username}/
```
### Автор
Sergei Zharkovsky
https://github.com/SergeiZharkovsky
