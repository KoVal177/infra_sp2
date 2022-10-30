# Разворачивание проекта ["Yamdb"](https://github.com/KoVal177/api_yamdb/) на Docker.
### Описание
Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles), подробное описание см. по [ссылке](https://github.com/KoVal177/api_yamdb/).
Проект разворачивается с помощью Docker на 3-х контейнерах: приложение, база данных и сервер (nginx). Конфигурационный файл `docker-compose.yaml` находится в папке `infra`.   
Перед запуском процесса создания образов докером необоходимо создать файл с переменными окружения `.env` по следующему шаблону:
```
DB_ENGINE=<бэкенд Django для подключения к базе данных> 
DB_NAME=<название базы данных>
POSTGRES_USER=<имя пользователя с правами редактирования базы данных>
POSTGRES_PASSWORD=<пароль>
DB_HOST=<название контейнера с базой данных>
DB_PORT=<порт для подключение к контейнеру с базой данных>
```
В случае использования Postgresql файл с переменными может выглядеть следующим образом:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```
### Основные команды для работы
Развертывание проекта в Docker и его запуск (в фоновом режиме, с пересозданием контейнеров при необходимости):  
`docker-compose up -d --build`  

Остановка работы проекта:  
`docker-compose down`  

Выполнение миграций:  
`docker-compose exec web python manage.py migrate`  

Создание суперюзера (адиминистратора) для базы:  
`docker-compose exec web python manage.py createsuperuser`  

Наполнение проекта статикой:  
`docker-compose exec web python manage.py collectstatic --no-input`  

Создание фикстур для заполнения базы данных тестовыми данными:  
`docker-compose exec web python manage.py create_fixtures`  

Заполнения базы данных тестовыми данными из созданных фикстур:  
`docker-compose exec web python manage.py populate_fixtures`  

### !!! Важно
При разворачивании проекта на локальной машине точкой входа для API-запросов является [http://127.0.0.1/api/v1/](http://127.0.0.1/api/v1/)
