# Домашнее задание к занятию «Docker. Часть 2»

### Боробов И.С.

---

### Задание 1

**Напишите ответ в свободной форме, не больше одного абзаца текста.**

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.

### Ответ:
Docker Compose - это надстройка над докером, которая позволяет запускать множество контейнеров одновременно и маршрутизировать потоки данных между ними. Упрощает жизнь администратора системы, позволяя разворачивать комплексные системы из одного конфигурационного файла.  

---

### Задание 2 

**Выполните действия и приложите текст конфига на этом этапе.** 

Создайте файл docker-compose.yml и внесите туда первичные настройки: 

 * version;
 * services;
 * networks.

При выполнении задания используйте подсеть 172.22.0.0.
Ваша подсеть должна называться: <ваши фамилия и инициалы>-my-netology-hw.

### Ответ:

![img-6-04-1](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-1.png)

---

### Задание 3 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите PostgreSQL с именем контейнера <ваши фамилия и инициалы>-netology-db. 
2. Предсоздайте БД <ваши фамилия и инициалы>-db.
3. Задайте пароль пользователя postgres, как <ваши фамилия и инициалы>12!3!!
4. Пример названия контейнера: ivanovii-netology-db.
5. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

### Ответ:


![img-6-04-2](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-2.png)

---

### Задание 4 

**Выполните действия:**

1. Установите pgAdmin с именем контейнера <ваши фамилия и инициалы>-pgadmin. 
2. Задайте логин администратора pgAdmin <ваши фамилия и инициалы>@ilove-netology.com и пароль на выбор.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.
4. Прокиньте на 80 порт контейнера порт 61231.

В качестве решения приложите:

* текст конфига текущего сервиса;
* скриншот админки pgAdmin.

### Ответ:


![img-6-04-3](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-3.png)

![img-6-04-4](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-4.png)

---

### Задание 5 

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Server с именем контейнера <ваши фамилия и инициалы>-zabbix-netology. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

### Ответ:


![img-6-04-5](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-5.png)

---

### Задание 6

**Выполните действия и приложите текст конфига текущего сервиса:** 

1. Установите Zabbix Frontend с именем контейнера <ваши фамилия и инициалы>-netology-zabbix-frontend. 
2. Настройте его подключение к вашему СУБД.
3. Назначьте для данного контейнера статический IP из подсети 172.22.0.0/24.

### Ответ:


![img-6-04-6](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-6.png)

---

### Задание 7 

**Выполните действия.**

Настройте линки, чтобы контейнеры запускались только в момент, когда запущены контейнеры, от которых они зависят.

В качестве решения приложите:

* текст конфига **целиком**;
* скриншот команды docker ps;
* скриншот авторизации в админке Zabbix.

### Ответ:

```
version: "3"

services:
  netology-db:
	image: postgres:latest
	container_name: borobovis-netology-db
	ports:
  	- 5432:5432
	volumes:
  	- ./pg_data:/var/lib/postgresql/data/pgdata
	environment:
  	POSTGRES_PASSWORD: borobovis12!3!!
  	POSTGRES_DB: borobovis-db
  	PGDATA: /var/lib/postgresql/data/pgdata
	networks:
  	borobovis-my-netology-hw:
    	ipv4_address: 172.22.0.10
	restart: always

  pgadmin:
	image: dpage/pgadmin4
	links:
  	- netology-db
	container_name: borobovis-pgadmin
	environment:
  	PGADMIN_DEFAULT_EMAIL: borobovis@ilove-netology.com
  	PGADMIN_DEFAULT_PASSWORD: 789456
	ports:
  	- "61231:80"
	networks:
  	borobovis-my-netology-hw:
    	ipv4_address: 172.22.0.11
	restart: always

  zabbix-server:
	image: zabbix/zabbix-server-pgsql
	links:
  	- netology-db
	container_name: borobovis-zabbix-netology
	environment:
  	DB_SERVER_HOST: '172.22.0.10'
  	POSTGRES_USER: postgres
  	POSTGRES_PASSWORD: borobovis12!3!!
	ports:
  	- "10051:10051"
	networks:
  	borobovis-my-netology-hw:
    	ipv4_address: 172.22.0.12
	restart: always

  zabbix-web:
	image: zabbix/zabbix-web-apache-pgsql
	links:
  	- zabbix-server
	container_name: borobovis-netology-zabbix-frontend
	environment:
  	DB_SERVER_HOST: '172.22.0.10'
  	POSTGRES_USER: 'postgres'
  	POSTGRES_PASSWORD: borobovis12!3!!
  	ZBX_SERVER_HOST: "zabbix-web"
  	PHP_TZ: "Europe/Moscow"
	ports:
  	- "80:8080"
  	- "443:8443"
	networks:
  	borobovis-my-netology-hw:
    	ipv4_address: 172.22.0.13
	restart: always

networks:
  borobovis-my-netology-hw:
	driver: bridge
	ipam:
  	config:
  	- subnet: 172.22.0.0/24
```

docker ps  

![img-6-04-7](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-7.png)

![img-6-04-8](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-8.png)

---

### Задание 8 

**Выполните действия:** 

1. Убейте все контейнеры и потом удалите их.
1. Приложите скриншот консоли с проделанными действиями.

### Ответ:

docker-compose down  

![img-6-04-9](https://github.com/Borobov/03-Virtualization-automation-and-CICD/blob/450d322bbbf2c6993871fb0fc39ca7c220d898ed/img-6-04/img-6-04-9.png)

---

## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 9* 

Запустите свой сценарий на чистом железе без предзагруженных образов.

**Ответьте на вопросы в свободной форме:**

1. Сколько ушло времени на то, чтобы развернуть на чистом железе написанный вами сценарий?
2. Чем вы занимались в процессе создания сценария так, как это видите вы?
3. Что бы вы улучшили в сценарии развёртывания?

### Ответ:

1.	Процесс занял на моей виртуальной машине около 2 минут.  
2.	Я описывал порядок создания сервисов необходимых для работы конечного приложения (в нашем случае Zabbix), устанавливал основные параметры взаимодействия сервисов (указывал адреса и порты, логины/пароли).  
3.	Я думаю стоит вынести каталог /var/lib/zabbix/ за пределы контейнера Zabbix сервера, так как там могут содержаться данные, которые было бы хорошо сохранить при пересоздании контейнера. Например параметры SSL.  
