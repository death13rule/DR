### Задание 1

Выполнена настройка трекинга для 1-ой группы маршрутизаторов. Скриншоты схемы, а так же конфигурации standby обоих маршрутизаторов на скриншотах ниже.

![alt text](https://github.com/death13rule/screenshots/blob/96c25693cebdaa25330f53308f787615aa53baf9/Schema.png)

![alt text](https://github.com/death13rule/screenshots/blob/96c25693cebdaa25330f53308f787615aa53baf9/Router1.png)

![alt text](https://github.com/death13rule/screenshots/blob/96c25693cebdaa25330f53308f787615aa53baf9/Router2.png)


### Задание 2

Был  написан следующий скрипт: 

```
!/bin/bash

PORT=80
FILE="/var/www/html/index.nginx-debian.html"

# Проверка порта
nc -z localhost $PORT
PORT_STATUS=$?

# Проверка файла
if [ -f "$FILE" ]; then
    FILE_STATUS=0
else
    FILE_STATUS=1
fi

# Если не работает — вернуть ошибку
if [ $PORT_STATUS -ne 0 ] || [ $FILE_STATUS -ne 0 ]; then
    exit 1
else
    exit 0
fi
```

Файл keepalived.conf сконфигурирован следующим образом:

```
vrrp_script check_web {
    script "/etc/keepalived/check_web.sh"
    interval 3
}

vrrp_instance VI_1 {
        state MASTER
        interface enp0s3
        virtual_router_id 210
        priority 255
        advert_int 1

        virtual_ipaddress {
              192.168.31.210/24
        }

        track_script {
                check_web
        }

}
```

На бэкапе конфиг аналогичный, но с более низким приоритетом.


Результат работы на скриншоте:

![alt text](https://github.com/death13rule/screenshots/blob/17e36e53080c828a49dbcc27f724e279036e2b65/keepalived.png)


