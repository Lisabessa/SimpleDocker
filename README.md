# Simple Docker

Введение в докер. Разработка простого докер образа для собственного сервера.

## Part 1. Готовый докер

- На мак установлен докер
- После установки предварительно выполняем следующие команды, чтоб сохранить свободное место на диске:
  `rm -rf ~/Library/Containers/com.docker.docker`
  `mkdir -p ~/goinfre/Docker/Data`
  `ln -s ~/goinfre/Docker ~/Library/Containers/com.docker.docker`

- Взят официальный докер образ с **nginx** и выкачан при помощи `docker pull nginx`
- Проверено наличие докер образа через `docker images`

![simple_docker](assets/Part1_1.png)

- Запущен докер образ через `docker run -d [image_id|repository]`
- Проверено, что образ запустился через `docker ps`

![simple_docker](assets/Part1_2.png)

- Просмотрена информация о контейнере через `docker inspect [container_id|container_name]`

![simple_docker](assets/Part1_3.png)
![simple_docker](assets/Part1_4.png)
![simple_docker](assets/Part1_5.png)

- Размер контейнера: 1095

![simple_docker](assets/Part1_6.png)

- Список замапленных портов: "80/tcp"

![simple_docker](assets/Part1_7.png)

- ip контейнера: 172.17.0.2

![simple_docker](assets/Part1_8.png)

- Остановлен докер образ через `docker stop [container_id|container_name]`
- Проверено, что образ остановился через `docker ps`

![simple_docker](assets/Part1_9.png)

- Запущен докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду *run*

![simple_docker](assets/Part1_10.png)

- Проверено, что в браузере по адресу *localhost:80* доступна стартовая страница **nginx**

![simple_docker](assets/Part1_11.png)

- Перезапущен докер контейнер через `docker restart [container_id|container_name]`
- Проверено, что контейнер запустился

![simple_docker](assets/Part1_12.png)


## Part 2. Операции с контейнером

- Прочитан конфигурационный файл *nginx.conf* внутри докер контейнера через команду *exec*

![simple_docker](assets/Part2_1.png)

- Создан на локальной машине файл *nginx.conf*
- В нем настроена по пути */status* отдача страницы статуса сервера **nginx**

![simple_docker](assets/Part2_2.png)

- Скопирован созданный файл *nginx.conf* внутрь докер образа через команду `docker cp`

![simple_docker](assets/Part2_3.png)

- Перезапущен **nginx** внутри докер образа через команду *exec*

![simple_docker](assets/Part2_4.png)

- Проверено, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![simple_docker](assets/Part2_5.png)

- Экспортирован контейнер в файл *container.tar* через команду *export*

![simple_docker](assets/Part2_6.png)

- Контейнер остановлен
- Удален образ через `docker rmi [image_id|repository]`, не удаляя перед этим контейнеры
- Удален остановленный контейнер

![simple_docker](assets/Part2_7.png)

- Импортирован контейнер обратно через команду *import*
- Запущен импортированный контейнер

![simple_docker](assets/Part2_8.png)

- Проверено, что по адресу *localhost:80/status* отдается страничка со статусом сервера **nginx**

![simple_docker](assets/Part2_9.png)

## Part 3. Мини веб-сервер

- Написан мини сервер на **C** и **FastCgi**, который будет возвращать простейшую страничку с надписью `Hello World!`

![simple_docker](assets/Part3_1.png)

- Написан свой *nginx.conf*, который будет проксировать все запросы с 81 порта на *127.0.0.1:8080*

![simple_docker](assets/Part3_2.png)

- Запущен контейнер
- Скопированы файлы nginx.conf и mini_server.c

![simple_docker](assets/Part3_3.png)

- Подключаемся к консоли запущенного контейнера командой `docker exec -it great_wescoff bash`
- Обновляем контейнер `apt-get update`
- Устанавливаем нужные пакеты `apt-get install gcc spawn-fcgi libfcgi-dev`
- Запуcкаем написанный мини сервер через *spawn-fcgi* на порту 8080,
командами `gcc -o server mini_server.c -lfcgi`, `spawn-fcgi -p 8080 ./server`, `nginx -s reload`

![simple_docker](assets/Part3_4.png)

- Проверено, что в браузере по *localhost:81* отдается написанная вами страничка

![simple_docker](assets/Part3_5.png)

## Part 4. Свой докер

- Написан свой докер образ, который:
  * собирает исходники мини сервера на FastCgi из предыдущей части
  * запускает его на 8080 порту
  * копирует внутрь образа написанный *./nginx/nginx.conf*
  * запускает **nginx**.

![simple_docker](assets/Part4_1.png)
![simple_docker](assets/Part4_2.png)
![simple_docker](assets/Part4_3.png)
![simple_docker](assets/Part4_4.png)

- Собираем написанный докер образ через `docker build` при этом указав имя и тег

![simple_docker](assets/Part4_5.png)

- Проверяем через `docker images`, что все собралось корректно
- Запуcкаем собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки *./nginx* внутрь контейнера по адресу, где лежат конфигурационные файлы **nginx**'а
командой `docker run -d -p 80:81 part4_build:1`

![simple_docker](assets/Part4_6.png)

- Проверяем, что по localhost:80 доступна страничка написанного мини сервера

![simple_docker](assets/Part4_7.png)

- Дописано в *./nginx/nginx.conf* проксирование странички */status*, по которой надо отдавать статус сервера **nginx**
- Проверяем, что теперь по *localhost:80/status* отдается страничка со статусом **nginx**

![simple_docker](assets/Part4_8.png)

## Part 5. **Dockle**

- Просканировали образ из предыдущего задания через `dockle [image_id|repository]`

![simple_docker](assets/Part5_1.png)

- Исправлен образ так, чтобы при проверке через **dockle** не было ошибок и предупреждений

![simple_docker](assets/Part5_2.png)
![simple_docker](assets/Part5_3.png)

## Part 6. Базовый **Docker Compose**

- Написан файл *docker-compose.yml*, с помощью которого:
  * поднимаем докер контейнер из предыдущей части _(он должен работать в локальной сети, т.е. не нужно использовать инструкцию **EXPOSE** и мапить порты на локальную машину)_ 
  * поднимаем докер контейнер с **nginx**, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера 
  * мапим 8080 порт второго контейнера на 80 порт локальной машины

![simple_docker](assets/Part6_1.png)
![simple_docker](assets/Part6_2.png)
![simple_docker](assets/Part6_3.png)
![simple_docker](assets/Part6_4.png)

- Остановили все запущенные контейнеры

![simple_docker](assets/Part6_5.png)

- Собираем и запускаем проект с помощью команд `docker-compose build` и `docker-compose up`

![simple_docker](assets/Part6_6.png)

- Проверяем, что в браузере по *localhost:80* отдается написанная вами страничка, как и ранее

![simple_docker](assets/Part6_7.png)