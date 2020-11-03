# Docker

## Полезные ссылочки
* https://losst.ru/ustanovka-docker-na-ubuntu-16-04
* https://stackoverflow.com/questions/48401495/docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socke
* https://stackoverflow.com/questions/52073000/how-to-remove-all-docker-containers
* https://itisgood.ru/2019/10/29/objasnenie-koncepcii-setej-v-docker/
* https://habr.com/ru/company/ruvds/blog/438796/

## Установка

Обновимся  
``` bash
sudo apt update && sudo apt upgrade
```

Перед тем как установить Docker Ubuntu 18.04 необходимо установить дополнительные пакеты ядра, которые позволяют использовать Aufs для контейнеров Docker. С помощью этой файловой системы мы сможем следить за изменениями и делать мгновенные снимки контейнеров:  
-- ХЗ на сколько это нужно  
``` bash
sudo apt install linux-image-extra-$(uname -r) linux-image-extra-virtual
```

Ещё надо установить пакеты, необходимые для работы apt по https:  
``` bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

После того как все приготовления завершены и вы убедились что ваша система полностью готова, можно перейти к установке. Мы будем устанавливать программу из официального репозитория разработчиков. Сначала надо добавить ключ репозитория:  
``` bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Затем добавьте репозиторий docker в систему:  
``` bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

``` bash
sudo apt update && apt-cache policy docker-ce
```

И установка Docker на Ubuntu:  
``` bash
sudo apt install -y docker-ce
```


Чтобы завершить установку осталось добавить нашего пользователя в группу docker. Иначе при запуске утилиты вы будете получать ошибку подключения к сокету:  
``` bash
sudo usermod -aG docker $(whoami)
```

Затем проверяем запущен ли сервис:  
``` bash
sudo systemctl status docker
```





## Пробы

hello world  
``` bash
docker run hello-world
```

## Поиск контейнеров

Список контейнеров с убунтой  
``` bash
docker search ubuntu
```

Затянем убунту  
``` bash
docker pull ubuntu
```

Текущие имеджы
``` bash
docker images
```

## Запуск контейнера

Запустим с консолью (-it иди -i -t)
``` bash
docker run -it ubuntu
```


Внутри можно что-нибудь поставить
``` bash
apt update
apt install python3
```

## Превращения контейнера в новый образ

Список запущенных контейнеров
``` bash
docker ps
```
а можно непрерывно
``` bash
watch -n 2 -c docker ps
```
а можно всех в том числе и старых
``` bash
docker ps -a
```


Теперь можно создать собственный образ
``` bash
docker commit -m "изменения" -a "автор" ид_контейнера repository/имя
```
например
``` bash
docker commit -m "ssh" -a "sch" 6f5f57f979a4 repository/ubuntu-ssh
```
посмотреть на дело рук своих (список имеджей или образов)
``` bash
docker images
```
удалить образ
``` bash
docker image rm repository/ubuntu-ssh
```

## Работа с контейнерами

удалить контейнер
``` bash
docker rm 7aef52950ba8
```

остановить контейнер
``` bash
docker stop 7aef52950ba8
```
запустить контейнер
``` bash
docker start 7aef52950ba8
```
подключиться к контейнеру
``` bash
docker attach 7aef52950ba8
```


удалить все выключенные контейнеры
``` bash
docker container prune -f
```

остановить у удалить все контейнеры
``` bash
docker container stop $(docker container ls -aq)
docker container prune -f

```

## Докер и сеть

инфа о сетях в докере
``` bash
docker network ls
```

инфа о бридж подключении в докере (очень много инфы)
``` bash
docker network inspect bridge
```

Важно что к контейнерам можно достучаться по сети с хотовой машины.  

