# Домашнее задание к занятию "`Очереди RabbitMQ`" - `Рыбянцев Павел`

---

### Задание 1. Установка RabbitMQ

Используя Vagrant или VirtualBox, создайте виртуальную машину и установите RabbitMQ.
Добавьте management plug-in и зайдите в веб-интерфейс.

*Итогом выполнения домашнего задания будет приложенный скриншот веб-интерфейса RabbitMQ.*
![0](image.png)

---

### Задание 2. Отправка и получение сообщений

Используя приложенные скрипты, проведите тестовую отправку и получение сообщения.
Для отправки сообщений необходимо запустить скрипт producer.py.

Для работы скриптов вам необходимо установить Python версии 3 и библиотеку Pika.
Также в скриптах нужно указать IP-адрес машины, на которой запущен RabbitMQ, заменив localhost на нужный IP.

```shell script
$ pip install pika
```

Зайдите в веб-интерфейс, найдите очередь под названием hello и сделайте скриншот.
После чего запустите второй скрипт consumer.py и сделайте скриншот результата выполнения скрипта

*В качестве решения домашнего задания приложите оба скриншота, сделанных на этапе выполнения.*
![1](image-1.png)
![2](image-2.png)

Для закрепления материала можете попробовать модифицировать скрипты, чтобы поменять название очереди и отправляемое сообщение.

---

### Задание 3. Подготовка HA кластера

Используя Vagrant или VirtualBox, создайте вторую виртуальную машину и установите RabbitMQ.
Добавьте в файл hosts название и IP-адрес каждой машины, чтобы машины могли видеть друг друга по имени.

Пример содержимого hosts файла:
```shell script
$ cat /etc/hosts
192.168.0.10 rmq01
192.168.0.11 rmq02
```
После этого ваши машины могут пинговаться по имени.

Затем объедините две машины в кластер и создайте политику ha-all на все очереди.

*В качестве решения домашнего задания приложите скриншоты из веб-интерфейса с информацией о доступных нодах в кластере и включённой политикой.*
![3](image-3.png)
Также приложите вывод команды с двух нод:

```shell script
$ rabbitmqctl cluster_status
```
1 нода:
```
(venv) pavel@ubuntu-24:~/hw/RabbitMQ-hw$ docker exec -it rabbitmq1 rabbitmqctl cluster_status
Cluster status of node rabbit@rabbit1 ...
Basics

Cluster name: rabbit@rabbit1
Total CPU cores available cluster-wide: 3

Disk Nodes

rabbit@rabbit1
rabbit@rabbit2

Running Nodes

rabbit@rabbit1
rabbit@rabbit2

Versions

rabbit@rabbit1: RabbitMQ 3.13.7 on Erlang 26.2.5.16
rabbit@rabbit2: RabbitMQ 3.13.7 on Erlang 26.2.5.16

CPU Cores

Node: rabbit@rabbit1, available CPU cores: 2
Node: rabbit@rabbit2, available CPU cores: 1

Maintenance status

Node: rabbit@rabbit1, status: not under maintenance
Node: rabbit@rabbit2, status: not under maintenance

Alarms

(none)

Network Partitions

(none)

Listeners

Node: rabbit@rabbit1, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit1, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit1, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit1, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit@rabbit2, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit2, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit2, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit2, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

Feature flags

Flag: classic_mirrored_queue_version, state: enabled
Flag: classic_queue_type_delivery_support, state: enabled
Flag: detailed_queues_endpoint, state: enabled
Flag: direct_exchange_routing_v2, state: enabled
Flag: drop_unroutable_metric, state: enabled
Flag: empty_basic_get_metric, state: enabled
Flag: feature_flags_v2, state: enabled
Flag: implicit_default_bindings, state: enabled
Flag: khepri_db, state: disabled
Flag: listener_records_in_ets, state: enabled
Flag: maintenance_mode_status, state: enabled
Flag: message_containers, state: enabled
Flag: message_containers_deaths_v2, state: enabled
Flag: quorum_queue, state: enabled
Flag: quorum_queue_non_voters, state: enabled
Flag: restart_streams, state: enabled
Flag: stream_filtering, state: enabled
Flag: stream_queue, state: enabled
Flag: stream_sac_coordinator_unblock_group, state: enabled
Flag: stream_single_active_consumer, state: enabled
Flag: stream_update_config_command, state: enabled
Flag: tracking_records_in_ets, state: enabled
Flag: user_limits, state: enabled
Flag: virtual_host_metadata, state: enabled
(venv) pavel@ubuntu-24:~/hw/RabbitMQ-hw$ 
```

2 нода:
```
pavel@web-1:~/RabbitMQ$ docker exec -it rabbitmq2 rabbitmqctl cluster_status
Cluster status of node rabbit@rabbit2 ...
Basics

Cluster name: rabbit@rabbit2
Total CPU cores available cluster-wide: 3

Disk Nodes

rabbit@rabbit1
rabbit@rabbit2

Running Nodes

rabbit@rabbit1
rabbit@rabbit2

Versions

rabbit@rabbit2: RabbitMQ 3.13.7 on Erlang 26.2.5.16
rabbit@rabbit1: RabbitMQ 3.13.7 on Erlang 26.2.5.16

CPU Cores

Node: rabbit@rabbit2, available CPU cores: 1
Node: rabbit@rabbit1, available CPU cores: 2

Maintenance status

Node: rabbit@rabbit2, status: not under maintenance
Node: rabbit@rabbit1, status: not under maintenance

Alarms

(none)

Network Partitions

(none)

Listeners

Node: rabbit@rabbit2, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit2, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit2, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit2, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0
Node: rabbit@rabbit1, interface: [::], port: 15672, protocol: http, purpose: HTTP API
Node: rabbit@rabbit1, interface: [::], port: 15692, protocol: http/prometheus, purpose: Prometheus exporter API over HTTP
Node: rabbit@rabbit1, interface: [::], port: 25672, protocol: clustering, purpose: inter-node and CLI tool communication
Node: rabbit@rabbit1, interface: [::], port: 5672, protocol: amqp, purpose: AMQP 0-9-1 and AMQP 1.0

Feature flags

Flag: classic_mirrored_queue_version, state: enabled
Flag: classic_queue_type_delivery_support, state: enabled
Flag: detailed_queues_endpoint, state: enabled
Flag: direct_exchange_routing_v2, state: enabled
Flag: drop_unroutable_metric, state: enabled
Flag: empty_basic_get_metric, state: enabled
Flag: feature_flags_v2, state: enabled
Flag: implicit_default_bindings, state: enabled
Flag: khepri_db, state: disabled
Flag: listener_records_in_ets, state: enabled
Flag: maintenance_mode_status, state: enabled
Flag: message_containers, state: enabled
Flag: message_containers_deaths_v2, state: enabled
Flag: quorum_queue, state: enabled
Flag: quorum_queue_non_voters, state: enabled
Flag: restart_streams, state: enabled
Flag: stream_filtering, state: enabled
Flag: stream_queue, state: enabled
Flag: stream_sac_coordinator_unblock_group, state: enabled
Flag: stream_single_active_consumer, state: enabled
Flag: stream_update_config_command, state: enabled
Flag: tracking_records_in_ets, state: enabled
Flag: user_limits, state: enabled
Flag: virtual_host_metadata, state: enabled
pavel@web-1:~/RabbitMQ$ 
```

Для закрепления материала снова запустите скрипт producer.py и приложите скриншот выполнения команды на каждой из нод:

```shell script
$ rabbitmqadmin get queue='hello'
```

После чего попробуйте отключить одну из нод, желательно ту, к которой подключались из скрипта, затем поправьте параметры подключения в скрипте consumer.py на вторую ноду и запустите его.

*Приложите скриншот результата работы второго скрипта.*


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### * Задание 4. Ansible playbook

Напишите плейбук, который будет производить установку RabbitMQ на любое количество нод и объединять их в кластер.
При этом будет автоматически создавать политику ha-all.

*Готовый плейбук разместите в своём репозитории.*
