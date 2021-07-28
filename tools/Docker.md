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

# Docker安装基础服务
## Redis
`docker pull redis`
`docker run --name some-redis -d redis`
`docker exec -ti {container_id} redis-cli`

## Zookeeper
`docker pull zookeeper`
`docker run -p 2181:2181 --name some-zookeeper --restart always -d zookeeper`
`docker run -it --rm --link some-zookeeper:zookeeper zookeeper zkCli.sh -server zookeeper`