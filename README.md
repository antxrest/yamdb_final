![main workflow](https://github.com/antxrest/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

# Проект - "YaMDb"

## Развёрнутый проект

http://84.201.136.174/redoc/

http://84.201.136.174/admin/ 

## Описание

Проект **YaMDb** собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
Произведения делятся на категории, такие как «Книги», «Фильмы», «Музыка». .

Произведению может быть присвоен жанр из списка предустановленных.

*Добавлять произведения, категории и жанры может только администратор.*

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — *рейтинг (целое число)*. На одно произведение пользователь может оставить только один отзыв.

*Пользователи могут оставлять комментарии к отзывам.*

*Добавлять отзывы, комментарии и ставить оценки могут только аутентифицированные пользователи.*

## Пользовательские роли и права доступа

- *Аноним* — может просматривать описания произведений, читать отзывы и комментарии.

- *Аутентифицированный пользователь (user)* — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.

- *Модератор (moderator)* — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.

- *Администратор (admin)* — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

- *Суперюзер Django* должен всегда обладать правами администратора, пользователя с правами admin. Даже если изменить пользовательскую роль суперюзера — это не лишит его прав администратора. Суперюзер — всегда администратор, но администратор — не обязательно суперюзер.

## Аутентификация

Используется аутентификация с использованием JWT-токенов

## Используемые технологии

- Python 3.7.9

- Django 2.2.16

- Djangorestframework 3.12.4

- PostgreSQL 13.0

- Gunicorn 20.0.4

- Nginx 1.21.3

## Установка

1. Клонировать репозиторий и перейти в него в командной строке:
```
git clone git@github.com:antxrest/yamdb_final.git
cd api_yamdb
```
2. Перейдите в директорию infra и создайте файл .env c содержимым:
```
cd infra
touch .env
nano .env
```
```
DB_ENGINE=django.db.backends.postgresql #указываем что работаем с postgresql
DB_NAME=postgres #указываем имя базы данных
POSTGRES_USER=set_your_username #логин для подключения к БД
POSTGRES_PASSWORD=set_your_pwd #пароль для подключения к БД
DB_HOST=db #название контейнера
DB_PORT=5432 #порт для подключения к БД
```
3. Запустите docker-compose.

```
docker-compose up -d
```

4. Проверьте, что контейнеры запустились:

```
docker container ls
```

5. Выполните миграции:

```
docker-compose exec web python manage.py migrate
```

6. Создайте суперпользователя:

```
docker-compose exec web python manage.py createsuperuser
```

7. Подгрузите статику:

```
docker-compose exec web python manage.py collectstatic --no-input
```

8. Заполните базу данными:
   - Скопируйте тестовую базу в контейнер:
    ```
    docker cp fixtures.json <id>:app/
    ```
и загрузите её:
```
docker-compose exec web python manage.py loaddata fixtures.json
```
9. Для того чтобы сохранить свою базу в fixtures:
```
docker-compose exec web python manage.py dumpdata > fixtures.json
```

10. Остановить работу контейнеров можно командой:
```
docker-compose down
```
Документация после запуска доступна по адресу localhost/redoc.

### Автор:
[Антон Молчанов](https://github.com/antxrest)
