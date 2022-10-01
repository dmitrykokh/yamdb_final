# CI и CD проекта api_yamdb

## Технологический стек

![Django-app workflow](https://github.com/dmitrykokh/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

![Python](https://img.shields.io/badge/-Python-464646?style=flat&logo=Python&logoColor=56C0C0&color=008080)
![Django](https://img.shields.io/badge/-Django-464646?style=flat&logo=Django&logoColor=56C0C0&color=008080)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646?style=flat&logo=PostgreSQL&logoColor=56C0C0&color=008080)
![Docker](https://img.shields.io/badge/-Docker-464646?style=flat&logo=Docker&logoColor=56C0C0&color=008080)
![Yandex.Cloud](https://img.shields.io/badge/-Yandex.Cloud-464646?style=flat&logo=Yandex.Cloud&logoColor=56C0C0&color=008080)

### Настройка для приложения Continuous Integration и Continuous Deployment

- автоматический запуск тестов
- обновление образов на Docker Hub
- автоматический деплой на боевой сервер при пуше в главную ветку main

### В workflow четыре задачи (job):

- проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest;
- сборка и доставка докер-образа для контейнера web на Docker Hub;
- автоматический деплой проекта на боевой сервер;
- отправка уведомления в Telegram о том, что процесс деплоя успешно завершился.

### Подготовка для запуска workflow
Создайте и активируйте виртуальное окружение, обновите pip:
```
python3 -m venv venv
. venv/bin/activate
python3 -m pip install --upgrade pip
```
Запустите автотесты:
```
pytest
```

### Процесс создания виртуальной машины на yandex.cloud
- указать имя ВМ
- выбрать образ Ubuntu
- указать логин для последующего подключения
- указать публичную часть ssh-ключа: *cat ~/.ssh/id_ed25519.pub*

Отредактируйте файл `nginx/default.conf` и в строке `server_name` впишите IP виртуальной машины (сервера).  
Скопируйте подготовленные файлы `docker-compose.yaml` и `nginx/default.conf` из вашего проекта на сервер:
```
scp -r nginx <login>@<ip>:/home/<login>/
scp docker-compose.yaml <login>@<ip>:/home/<login>/
```

### Для нормальной работы необходимо обновить секреты на GitHub
- SSH_KEY (если генерировался новый ключ)
- HOST

### Необходимо провести некотрую работу на сервере
- ssh <login>@<внешний ip> (вход на сервер)
- sudo systemctl stop nginx (Остановка службы nginx)
- sudo apt install docker.io (Установка docker)
- sudo apt  install docker-compose (установка docker-compose)

### В репозитории на Гитхабе добавьте данные в `Settings - Secrets - Actions secrets`:
```
DOCKER_USERNAME - имя пользователя в DockerHub
DOCKER_PASSWORD - пароль пользователя в DockerHub
HOST - ip-адрес сервера
USER - пользователь
SSH_KEY - приватный ssh-ключ (публичный должен быть на сервере)
PASSPHRASE - кодовая фраза для ssh-ключа (при наличии)
DB_ENGINE - django.db.backends.postgresql
DB_HOST - db
DB_PORT - 5432
TELEGRAM_TO - id своего телеграм-аккаунта (можно узнать у @userinfobot, команда /start)
TELEGRAM_TOKEN - токен бота (получить токен можно у @BotFather, /token, имя бота)
DB_NAME - postgres (по умолчанию)
POSTGRES_USER - postgres (по умолчанию)
POSTGRES_PASSWORD - postgres (по умолчанию)
```
### После успешного деплоя:
Соберите статические файлы (статику):
```
docker-compose exec web python manage.py collectstatic --no-input
```
Примените миграции:
```
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate --noinput
```
Создайте суперпользователя:
```
docker-compose exec web python manage.py createsuperuser

```
или
```
docker-compose exec web python manage.py loaddata fixtures.json
```
## Автор
Дмитрий Кох