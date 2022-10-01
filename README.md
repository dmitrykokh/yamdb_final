# yamdb_final
### Настройка для приложения Continuous Integration и Continuous Deployment

- автоматический запуск тестов
- обновление образов на Docker Hub
- автоматический деплой на боевой сервер при пуше в главную ветку main

![example workflow](https://github.com/dmitrykokh/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

### В workflow четыре задачи (job):

- проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest;
- сборка и доставка докер-образа для контейнера web на Docker Hub;
- автоматический деплой проекта на боевой сервер;
- отправка уведомления в Telegram о том, что процесс деплоя успешно завершился.

### Процесс создания виртуальной машины на yandex.cloud
- указать имя ВМ
- выбрать образ Ubuntu
- указать логин для последующего подключения
- указать публичную часть ssh-ключа: *cat ~/.ssh/id_ed25519.pub*

### Для нормальной работы необходимо обновить секреты на GitHub
- SSH_KEY (если генерировался новый ключ)
- HOST

### Необходимо провести некотрую работу на сервере
- ssh <login>@<внешний ip> (вход на сервер)
- sudo systemctl stop nginx (Остановка службы nginx)
- sudo apt install docker.io (Установка docker)
- sudo apt  install docker-compose (установка docker-compose)