# Docker常用命令
`docker pull image_name`：下载镜像
`docker ps`：列出正在运行中的容器
`docker ps ‑a`：列出所有容器（包括未运行的）
`docker stop container_id`：停止运行容器
`docker start container_id`：启动容器
`docker restart container_id`：重启容器
`docker rm container_id`：删除容器
`docker rm ‑f container_id`：删除正在运行中容器
`docker rmi image_id`：删除镜像

# Docker镜像和容器概念
`image（镜像）`文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。`容器`简单来说容器为镜像的运行实例，Docker 容器是对 Linux容器（LXC）的封装

# Docker安装基础服务
## Redis
`docker pull redis`
`docker run --name some-redis -d redis`
`docker exec -ti {container_id} redis-cli`

## Zookeeper
`docker pull zookeeper`
`docker run -p 2181:2181 --name some-zookeeper --restart always -d zookeeper`
`docker run -it --rm --link some-zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper`

## MariaDB
`docker run -p 127.0.0.1:3306:3306  --name {container_name} -e MARIADB_ROOT_PASSWORD={password} -d mariadb:{tag}`
`docker exec -it {container_name} bash`
`mysql -h localhost -u root -p`

## Prometheus
`docker run --name prometheus -d -p 127.0.0.1:9090:9090 -v {path-to-prometheus.yml}:/etc/prometheus/prometheus.yml prom/prometheus`
```prometheus.yml
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s
alerting:
  alertmanagers:
  - follow_redirects: true
    scheme: http
    timeout: 10s
    api_version: v2
    static_configs:
    - targets: []
scrape_configs:
- job_name: prometheus
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  follow_redirects: true
  static_configs:
  - targets:
    - localhost:9090
- job_name: node
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  follow_redirects: true
  static_configs:
  - targets:
    - 10.224.104.150:9100
```

node_exporter
`sudo docker run --name node_exporter --network="host" --pid="host" -v "/:/host:ro" prom/node-exporter:v1.0.0 --path.rootfs="/host"` (不支持 macos)

Grafana
`docker run --name grafana -d -p 3000:3000 grafana/grafana`
https://grafana.com/grafana/dashboards/8919

## RabbitMQ
`docker run -d --hostname rabbit-host1 --name rabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=123456 -p 15672:15672 -p 5672:5672 -p 25672:25672 -p 61613:61613 -p 1883:1883 rabbitmq:management`