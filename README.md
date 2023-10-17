![main workflow badge](https://github.com/beta-Lilith/kittygram_final/actions/workflows/main.yml/badge.svg)  
  
# KITTYGRAM – СОЦИАЛЬНАЯ СЕТЬ ВЛАДЕЛЬЦЕВ КОТИКОВ. УЧЕБНЫЙ ПРОЕКТ Я.ПРАКТИКУМ.  
  
## ОПИСАНИЕ ПРОЕКТА  
Пользователи могут регистрироваться, добавлять, редактировать, удалять свои записи о котиках с фотографиями и без, могут просматривать записи чужих котиков. Стандартная админка Django.  
  
Цель проекта: научиться работать с Docker, Docker Hub, CI/CD с помощью GitHub Actions.  
  
## ТЕХНОЛОГИИ
- Python 3.9
- Django 3.2.3
- Django REST Framework 3.12.4
- Linux Ubuntu
- Yandex Cloud
- Nginx
- Gunicorn
- Docker
- GitHub Actions
  
## ЗАПУСТИТЬ ПРОЕКТ
  
Клонировать репозиторий:
```
git clone https://github.com/beta-Lilith/kittygram_final.git 
```
  
Добавить секреты для Actions на GitHub:  
(kittygram_final -> Settings -> Secrets and variables -> Actions)  
  
`DOCKER_PASSWORD` - пароль учетной записи DockerHub  
`DOCKER_USERNAME` - имя пользователя учетной записи Docker Hub  
`HOST` - ip-адресс вашего удаленного сервера  
`USER` - имя пользователя на удаленном сервере  
`SSH_KEY` - закрытый SSH ключ от удаленного сервера  
`SSH_PASSPHRASE` - пассфраза от удаленного сервера  
`TELEGRAM_TO` - ваш телеграм id  
`TELEGRAM_TOKEN` - token вашего бота  
  
### НА УДАЛЕННОМ СЕРВЕРЕ: 
  
Установите Docker Compose поочередно выполнив команды
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```
  
Откройте настрой nginx в редакторе nano:
```
sudo nano /etc/nginx/sites-enabled/default
```
  
Отредактируйте по шаблону:
```
server {
    server_name <ip-вашего сервера> <ваше доменное имя>;
    server_tokens off;
    location /api/ {
        proxy_pass http://127.0.0.1:9000;
    }
    location /admin/ {
        proxy_pass http://127.0.0.1:9000;
    }
    location / {
        proxy_pass http://127.0.0.1:9000;
    }
}
```
  
Перейдите в корневую директорию, создайте папку kittygram и перейдите в нее:
```
cd
mkdir kittygram
cd kittygram
```
  
Создать файл .evn на примере .env.example в корне проекта репозитория.
  
Скопируйте docker-compose.production.yml в корневую дирректорию удобным для вас способом
  
Запустить docker-compose.production в режиме демона:
```
docker compose -f docker-compose.production.yml up -d
```
  
Выполнить миграции и собрать статику бэкенда:
```
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /back_static/static/

```
  
Создать суперпользователя, ввести почту, логин, пароль:
  
```
docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```
  
Готово!
  
## АВТОР
Оскомова Ксения