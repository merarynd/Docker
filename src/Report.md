# Simple Docker

## Part 1. Готовый докер

##### Взять официальный докер образ с **nginx** и выкачать его при помощи `docker pull`

![img1](Img/1.png)

##### Проверить наличие докер образа через `docker images`

![img2](Img/2.png)

##### Запустить докер образ через `docker run -d [image_id|repository]`
##### Проверить, что образ запустился через `docker ps`

![img3](Img/3.png)

##### Посмотреть информацию о контейнере через `docker inspect [container_id|container_name]`

![img4](Img/4.png)

Где " nervouse_curran " - container_name.

##### По выводу команды определить и поместить в отчёт размер контейнера, список замапленных портов и ip контейнера

![img5](Img/5.png)

##### Остановить докер образ через `docker stop [container_id|container_name]`
##### Проверить, что образ остановился через `docker ps`

![img6](Img/6.png)

##### Запустить докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run*

![img7](Img/7.png)

##### Проверить, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**

![img8](Img/8.png)

##### Перезапустить докер контейнер через `docker restart [container_id|container_name]`

![img9](Img/9.png)

##### Проверить любым способом, что контейнер запустился

![img10](Img/10.png)

## Part 2. Операции с контейнером

##### Прочитать конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec*

![img11](Img/11.png)

##### Создать на локальной машине файл *nginx.conf*
##### Настроить в нем по пути */status* отдачу страницы статуса сервера **nginx**

![img12](Img/12.png)

##### Скопировать созданный файл *nginx.conf* внутрь докер образа через команду 
`docker cp nginx.conf [container_id|:/etc/nginx/`
##### Перезапустить **nginx** внутри докер образа через команду
`exec [container_id| nginx -s reload`

![img13](Img/13.png)

##### Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![img14](Img/14.png)

##### Экспортировать контейнер в файл *container.tar* через команду 
`docker export [container_id| > container.tar`

![img15](Img/15.png)

##### Остановить контейнер

`docker stop [container_id|`

![img16](Img/16.png)

##### Удалить образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры

![img17](Img/17.png)

##### Удалить остановленный контейнер

`docker rm [container_id|`

![img18](Img/18.png)

##### Импортировать контейнер обратно через команду 
`docker import -c 'cmd["nginx", "-g", "daemon off;"]' container.tar nginx_merarynd`

![img19](Img/19.png)

##### Запустить импортированный контейнер

`docker run -dp 80:80 -p 443:443 [container_id|`

![img20](Img/20.png)

##### Проверить, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![img21](Img/21.png)

## Part 3. Мини веб-сервер

##### Написать мини сервер на **C** и **FastCgi**, который будет возвращать простейшую страничку с надписью `Hello World!`

![img22](Img/22.png)

##### Написать свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080*

![img23](Img/23.png)

##### Запустить написанный мини сервер через *spawn-fcgi* на порту 8080

Нам потребуются команды :
```
docker pull nginx
docker run -d -p 81:81 --name name nginx
docker cp nginx.conf name:/etc/nginx/
docker cp server.c name:/home
docker exec -it name bash
apt-get update
apt-get install -y gcc spawn-fcgi libfcgi-dev
gcc -o server server.c -lfcgi
spawn-fcgi -p 8080 ./server
docker exec name nginx -s reload
```

##### Проверить, что в браузере по *localhost:81* отдается написанная вами страничка

![img24](Img/24.png)

## Part 4. Свой докер

#### Написать свой докер образ, который:

![img25](Img/25.png)

##### Собрать написанный докер образ через `docker build` при этом указав имя и тег
##### Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы **nginx**'а.

![img26](Img/26.png)

##### Проверить через `docker images`, что все собралось корректно

![img27](Img/27.png)

##### Проверить, что по localhost:80 доступна страничка написанного мини сервера

![img28](Img/28.png)

##### Проверить, что теперь по *localhost:80/status* отдается страничка со статусом **nginx**

![img29](Img/29.png)

## Part 5. **Dockle**

##### Просканировать образ из предыдущего задания через `dockle [image_id|repository]`

![img30](Img/30.png)

##### Исправить образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений

![img31](Img/31.png)

## Part 6. Базовый **Docker Compose**

##### Написать файл *docker-compose.yml*, с помощью которого:
##### 1) Поднять докер контейнер
##### 2) Поднять докер контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера
##### Замапить 8080 порт второго контейнера на 80 порт локальной машины

![img32](Img/32.png)

##### Остановить все запущенные контейнеры

![img33](Img/33.png)

##### Собрать и запустить проект с помощью команд `docker-compose build` и `docker-compose up`

![img34](Img/34.png)

##### Проверить, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранее

![img35](Img/35.png)
