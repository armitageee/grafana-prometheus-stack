# grafana-docker-stack

Для деплоя Grafana, Prometheus и Node Exporter, нам понадобится:
1. Clone repo
```
git clone https://github.com/armitageee/grafana-prometheus-stack
```
2. Поменять конфиг (at least `configs/alertmanager.yml` для уведомлений в тг боте)
3. Deploy stack
```bash
docker stack deploy -c grafana-docker-stack/docker-compose.yml monitoring
```


Если требуется разместить Node-exporter на нескольких хостах и отслеживать их:
1. Clone repo:
```
git clone https://github.com/armitageee/grafana-prometheus-stack

```
Перейти в ветку node-exporter и запустить docker-compose файл::
```bash
git checkout node-exporter
cd node-exporter
docker-compose up 
```
2. Добавить хосты в файле  `configs/prometheus/prometheus.yml` в `- targets: ['node-exporter:9100']` список jobs `- job_name: 'node-exporter'` как здесь: `- targets: ['node-exporter:9100', 'server1:9100', 'server2:9100', '...']`
3. Обновить на лету конфиг прометеуса:
```bash
docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP
```
