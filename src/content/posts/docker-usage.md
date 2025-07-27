---
title: Docker使用教程
published: 2025-06-30
description: 'docker基础以及一些我常用的镜像'
image: 'https://teleimg.loophy.top/file/1753610411630_image.png'
tags: [Docker, Linux]
category: '教程'
draft: false 
lang: ''
---

## 基础命令

安装有自动化脚本 安装1Panel就行

```
docker ps <-a>
docker rm -f xx (force)
docker rmi xx (rm image)
```

```
docker run -it --rm alpine (用于调试)
--restart
--unless-stopped

docker run -d -p 80:80 -e xxx=xxx -e yy=yy nginx
docker create/stop/start/logs -f

docker logs name

docker exec id (ps aux)
docker exec -it name bash

docker run -d --network host nginx
```

Macvlan 和 Ipvlan

```
docker network create -d ipvlan \
    --subnet=192.168.1.0/24 \
    --ip-range=192.168.1.128/25 \
    --gateway=192.168.1.1 \
    --opt parent=eth0 \
    macvlan1

docker network rm macvlan

docker network create -d macvlan \
  	--subnet=10.31.0.0/24 \
    --ip-range=10.31.0.128/25 \
    --gateway=10.31.0.1 \
    --opt parent=eth0.10 \
    macvlan1
```

Dockerfile

```
docker compose up/stop/start/down (自动创建network
docker compose -f file.yml up -d
```

