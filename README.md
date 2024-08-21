# Домашнее задание к занятию "Cистема мониторинга Zabbix" - Артем Пестроухов

---

## Задание 1 

Установите Zabbix Server с веб-интерфейсом.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результатам 
1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

### Решение 1

![страница входа в панель zabbix](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-1.png)

Введённые команды для установки zabbix_server:

```
sudo wget https://repo.zabbix.com/zabbix/5.0/debian/pool/main/z/zabbix-release/zabbix-release_5.0-2+debian11_all.deb
sudo dpkg -i zabbix-release_5.0-2+debian11_all.deb
sudo apt update #установил репозитории и обновил apt

sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-agent postgresql

sudo su
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/doc/zabbix-server-pgsql*/create.sql.gz | sudo -u zabbix psql zabbix # добавил пользователя и БД zabbix, заполнил шаблон

gedit /etc/zabbix/zabbix_server.conf # поставил пароль для БД
gedit /etc/zabbix/apache.conf # установил часовой пояс

systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

### Задание 2 

Установите Zabbix Agent на два хоста.

#### Процесс выполнения
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результатам 
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

### Решение 2

![configuration > hosts](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-2.1.png)
![логи zabbix agent](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-2.2.png)
![последние логи ВМ Ubuntu](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-2.3.png)
![последние логи ВМ Debian](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-2.4.png)

Команды на агентах:

```
sudo su
wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-2+ubuntu22.04_all.deb
dpkg -i zabbix-release_5.0-2+ubuntu22.04_all.deb
apt update

apt install zabbix-agent

gedit /etc/zabbix/zabbix_agentd.conf # для настройки конфига агента

systemctl restart zabbix-agent
systemctl enable zabbix-agent

```

---

## Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

#### Требования к результаты 
1. Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

### Решение 3

![Windows Latest Data](https://github.com/ArtemPestro/git-hw/blob/zabbix-1/img/zabbix-3.png)

---

