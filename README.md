Урок 5. Docker Compose и Docker Swarm
Задание 1:
1) создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД
2) далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)
3) по итогу на каждой ноде должно быть по 2 работающих контейнера
4) выводы зафиксировать

Задание 2*:
1) нужно создать 2 ДК-файла, в которых будут описываться сервисы
2) повторить задание 1 для двух окружений: lab, dev
3) обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверки

Задание со звездочкой - повышенной сложности, это нужно учесть при выполнении (но сделать его необходимо).

Формат сдачи ДЗ: предоставить доказательства выполнения задания посредством ссылки на google-документ с правами на комментирование/редактирование.
Результатом работы будет: текст объяснения, логи выполнения, история команд и скриншоты (важно придерживаться такой последовательности).
В названии работы должны быть указаны ФИ, номер группы и номер урока.


Шаг 1 — Установка Docker Compose
Следующая команда загружает версию 1.26.0 и сохраняет исполняемый файл в каталоге /usr/local/bin/docker-compose, в результате чего данное программное обеспечение будет глобально доступно под именем docker-compose:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
Затем необходимо задать правильные разрешения, чтобы сделать команду docker-compose исполняемой:
```
sudo chmod +x /usr/local/bin/docker-compose
```
Чтобы проверить успешность установки, запустите следующую команду:
```
docker-compose --version
```

Шаг 2 — Настройка файла docker-compose.yml

Для начала создайте новый каталог в домашнем каталоге и перейдите в него:
```
mkdir ~/compose-demo
cd ~/compose-demo
```

Настройте в этом каталоге папку приложения, которая будет выступать в качестве корневого каталога документов для вашей среды Nginx:

```
mkdir app
```
![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/2.bmp)
Создайте в предпочитаемом текстовом редакторе новый файл index.html в папке app:

```
nano app/index.html
```
Вставьте в файл следующее содержимое:
```
~/compose-demo/app/index.html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Docker Compose Demo</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/kognise/water.css@latest/dist/dark.min.css">
</head>
<body>

	<h1>This is a Docker Compose Demo Page.</h1>
	<p>This content is being served by an Nginx container.</p>

</body>
</html>
```
![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/1.bmp)
Сохраните и закройте файл после завершения. Если вы использовали nano, нажмите CTRL+X, а затем Y и ENTER для подтверждения.
Затем создайте файл docker-compose.yml:

nano docker-compose.yml
Вставьте в файл docker-compose.yml следующее содержимое:

```
docker-compose.yml
version: '3.7'
services:
  web:
    image: nginx:alpine
    ports:
      - "8000:80"
    volumes:
      - ./app:/usr/share/nginx/html
```
Файл docker-compose.yml обычно начинается с определения версии. Оно показывает Docker Compose, какую версию конфигурации мы используем.

Шаг 3 — Запуск Docker Compose

Следующая команда загрузит необходимые образы Docker, создаст контейнер для службы web и запустит контейнерную среду в фоновом режиме:

```
docker-compose up -d
```

Docker Compose будет вначале искать заданный образ в локальной системе, и если не найдет его, загрузит его из Docker Hub. 


Теперь ваша среда запущена в фоновом режиме. Для проверки активности контейнера используйте следующую команду:

```
docker-compose ps
```

Эта команда покажет вам информацию о работающих контейнерах и их состоянии, а также о действующей переадресации портов.
Для получения доступа к демонстрационному приложению введите в браузере адрес localhost:8000, если оно запущено на локальном компьютере, или your_server_domain_or_IP:8000, если оно запущено на удаленном сервере.

![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/4.bmp)


Необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)

Для окружения dev создадим файл docker-compose.dev.yml с содержимым:
![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/5.bmp)

Для окружения prod создадим файл docker-compose.prod.yml с содержимым:
![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/6.bmp)

Для окружения lab создадим файл docker-compose.lab.yml с содержимым:
![lxc version](https://github.com/MariAntonova94/Containerization5/blob/main/file/7.bmp)

*Подготовила студентка GeekBrains* [*`Антонова Мария`*]