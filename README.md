# Групповой проект «Yamdb»
![workflow](https://github.com/DenisSivko/yamdb_final/actions/workflows/main.yml/badge.svg)
## Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок высчитывается средняя оценка произведения.
##### Функционал:
###### REVIEWS
- Получить список всех отзывов
- Создать новый отзыв
- Получить отзыв по id
- Частично обновить отзыв по id
- Удалить отзыв по id
###### COMMENTS
- Получить список всех комментариев к отзыву по id
- Создать новый комментарий для отзыва
- Получить комментарий для отзыва по id
- Частично обновить комментарий к отзыву по id
- Удалить комментарий к отзыву по id
###### AUTH
- Отправление confirmation_code на переданный email
- Получение JWT-токена в обмен на email и confirmation_code
###### USERS
- Получить список всех пользователей
- Создание пользователя
- Получить пользователя по username
- Изменить данные пользователя по username
- Удалить пользователя по username
- Получить данные своей учетной записи
- Изменить данные своей учетной записи
###### CATEGORIES
- Получить список всех категорий
- Создать категорию
- Удалить категорию
###### GENRES
- Получить список всех жанров
- Создать жанр
- Удалить жанр
###### TITLES
- Получить список всех объектов
- Создать произведение для отзывов
- Информация об объекте
- Обновить информацию об объекте
- Удалить произведение

Документация к API доступна по адресу `http://127.0.0.1/redoc/`

## Установка
##### Шаг 1. Проверьте установлен ли у вас Docker
Прежде, чем приступать к работе, необходимо знать, что Docker установлен. Для этого достаточно ввести:
```bash
docker -v
```
Или скачайте [Docker Desktop](https://www.docker.com/products/docker-desktop) для Mac или Windows. [Docker Compose](https://docs.docker.com/compose) будет установлен автоматически. В Linux убедитесь, что у вас установлена последняя версия [Compose](https://docs.docker.com/compose/install/). Также вы можете воспользоваться официальной [инструкцией](https://docs.docker.com/engine/install/).

##### Шаг 2. Клонируйте репозиторий себе на компьютер
Введите команду:
```bash
git clone https://github.com/DenisSivko/infra_sp2.git
```

##### Шаг 3. Создайте в клонированной директории файл .env
Пример:
```bash
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

##### Шаг 4. Запуск docker-compose
Для запуска необходимо выполнить из директории с проектом команду:
```bash
docker-compose up -d
```

##### Шаг 5. База данных
Создаем и применяем миграции:
```bash
docker-compose exec web python manage.py makemigrations --noinput
docker-compose exec web python manage.py migrate --noinput
```

##### Шаг 6. Подгружаем статику
Выполните команду:
```bash
docker-compose exec web python manage.py collectstatic --no-input 
```

##### Шаг 7. Заполнение базы тестовыми данными
Для заполнения базы тестовыми данными вы можете использовать файл fixtures.json, который находится в infra_sp2. Выполните команду:
```bash
docker-compose exec web python manage.py loaddata fixtures.json
```

##### Другие команды
Создание суперпользователя:
```bash
docker-compose exec web python manage.py createsuperuser
```

Остановить работу всех контейнеров можно командой:
```bash
docker-compose down
```

Для пересборки и запуска контейнеров воспользуйтесь командой:
```bash
docker-compose up -d --build 
```

Мониторинг запущенных контейнеров:
```bash
docker stats
```

Останавливаем и удаляем контейнеры, сети, тома и образы:
```bash
docker-compose down -v
```

## Примеры
##### Получаем confirmation_code
Для формирования запросов и ответов использована программа [Postman](https://www.postman.com/).

Отправляем POST-запрос на адрес `http://127.0.0.1/api/v1/auth/email/` 

- Обязательное поле: `email`

```json
POST   http://127.0.0.1/api/v1/auth/email/
Content-Type: application/json

{
    "email": "<EMAIL>"
}
```

Код подтверждения будет доступен по адресу `api_yamdb\sent_emails\file.log`

##### Получаем token
Отправляем POST-запрос для получения JWT-токена на адрес `http://127.0.0.1/api/v1/auth/token/`.
- Обязательное поле: `email`,
- Обязательное поле: `confirmation_code`
```json
POST   http://127.0.0.1/api/v1/auth/token/
Content-Type: application/json

{
    "email": "<EMAIL>",
    "confirmation_code": "<CONFIRMATION CODE>"
}
```
##### Запрос для получения списка произведений
Отправляем GET-запрос на адрес `http://127.0.0.1/api/v1/titles/`.
```json
GET http://127.0.0.1/api/v1/titles/
Content-Type: application/json
```
##### Оставить отзыв
Отправляем POST-запрос на адрес `http://127.0.0.1/api/v1/titles/1/reviews/`.
Оценка произведения производится по 10-ти бальной шкале.
```json
POST http://127.0.0.1/api/v1/titles/1/reviews/
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjY0MDIyOTU2LCJqdGkiOiJhOThhNzY5ZjM3MjQ0OGI2YjNkZmU2ZWZhZTk3ZjQ5MyIsInVzZXJfaWQiOjN9.up2BFy3C_Yd3WrsiZLO3EQbZ5DZMqqmfAOXF0lrlIw0
Content-Type: application/json

{
    "text": "<REVIEW TEXT>",
    "score": <RATING>
}
```
