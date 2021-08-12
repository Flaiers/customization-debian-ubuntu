Customization for debian or ubuntu server. Priority versions for Debian 10.+ and Ubuntu 20.+
============================================================================================

1. ### [Project structure](#general-structure-of-the-project)

2. ### [Build Django](#building-django-project)
    * [Description](#description-for-build-django-project)
    * [Run script](#use-my-script-buildsh)
    * [Final tree](#final-tree-of-created-project)

3. ### [Network](#customization-the-network)
    * [Network device](#first-lets-find-out-the-parameters-and-network-device-on-our-server)
    * [Edit parameters](#lets-start-customization-open-the-file-for-editing)

4. ### [Dependencies](#installing-dependencies)
    * [Apt update upgrade](#first-lets-update-the-apt-packages-and-then-the-system)
    * [Install packages](#lets-download-the-main-packages-for-python-development-and-for-further-customization)
    * [Use oh-my-zsh](#install-and-use-oh-my-zsh)

5. ### [Users](#working-with-users)
    * [Create users](#create-users-with-different-rights)
    * [Delete users](#delete-users)

6. ### [SSH](#working-and-use-ssh)
    * [Add to startup](#add-openssh-server-to-startup)
    * [Check functionality](#checking-functionality-of-the-utility)
    * [Generate rsa keys](#generate-and-upload-a-pair-of-rsa-keys-to-server)
    * [Customization ssh](#customization-ssh-config)
    * [Save and restart](#save-file-etcsshsshd_config-and-restart-ssh)
    * [Connection](#config-ssh-connections)

7. ### [VPN](#install-and-setup-openvpn)
    * [Install](#download-shell-script-for-installation-openvpn)
    * [Add to startup](#starting-and-add-to-startup)
    * [Connect](#connecting-from-local-computer)

&nbsp;

General structure of the project
--------------------------------

123

&nbsp;

Building Django project
-----------------------

### Description for build Django project

> Описание для сборки Django проекта

To build and raise project you need to:

1. Run shell script that creates and initially configures a Django project

2. To raise project in production mode, you need to select a database, example is PostgreSQL and MySQL. Docker compose files is in repository. You need to move the content in `services` to main file with instructions (`django.yml`) necessary to start and configure the services

3. 123

> Для того чтобы собрать и поднять проект вам нужно:
>
> 1. Запустить shell скрипт который создает и первично настраивает Django проект
>
> 2. Для поднятия проекта в продакшн режиме вам нужно выбрать базу данных, в пример приведены PostgreSQL и MySQL. Docker compose файлы есть в репозитории. Нужно содержимое в `services` переместить в главный файл с инструкциями (`django.yml`) необходимыми для запуска и настройки сервисов
>
> 3. 123

***

### Use my script `build.sh`

> Используйте мой скрипт `build.sh`

```bash
sudo chmod a+x build.sh ; \
./build.sh
```

The first thing to choose is the path to interpreter Python. If your path to interpreter is `/usr/bin/python3`, click Enter. Else write your `/path/to/python` and click Enter

> Первое что нужно выбрать это путь до интерпретатора Python. Если ваш путь до интерпретатора `/usr/bin/python3`, нажмите Enter. Иначе напишите ваш `/path/to/python` и нажмите Enter

```
----------------------
| Python interpreter |
----------------------
Default: /usr/bin/python3
(Сlick Enter for choose default)
If you wont to change, write: /path/to/python
```

The second thing to choose is the name of first Django app. If you want first app with a name `core`, click Enter. Else write your name `my_app` and click Enter

> Второе что нужно выбрать это название первого приложения Django. Если вы хотите первое приложение с названием `core`, нажмите Enter. Иначе напишите ваше название `my_app` и нажмите Enter

```
------------------
| First app name |
------------------
Default: core
(Сlick Enter for choose default)
If you wont to change, write: my_app
```

***

### Final tree of created project

> Финальное дерево созданного проекта

```
.
├── build.sh
├── db.sqlite3
├── django.yml
├── Dockerfile
├── LICENSE.md
├── mysql.yml
├── nginx
│   ├── letsencrypt
│   │   └── __init__
│   └── nginx.conf
├── psql.yml
├── README.md
├── requirements.txt
├── src
│   ├── config
│   │   ├── asgi.py
│   │   ├── __init__.py
│   │   ├── settings
│   │   │   ├── base.py
│   │   │   ├── __init__.py
│   │   │   ├── local.py
│   │   │   └── production.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   ├── core
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── __init__.py
│   │   ├── migrations
│   │   │   └── __init__.py
│   │   ├── models.py
│   │   ├── tests.py
│   │   └── views.py
│   └── manage.py
└── static

8 directories, 27 files
```

&nbsp;

Customization the Network
-------------------------

### First, let's find out the parameters and network device on our server

> Для начала узнаем параметры и сетевое устройство на нашем сервере

```bash
sudo lshw -class network
```

From the response, you need a parameter `*-network` then the next parameter `logical name` write it down or remember it

> Из ответа нужен параметр `*-network` следующий параметр `logical name` запишите или запомните его

***

### Let's start customization, open the file for editing

> Приступаем к настройке, открываем для редактирования файл

```bash
sudo nano /etc/network/interfaces
```

Write the code in this file with their parameters

> Записываем в файл данный код, со своими парметрами

```bash
iface lo inet loopback
auto lo

auto <logical name>
iface <logical name> inet static
    address <ip>
    netmask 255.255.255.0
    gateway <ip gateway>
    dns-nameservers 8.8.8.8
    metric 0
```

Reboot the server

> Перезагружаем сервер

```bash
sudo reboot
```

&nbsp;

Installing Dependencies
-----------------------

### First, let's update the apt packages and then the system

> Для начала обновим apt пакеты и затем систему

```bash
sudo apt update ; \
sudo apt upgrade
```

***

### Let's download the main packages for python development and for further customization

> Скачиваем основные пакеты для разработки на python и для дальнейшей настройки

```bash
sudo apt install -y openssh-server vim zsh mosh nginx htop git curl wget unzip zip make python3-dev python3-lxml supervisor python3 build-essential libssl-dev libffi-dev python3-pip python3-venv tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libbz2-dev libexpat1-dev liblzma-dev zlib1g-dev
```

***

### Install and use oh-my-zsh

> Установка и использование конфига oh-my-zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" ; \
zsh
```

&nbsp;

Working with users
------------------

### Create users with different rights

> Создание пользователей с разными правами

Create standard user without rights

> Создание обычного пользователя без прав

```bash
sudo useradd -m -G sudo,wheel -s /bin/bash -p <password> <username>
```

Create user with group and rights root

> Создание пользователя с группой и правами root

```bash
sudo useradd -m -o -u 0 -g 0 -s /bin/bash -p <password> <username>
```

To see the settings of the rights of groups and users

> Посмотреть настройки прав групп и пользователей

```bash
sudo nano /etc/sudoers
```

***

### Delete users

> Удаление пользователей

Flags: -f (--force) for forced deletion, -r (--remove) for remove home directory

> Флаги: -f для принудительного удаления, -r для удаления домашней дериктории

```bash
userdel -f -r <username>
```

&nbsp;

Working and use SSH
-------------------

### Add openssh-server to startup 

> Добавим openssh-server в автозагрузку

```bash
sudo systemctl enable ssh
```

***

### Checking functionality of the utility

> Проверяем работоспособность утилиты

```bash
ssh localhost
```

***

### Generate and upload a pair of rsa keys to server

> Сгенерировать и загрузить на сервер пару rsa ключей

```bash
ssh-keygen -t rsa
```

```bash
ssh-copy-id -i ~/.ssh/<id_rsa> -p <port> <user>@<host>
```

***

### Customization ssh config

> Настрока ssh конфига

Copying the default config

> Копирование стандартного конфига

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.default
```

Let's start editing

> Начинаем редактирование

```bash
sudo nano /etc/ssh/sshd_config
```

Check the most important parameters in config

> Проверьте наиболее важные параметры  в конфиге

```bash
Port 2222
LoginGraceTime 2m
AllowUsers <username>, <username>
PermitRootLogin no
PubkeyAuthentication yes
ClientAliveInterval 600
ClientAliveCountMax 0
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no
```

> In the parameter AllowUsers, specify a user or several users to which it is allowed to connect

> В параметре AllowUsers укажите пользователя или несколько пользователей к которым разрешено подключаться

***

### Save file `/etc/ssh/sshd_config` and restart ssh

> Сохраняем файл `/etc/ssh/sshd_config` и перезагружаем ssh

```bash
systemctl restart ssh
```

***

### Config ssh connections

> Настроим ssh соединения

On your local computer in the `~/.ssh` folder create and open file

> На вашем локальном компьютере в папке `~/.ssh` создаем и откываем файл

```bash
sudo nano ~/.ssh/config
```

Write to a file according to this structure with our data

> Записываем в файл по данной структуре со своими данными

```bash
Host <name_connection>
Hostname <ip_address>
User <username>
PubKeyAuthentication yes
Port <port>
```

Save file `~/.ssh/config` and connect

> Сохраняем файл `~/.ssh/config` и подключаемся

```bash
ssh <name_connection>
```

&nbsp;

Install and setup OpenVPN
-------------------------

### Download shell script for installation OpenVPN

> Cкачаем shell скрипт для установки OpenVPN

I inherit the installation commands from this guide:
[https://github.com/Nyr/openvpn-install](https://github.com/Nyr/openvpn-install "Customization for install OpenVPN")

> Я наследую команды установки из этого руководства

Use this command for download and run script, then proceed as directed

> Используйте данную команды для загрузки и запуска скрипта, затем действуйте по указаниям

```bash
wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh
```

As a result, you will receive a file in directory `/root/<client>.ovpn`. This configuration file connects to VPN

> По результату вы получите файл в деритории `/root/<client>.ovpn`. Этот файл конфигурации подключения к VPN

***

### Starting and add to startup

> Запускаем и включаем в автозагрузку

```bash
sudo systemctl start openvpn ; \
sudo systemctl enable openvpn
```

***

### Connecting from local computer

> Подключение с локального компьютера

I have OS Linux/Ubuntu and will demonstrate on it

> Я имею операционную систему Linux/Ubuntu и буду демонстрировать на ней

![VPN](https://ltdfoto.ru/images/VPN.png)

Сlick: Import from file... And choose your file `<client>.ovpn`. As a result, you will receive a complete setting for adding VPN. Now click Add

> Нажмите: Импортировать из файла... И выбирите ваш файл `<client>.ovpn`. ПО итогу вы получите полную настройку для добавления VPN. Теперь нажмите: Добавить

![Add VPN](https://ltdfoto.ru/images/add-vpn.png)

Successfully!

> Успешно!
