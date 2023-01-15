11.3 Домашнее задание к занятию "11.4. "Очереди RabbitMQ" - Амелин Антон

Задание 1

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad1rabbit.png)

Задание 2

Очередь hello
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad2rabbit.png)

Скрипт consumer.py

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad2cons.png)

Задание 3

Доступные ноды в кластере и включенной политикой

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad3rabbit.png)

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad31rabbit.png)

rabbit cluster_status
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/rabbitmqctl%20cluster_status.png)

rabbitmqadmin get queue='hello'
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/rabbitmqadmin%20get%20queue%3D'hello'.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/rabbitmqadmin%20get%20queue%3D'hello'2.png)

Запуск скрипта
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/zad3producer.png)

Отключаю первую ноду и запуск скрипта на 2 ноде

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/rabbitmqctl%20stop_app.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/consumerpy.png)

