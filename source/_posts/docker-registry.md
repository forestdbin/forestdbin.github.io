---
title: Docker Registry
date: 2025-05-27 20:47:22
tags:
  - container
categories:
  - container
---

[Mirror the Docker Hub library](https://docs.docker.com/docker-hub/image-library/mirror/)

## 重新打标签

```bash
docker pull <your-registry-mirror>[:<port>]/library/busybox
docker tag <your-registry-mirror>[:<port>]/library/busybox:latest busybox:latest
```

## 运行一个Registry

[Registry image](https://hub.docker.com/_/registry)

```bash
$ docker pull registry
$ docker run -d -p 5000:5000 --restart always --name registry registry:2
$ docker pull ubuntu
$ docker tag ubuntu localhost:5000/ubuntu
$ docker push localhost:5000/ubuntu
```

## pull-through cache

```bash
# /etc/docker/registry/config.yml
proxy:
  remoteurl: https://registry-1.docker.io
```

## 配置Docker daemon

```bash
# /etc/docker/daemon.json
{
  "registry-mirrors": ["https://<my-docker-mirror-host>"]
}

# sudo systemctl daemon-reload
# sudo systemctl restart docker
```

## 镜像加速

[镜像加速服务状态监控](https://status.daocloud.io/status/docker)

[DaoCloud](https://github.com/DaoCloud/public-image-mirror)

[1panel](https://hub.1panel.dev)

[渡渡鸟](https://docker.aityp.com/#google_vignette)
