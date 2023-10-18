## Устанавливаем Docker

Обновляем списки пакетов

sudo apt update

Установливаем пакеты, которые позволят использовать репозиторий по HTTPS:

sudo apt install apt-transport-https ca-certificates curl software-properties-common

Добавляем ключ

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Добавляем репозиторий Docker

echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

Установливаем Docker:

sudo apt install docker-ce

sudo usermod -aG docker $vboxuser

newgrp docker

docker --version 

Запускаем контейнер с использованием образа "cowsay"

docker run docker/whalesay cowsay -f elephant "Hello, Docker!"

docker run docker/whalesay cowsay -f tux "Hello, Docker!"

docker run docker/whalesay cowsay -f dragon "Hello, Docker!"

docker run docker/whalesay cowsay -f kangaroo "Hello, Docker!"

docker run docker/whalesay cowsay -f duck "Hello, Docker!"

docker run docker/whalesay cowsay -f panda "Hello, Docker!"

docker run docker/whalesay cowsay -f owl "Hello, Docker!"

## Команды

Запуск контейнера из образа

docker run

Запуск остановленного контейнера

docker start

Остановить работающий контейнер

docker stop

Перезапустить контейнер

docker restart

Выполнить команду внутри контейнера

docker exec

Просмотр списка образов

docker images

Просмотр логов контейнера

docker logs

Просмотр списка запущенных контейнеров

docker ps

Просмотр списка всех контейнеров (включая остановленные).

docker ps -a

Удалить контейнер

docker rm

Удалить все остановленные контейнеры

docker rm $(docker ps -aq)

Загрузка образа с Docker Hub

docker pull

Сборка образа из Dockerfile

docker build

Удалить образ

docker rmi ID

## Хранение данных в контейнере

Запустим контейнер из образа Ubuntu и войдем в него

docker run -it -h GB --name gb-test ubuntu:22.10

Посмотрим содержимое корневой директории

ls -l /

Создадим новую директорию в корне

mkdir /example

Создадим файл "passwords.txt" и добавим в него какие-либо данные

touch /example/passwords.txt

echo "123test" >> /example/passwords.txt

Проверили сохранится ли наши данные при перезапуске контейнера

docker stop gb-test

docker start gb-test

docker exec -it gb-test bash

cat /example/passwords.txt

Данные сохранились, так как мы не пересоздавали контейнер

Создадим директорию и подмонтируем ее к контейнеру

mkdir /test/folder

docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

Добавим данные в подмонтированную директорию

echo "$HOSTNAME" >> /otherway/test.txt

Проверили доступность данных с локальной системы

cat /test/folder/test.txt

Удалим контейнер и создадим его снова, подмонтировав директорию

docker rm gb-test

docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10

Мы видим, что данные по-прежнему доступны

Заключение

Самый надежный способ хранения данных в контейнерах - использование внешних хранилищ. 

Важно избегать хранения важных данных внутри контейнеров, чтобы предотвратить потерю информации.

## Хранение данных в контейнерах Docker

Создаем папку, которую мы будем готовы смонтировать в контейнер

mkdir ~/docker-mount-example

В этой папке создайте файл test.txt и наполните его данными

echo "123" > ~/docker-mount-example/test.txt

В домашней директории создаем файл test.txt, который также понадобится для монтирования в контейнер, но с другим содержимым

echo "123456" > ~/test.txt

Создаем контейнер из образа ubuntu:22.10 и задайте ему имя и hostname:

docker run -it -h GB --name gb-test ubuntu:22.10

Смонтируйте ранее созданную папку с хоста в контейнер:

docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount ubuntu:22.10

Смонтируем созданный ранее текстовый файл из домашней директории внутрь смонтированной папки в контейнере

docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10

Смотрим содержимое текстового файла в контейнере:

cat /container-mount/test.txt

Объяснение

Мы создали контейнер и монтировали папку docker-mount-example внутрь контейнера. 

Затем мы монтировали файл test.txt из домашней директории внутрь этой папки в контейнере. 

При просмотре содержимого файла в контейнере, мы видим данные из файла в домашней директории.

Мы рассмотрели, как монтировать папки и файлы в контейнерах Docker, и почему изменения внутри контейнера могут повлиять на файлы хостовой системы. 

Это позволяет эффективно управлять данными в контейнерах и избегать потери информации.

## Удаление Докер 

Остановливаем службу Docker

sudo systemctl stop docker

Удаляем пакеты Docker. 

В зависимости от того, как установлен Docker, у нас может быть установлен docker, docker-engine, docker.io или containerd runc.

sudo apt-get purge docker-ce docker-ce-cli containerd.io

Если Docker из официального репозитория Docker, у нас также могут быть установлены дополнительные пакеты, такие как docker-ce-rootless-extras и docker-scan-plugin. 

Их тоже необходимо удалить 

Удаляем все оставшиеся файлы и директории

sudo rm -rf /var/lib/docker

sudo rm -rf /var/lib/containerd
