# Киттиграм

## Домен: https://kittygram.servehttp.com/

## Оглавление:
- [Стек технологий.](#Стек-технологий)
- [Краткое описание проекта.](#Краткое-описание-проекта)
- [Как запустить проект.](#Как-запустить-проект)
  - [Запуск с DockerHub.](#Запуск-с-DockerHub)
  - [Запуск с GitHub.](#Запуск-с-GitHub)

## Стек технологий
- Python
- Django
- DRF
- Docker
- Nginx
- Gunicorn
- GitHub Actions
- Linux

## Краткое описание проекта:
Сайт с возможностью публикации фотографий котов и их достижений.
  
В файле docker-compose описывается система из контейнеров. Docker-compose-production описывает уже непосредственно вариант с работой через GitHub Actions и деплоем на сервере.

Реализован деплой на сервере после загрузки на GitHub при помощи GitHub Actions. Перед деплоем GitHub Actions делает следующее:
- тестирует backend код на соответствие PEP8
- тестирует frontend код
- загружает на DockerHub все контейнеры (c nginx, backend и frontend)
- с DockerHub осуществляет деплой на сервер
- направляет в Телеграме сообщение о совершенном деплое, уточняя:
  - кто сделал коммит
  - какое сообщение было у коммита
  - ссылку на коммит

## Как запустить проект:
- Скачать docker на сервер, если его нет. Инструкции: https://docs.docker.com/get-docker/
- [Запустить с DockerHub](#Запуск-с-DockerHub) или [запустить с GitHub](#Запуск-с-GitHub).
- Выполнить миграции:
  
  После запуска необходимо выполнить сбор статистики и миграции бэкенда. Статистика фронтенда собирается во время запуска контейнера, после чего он останавливается.
  ```
  sudo docker exec -it [имя или id контейнера, узнать можно через sudo docker ps -a] python manage.py migrate

  sudo docker exec -it [имя или id контейнера, узнать можно через sudo docker ps -a] python manage.py  collectstatic

  sudo docker exec -it [имя или id контейнера, узнать можно через sudo docker ps -a] cp -r /app/collected_static/. /static/static/
  ```
- Как остановить систему контейнеров:
  ```
  sudo docker compose -f docker-compose.yml down
  ```
- 127.0.0.1:9000 Главная страница

### Запуск с DockerHub
1. Создать папку проекта, например kittygram и перейти в нее:
  ```
  mkdir kittygram
  cd kittygram
  ```
2. Создать .env файл со сделующей информацией:
  ```
  POSTGRES_USER=логин
  POSTGRES_PASSWORD=пароль
  POSTGRES_DB=имя БД
  DB_HOST=название хоста
  DB_PORT=5432
  SECRET_KEY=секретный ключ (если идет подключение к серверу)
  DEBUG=False
  ALLOWED_HOSTS=ip адрес хоста(если есть),127.0.0.1,localhost,доменное имя (если есть)
  ```  
3. В папку проекта скачать и запустить файл docker-compose.production.yml:
   ```
   sudo docker compose -f docker-compose.production.yml up
   ```
   
### Запуск с GitHub
1. Клонировать репозиторий с проектом на свой компьютер:
   ```git clone git@github.com:gaifut/kittygram_final.git```

2. Установить и активировать виртуальное окружение: 
```
python3.9 -m venv venv
. venv/bin/activate
```
В виртуальном окружении в папке backend (```cd backend```)установить зависимости:
```
pip install -r requirements.txt
```

3. Создать .env файл со сделующей информацией:
  ```                                                       
  POSTGRES_USER=логин
  POSTGRES_PASSWORD=пароль
  POSTGRES_DB=имя БД
  DB_HOST=название хоста
  DB_PORT=5432
  SECRET_KEY=секретный ключ (если идет подключение к серверу)
  DEBUG=False
  ALLOWED_HOSTS=ip адрес хоста(если есть),127.0.0.1,localhost,доменное имя (если есть)
  ```
4. Выполнить сборку контейнеров:
   ```
   sudo docker compose -f docker-compose.yml up
   ```

