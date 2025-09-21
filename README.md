# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Епишин Павел`

### Задание 1

`Установил zabbix server и агента на Ubuntu 20.04 (focal) с помощью инструкции на оф. сайте'

1. Станьте пользователем root
Запустите новую оболочку с правами root.
$ sudo -s

2. Установите репозиторий Zabbix
Документация
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu20.04_all.deb
# dpkg -i zabbix-release_latest_6.0+ubuntu20.04_all.deb
# apt update

3. Установите сервер Zabbix, интерфейс и агент
# apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

4. Создайте исходную базу данных
Документация
Убедитесь, что сервер базы данных запущен и работает.
Выполните следующие действия на хосте вашей базы данных.
# sudo -u postgres createuser --pwprompt zabbix
# sudo -u postgres createdb -O zabbix zabbix
На сервере Zabbix импортируйте исходную схему и данные. Вам будет предложено ввести новый пароль.
# zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

5. Настройте базу данных для сервера Zabbix
Отредактируйте файл /etc/zabbix/zabbix_server.conf
DBPassword=password

6. Запустите сервер Zabbix и процессы агентов
Запустите сервер и агент Zabbix и настройте их запуск при загрузке системы.
# systemctl restart zabbix-server zabbix-agent apache2
# systemctl enable zabbix-server zabbix-agent apache2

7. Откройте веб-страницу Zabbix UI


Скриншот веб интерфейса zabbix-server
![1](https://github.com/Epishin4701/zabbix_1/blob/main/img/1.PNG)

---

### Задание 2

`Подключил второго хоста установив на него агент и изменив в конф файле адрес сервера, первый хост - это сам сервер`

Скриншот раздела Configuration > Hosts, где видно, что агенты (debian и host zabbix) подключены к серверу
![2](https://github.com/Epishin4701/zabbix_1/blob/main/img/2.PNG)

Скриншот лога zabbix agent у debian, где видно, что он работает с сервером
![3](https://github.com/Epishin4701/zabbix_1/blob/main/img/3.PNG)

Скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
![4](https://github.com/Epishin4701/zabbix_1/blob/main/img/4.PNG)