# yamdb_final
![example workflow](https://github.com/AlexBatanov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg) 

YaMDb собирает **отзывы** пользователей на **произведения**. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

Произведения делятся на **категории**, такие как «Книги», «Фильмы», «Музыка». Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Жуки» и вторая сюита Баха. Список категорий может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Произведению может быть присвоен **жанр** из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).

Добавлять произведения, категории и жанры может только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые **отзывы** и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — **рейтинг** (целое число). На одно произведение пользователь может оставить только один отзыв.

Пользователи могут оставлять **комментарии** к отзывам.

Добавлять отзывы, комментарии и ставить оценки могут только аутентифицированные пользователи.
### Технологии
Python 3.9
Django 3.2
DRF
JWT
CI на GitHub Action

### После каждого git push будет происходить следующее:
* проверка кода на PEP8 
* запуск pytest проекта
* сборка и доставка докер-образов на Docker Hub
* автоматический деплой.
* отправка уведомления в Telegram в случае успеха.

Клонировать репозиторий и перейти в него в командной строке:

```
git clone git@github.com:AlexBatanov/infra_sp2.git
```

```
cd infra_sp2/infra
```

Шаблон наполнения .env файла:

```
DB_ENGINE=django.db.backends.postgresql (указываем, что работаем с postgresql)
DB_NAME=postgres (имя базы данных)
POSTGRES_USER=postgres (логин для подключения к базе данных)
POSTGRES_PASSWORD= yourpassword (пароль для подключения к БД (установите свой))
DB_HOST=db (название сервиса (контейнера))
DB_PORT=5432 (порт для подключения к БД)
SECRET_KEY=secret_key (секретный ключ Django проекта)
HOST=01.234.567.890 (порт сервера)
```

Запуск docker-compose:

```
sudo docker-compose up -d --build 
```

Выполнить миграции:

```
sudo docker-compose exec web python manage.py migrate
```

Создать суперпользователя:

```
sudo docker-compose exec web python manage.py createsuperuser
```

Сбор статических файлов:

```
sudo docker-compose exec web python manage.py collectstatic --no-input
```

Заполнение бд данными:

```
python manage.py loaddata fixtures.json
```

Открыть документацию:

```
http://localhost/redoc/
```
***

### Регистрация:

```
POST http://localhost/api/v1/auth/signup/
```
тело запроса:
```
{
    "email": "user@example.com",
    "username": "string"
}
```
после запроса на указанную почту придет ключ, для получения токена

### Получение JWT-токена
```
http://localhost/api/v1/auth/token/
```
тело запроса:
```
{
    "username": "string",
    "confirmation_code": "string"
}
```
### Другие примеры запросов:


#### Получение списка всех произведений:

```
GET http://localhost//api/v1/titles/
```
вернет ответ в формате JSON:
```
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
```

#### Частичное обновление информации о произведении:

```
PATCH http://localhost//api/v1/titles/{titles_id}/
```
тело запроса:
```
{
  "name": "string",
  "year": 0,
  "description": "string",
  "genre": [
    "string"
  ],
  "category": "string"
}
```

#### Добавление нового отзыва:

```
POST http://localhost//api/v1/titles/{title_id}/reviews/
```
тело запроса:
```
{
    "text": "string",
    "score": 1
}
```

#### Частичное обновление отзыва по id:

```
PUTCH http://localhost//api/v1/titles/{title_id}/reviews/{review_id}/
```
тело запроса:
```
{
  "text": "string",
  "score": 1
}
```
### Автор
Александр Батанов
