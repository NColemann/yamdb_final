# YaMDb API 
---
## Описание
Проект YaMDb собирает отзывы пользователей на произведения.
Произведения делятся на категории: "Книги", "Фильмы", "Музыка".
Список категорий может быть расширен администратором
(например, можно добавить категорию "Ювелирные изделия").
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
Произведению может быть присвоен жанр из списка.
Новые жанры может создавать только администратор.
Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти;
из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число).
На одно произведение пользователь может оставить только один отзыв.

### Технологии
1. Python 3.7
2. Django 2.2.16
3. Django REST framework 3.12.4

### Зависимости
```
django==2.2.16
djangorestframework==3.12.4
djangorestframework-simplejwt==5.1.0
django-filter==21.1
pytest==6.2.4
pytest-django==4.4.0
pytest-pythonpath==0.7.3
PyJWT==2.1.0
requests==2.26.0
```

### Шаблон наполнения файла .env

```yaml
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

### Запуск приложения в контейнерах
- Из директории проекта перейдите в директорию с файлом docker-compose и выполните команду:
```
cd infra
docker-compose up -d --build
```
- Выполните миграции
```
docker-compose exec web python manage.py migrate 
```
- Чтобы заполнить базу тестовыми данными выполните команду
```
docker-compose exec web python manage.py upload_data_to_db
``` 
- Собрать статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```
Проект будет доступен по адресу: http://localhost:8080/

 ---

### Некоторые примеры запросов к API

#### Cписок всех произведений
GET /api/v1/titles/

#### Cписок всех отзывов
GET /api/v1/titles/{title_id}/reviews/

#### Добавить новый отзыв
POST /api/v1/titles/{title_id}/reviews/
```json
{
  "text": "string",
  "score": "integer"
}
```

#### Получение отзыва по id
GET /api/v1/titles/{title_id}/reviews/{review_id}/

#### Добавление комментария к отзыву
POST /api/v1/titles/{title_id}/reviews/{review_id}/comments/
```json
{
  "text": "string"
}
```

#### Регистрация нового пользователя
POST /api/v1/auth/signup/
```json
{
  "email": "string",
  "username": "string"
}
```

#### Получение JWT-токена
POST /api/v1/auth/token/
```json
{
  "username": "string",
  "confirmation_code": "string"
}
```

### [Ссылка](http://127.0.0.1:8000/redoc/) на документацию для YaMDb API.
