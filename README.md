# Домашняя работа к занятию "Система мониторинга Zabbix"

Добавление **SSH-key**, чтобы каждый раз не вводить **Login** и **token**. Для этого нужно:

1. Добавить публичный ключ в аккаунт **GitHub**.
    * Click на изображение профиля в правом верхнем углу
    * **Settings**
    * Слева в меню пункт **SSH and GPG keys**
    * **New SSH key**
    * Ввести название ключа (*ssh-key-1759501063847.pub*)
    * Вставить содержимое ключа
    * **Add SSH key**
2. Запустить агента: 
   ````
   eval "$(ssh-agent -s)"
   ````
4. Проверить соединение:
   ````
   ssh -T git@github.com
   ````
5. Изменить адрес с https формата на SSH:
   ````
   git remote set-url origin git@github.com:smyity/Home_Work.git
   ````
    * **smyity** - имя пользователя
    * **Home_Work** - рабочая директория

6. Теперь push выполняется без пароля:
   ````
   git push origin main
   ````

# Домашнее задание к занятию "Система мониторинга Zabbix"

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

**Процесс выполнения**
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитории Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

**Требования к результатам**
1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

### Решение 1

**Процесс выполнения:**

````
sudo apt install postgresql
sudo -s
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu22.04_all.deb
apt update
apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
systemctl status zabbix-server.service
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
nano /etc/zabbix/zabbix_server.conf
````
Строка должна быть :

``DBPassword=zabbix``

```diff
-! (пароль такой же как и вводился при создании user_postgresql)
```
 
````
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
````
![](https://github.com/smyity/Home_Work/blob/main/.pic/SMK011.PNG)

### Задание 2

Установите Zabbix Agent на два хоста.

**Процесс выполнения**
1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

**Требования к результатам**
1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

### Решение 2

![](https://github.com/smyity/Home_Work/blob/main/.pic/SMK012.PNG)

![](https://github.com/smyity/Home_Work/blob/main/.pic/SMK013.PNG)

![](https://github.com/smyity/Home_Work/blob/main/.pic/SMK014.PNG)

**Процесс выполнения:**

````
sudo -s
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu22.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu22.04_all.deb
apt update
apt install zabbix-agent
systemctl restart zabbix-agent
systemctl enable zabbix-agent
nano /etc/zabbix/zabbix_agentd.conf
````
изменить строку:
Server=``<ip_zabbix_сервера>``

````
systemctl restart zabbix-agent
````

### Задание 3*

Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

**Требования к результатам**
1. Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

### Решение 3

*здесь должно быть что-то*

### Дополнительно

[Ссылка на GoogleDoc](https://docs.google.com/document/d/1jv306h8ZfcfENLwdRAxXBWHVeDdgLUuYv9pkxyZb3a0/edit?usp=drive_link)
