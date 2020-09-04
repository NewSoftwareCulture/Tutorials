# Как перенести проект на хостинг? 

> __Ознакомиться:__
> https://github.com/isuvorov/notes/blob/master/ssh.md

1) Зайти в панель хостинга и оформить новый сервер ubuntu 
2) При оформлении добавить свой ssh ключ
3) Установить туда node.js (установить версию, которая прописана в Dockerfile!!!!!)
4) Залить туда файлы и попробовать запустить
Для того, чтобы залить файлы на хостинг необходимо выполнить следующие шаги:
- Переходим в корень папки проекта на локале
- Создаем там deploy.sh
- Запускаем его
______________________________________
**Создание deploy.sh через терминал:**
> - touch deploy.sh (создаем файл)
> - nano deploy.sh (открываем режим редактирования)
> - chmod u+x deploy.sh (даем права на запуск)
> - ./deploy.sh (запускаем)
__________________________________________________________________

**deploy.sh (заменить на свои HOSTING_IP и PROJECT_NAME)**
> ssh HOSTING_IP 'mkdir -p /root/projects/PROJECT_NAME' && \
> rsync -avz . HOSTING_IP:/root/projects/PROJECT_NAME && \
> ssh HOSTING_IP 'cd /root/projects/PROJECT_NAME && npm i && npm run bootstrap && npm run build && npm start'

ИЛИ

> ssh HOSTING_IP 'mkdir -p /root/projects/PROJECT_NAME' && \
> rsync -avz . HOSTING_IP:/root/projects/PROJECT_NAME && \
> ssh HOSTING_IP 'cd /root/projects/PROJECT_NAME && docker-compose pull && docker-compose up'

______________________________________

**Для установки ноды определенной версии надо немного пошаманить:**

> 1. Устанавливаем пакетный менеджер wget:
> sudo apt install wget 
> 2. Устанавливаем управление версиями ноды NVM:
> wget -qO-https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
> 3. Разрешаем доступ:
> source ~/.profile
> 4. Чекаем список версий ноды:
> nvm ls-remote
> 5. Ставим нужную версию ноды:
nvm install 13.7.0

______________________________________

**Как до файлов на хостинге добраться?**
> 1. Сгенерировать публичный ssh ключ 
> 2. Привязать его к серверу
> 3. Открыть терминал в маке/линуксе и ввести ssh root@ip, где ip - ip сервера (с альясами Игоря - ssh ip)
> 4. Перейти в папку root/projects/PROJECT_NAME/packages/app. Тут лежат файлы

**Как привязать домен к хостингу?**
> - По __IP__
> - Купить __домен__ и привязать в панели хостинга

**Как запустить бота, чтоб он не падал при закрытии терминала?**
> - npm install pm2 -g
> - pm2-runtime npm -- start

**Как установить mongoDb на сервер? (Ubuntu 18.04)**
> - mongodb://localhost:27017/PROJECT_NAME
> - https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04-ru

**Как добраться до админки на хостинге, зная IP сервера?**
> https://IP:PORT/
