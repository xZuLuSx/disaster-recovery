Здравствуйте! Извините пожалуйста за сдачу диплома в притык. Сломался ПК и потом началась череда личных проблем.

### Сайт

Создал две виртуальные машины: web-1 - 10.0.1.7 и web-2 - 10.0.2.7 через ansible-playbook nginx.yaml

Создал Target Group и включите в нее две виртуальные машины.

Настраил healthcheck на корень (/) и порт 80, протокол HTTP. Создал HTTP router. Путь указал - /.

Создал Application load balancer для распределения трафика на web-сервера, созданные ранее.

Задал listener тип auto, порт 80.

Тестирую сайт командой curl -v <публичный IP балансера>:80

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/dip1.png)

### Мониторинг

Установил на web-1 и web-2 Node Exporter и Nginx Log Exporter через ansible-playbook nginxlog-exporter.yaml и ansible-playbook node-exporter.yaml.

Устанавливаю Prometheus на vm-prometheus -10.0.5.9 через ansible-playbook prometheus.yaml

Проверка метрик

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/metrics.png)

Создал виртуальную машину и устанавливил туда Grafana через ansible-playbook grafana.yaml

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf1.png)

Проверка и настройка взаимодействия с Prometheus

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf2.png)

Добавил дашборды Node Exporter и Nginx Log Exporter.
Далее настройка дешбордов с отображением метрик, минимальный набор - Utilization, Saturation, Errors для CPU, RAM, диски, сеть, http_response_count_total, http_response_size_bytes.
Настройка nginx_http_response_count_total

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf3.png)

Добавил необходимые tresholds на соответствующий график.
Остальные настроил по аналогии.

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf4.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf5.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf6.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf7.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf8.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf9.png)
![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/graf10.png)

Grafana работает и все видит.

### Логи

Cоздал виртуальную машину и устанавливаю Elastic через ansible-playbook elastic.yaml

Установил filebeat на виртуальные машины web-1 - 10.0.1.7 и web-2 - 10.0.2.7 через ansible-playbook filebeat.yaml

Создал виртуальную машину и установил Kibana через ansible-playbook kibana.yaml

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/elastic1.png)

Добавил Index patterns, Kibana увидела Filebeat

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/elastic2.png)

Добавил Discover для отображения логов nginx

Логи nginx за час на серверах web-1 и web-2

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/elastic3.png)

### Сеть

Создал VPC.

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/vpc1.png)

Web, Prometheus, Elasticsearch поместил в приватные подсети.

Grafana, Kibana, application load balancer, Bastion host в публичной подсети.

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/vpc2.png)

Группы безопасности

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/vpc33333.png)

Настроил Security Groups на разрешение входяшего ssh через Bastion host. Доступ через Bastion host работает

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/ssh%2022.png)

### Резервное копирование

Создаю snapshot всех 7 дисков с ограничением времени жизни snaphot в неделю и сами snaphot настроем на ежедневное копирование через terratorm манифест yandex_compute_snapshot_schedule.tf

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/copy1.png)

Расписание снимков example-schedule

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/copy2.png)


Начались создаваться snapshot. 22.00 по Москве будут создан snapshot всех дисков.

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/copy3.png)

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/copy4.png)

Спустя сутки

![alt test](https://raw.githubusercontent.com/xZuLuSx/disaster-recovery/main/img/copy5.png)
