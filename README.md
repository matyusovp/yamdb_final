![example workflow](https://github.com/matyusovp/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
В данный момент доступен по адресу: 

http://130.193.43.108/api/v1/

# Проект api_yamdb

## Описание

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title).
Произведения делятся на категории: «Книги», «Фильмы», «Музыка».
Список категорий (Category) и жанров (Genre) может быть расширен
Отзывы могут комментироваться пользователями (Comments) 

## Алгоритм регистрации пользователей
Пользователь отправляет POST-запрос с параметром email на `/api/v1/auth/email/`.
YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на адрес email.
Пользователь отправляет POST-запрос с параметрами email и confirmation_code на `/api/v1/auth/token/`, в ответе на запрос ему приходит token (JWT-токен).
Эти операции выполняются один раз, при регистрации пользователя. В результате пользователь получает токен и может работать с API, отправляя этот токен с каждым запросом.
## Пользовательские роли
**Аноним** — может просматривать описания произведений, читать отзывы и комментарии.
**Аутентифицированный пользователь (user)** — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.
**Модератор (moderator)** — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.
**Администратор (admin)** — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
**Администратор Django** — те же права, что и у роли Администратор.
## Установка локально.
Склонируйте репозиторий. Находясь в папке с кодом создайте виртуальное окружение `python -m venv venv`, активируйте его (Windows: `source venv\scripts\activate`; Linux/Mac: `sorce venv/bin/activate`), установите зависимости `python -m pip install -r requirements.txt`.
Для запуска сервера разработки,  находясь в директории проекта выполните команды:
```
Выполняются миграции. 
python manage.py migrate
Создается суперпользователь. 
python manage.py createsuperuser
Запускается сервер.
python manage.py runserver
```
## Установка через Docker
1. Сделать fork проекта в свой GitHUB;
2. В разделе проекта Setting/Secrets указать логин и пароль DockerHUB с ключами:
DOCKER_USERNAME, DOCKER_PASSWORD
3. В разделе проекта Setting/Secrets указать параметры (хост, логин, ssh-key, пароль ) DockerHUB с ключами:
HOST, USER, SSH_KEY, PASSPHRASE
4. В разделе проекта Setting/Secrets указать параметры базы данных с ключами:
DB_ENGINE, DB_NAME , POSTGRES_USER, POSTGRES_PASSWORD, DB_HOST, DB_PORT
5. В разделе проекта Setting/Secrets указать параметры базы данных с ключами:
DB_ENGINE, DB_NAME , POSTGRES_USER, POSTGRES_PASSWORD, DB_HOST, DB_PORT
6. В разделе проекта Setting/Secrets указать ID телеграм-канала и токен телеграм-бота для получения уведомлений с ключами:
TELEGRAM_TO, TELEGRAM_TOKEN
7. Подготовить сервер:
- Остановить службу nginx:
 sudo systemctl stop nginx 
- Установить докер:
 sudo apt install docker.io 
- Установить docker-compose в соответствии с официальной документацией;
- Скопировать файлы docker-compose.yaml и nginx/default.conf из проекта на сервер в home/<ваш_username>/docker-compose.yaml и home/<ваш_username>/nginx/default.conf соответственно.
8. На GitHUB выполнить commit, после которого запустятся процедуры workflow;
9. На сервере выполнить миграции, создать суперюзера, собрать статику:
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
10. Для заполенния бд, скачайте файл fixtures.json из корневой директории проекта, перенесите его на свой удаленный сервер. после выполните python manage.py loaddata fixtures.json 
11. Набрать в браузере:
http://<ip_сервера>/admin
Работоспособность приложения можно проверить без развертывания на уже запущенном сервере:
http://84.252.142.36/admin
##  Примеры запросов к API:

Получение списка всех категорий:
```
http://127.0.0.1:8000/api/v1/categories/
```
Получение списка всех жанров:
```
http://127.0.0.1:8000/api/v1/genres/
```
Получение списка всех произведений:
```
http://127.0.0.1:8000/api/v1/titles/
```
Получение информации о произведении с ID=1
```
http://127.0.0.1:8000/api/v1/titles/1/
```
Получение списка всех отзывов для произведения с ID=1:
```
http://127.0.0.1:8000/api/v1/titles/1/reviews/
```
Получение информации об отзыве с ID=1 для произведения с ID=1:
```
http://127.0.0.1:8000/api/v1/titles/1/reviews/1/
```
Получение списка комментариев об отзыве с ID=1 для произведения с ID=1:
```
http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/
```
Получение комментария с ID=1 об отзыве с ID=1 для произведения с ID=1:
```
http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/1/
```
Получение списка всех пользователей (Права доступа: Администратор):
```
http://127.0.0.1:8000/api/v1/users/
```
Получение информации о пользователе user1 (Права доступа: Администратор):
```
http://127.0.0.1:8000/api/v1/users/user1/
```

Контакты:
Автор : Павел. М.
tg : bizewka