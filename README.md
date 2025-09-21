# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Епишин Павел`

### Задание 1

`Установил zabbix server и агента на Ubuntu 20.04 (focal) с помощью инструкции на оф. сайте'

Become root user
Start new shell session with root privileges.

$ sudo -s
b. Install Zabbix repository
Documentation
# wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu20.04_all.deb
# dpkg -i zabbix-release_latest_6.0+ubuntu20.04_all.deb
# apt update
c. Install Zabbix server, frontend, agent
# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
d. Create initial database
Documentation
Make sure you have database server up and running.

Run the following on your database host.

# mysql -uroot -p
password
mysql> create database zabbix character set utf8mb4 collate utf8mb4_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> set global log_bin_trust_function_creators = 1;
mysql> quit;
On Zabbix server host import initial schema and data. You will be prompted to enter your newly created password.

# zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
Disable log_bin_trust_function_creators option after importing database schema.

# mysql -uroot -p
password
mysql> set global log_bin_trust_function_creators = 0;
mysql> quit;
e. Configure the database for Zabbix server
Edit file /etc/zabbix/zabbix_server.conf

DBPassword=password
f. Start Zabbix server and agent processes
Start Zabbix server and agent processes and make it start at system boot.

# systemctl restart zabbix-server zabbix-agent apache2
# systemctl enable zabbix-server zabbix-agent apache2

Скриншот веб интерфейса zabbix-server
![1](ссылка на скриншот 1)`


---

### Задание 2

`Подключил второго хоста установив на него агент и изменив в конф файле адрес сервера, первый хост - это сам сервер`

Скриншот раздела Configuration > Hosts, где видно, что агенты (debian и host zabbix) подключены к серверу
![2](ссылка на скриншот 2)`

Скриншот лога zabbix agent у debian, где видно, что он работает с сервером
![3](ссылка на скриншот 2)`

Скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
![4](ссылка на скриншот 2)`