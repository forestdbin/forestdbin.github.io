---
title: 生成容器镜像的几种方式
date: 2025-11-04 18:46:01
tags:
  - container
categories:
---

## commit

**commit**可能是最简单直白的方法。

1. 运行一个容器

```bash
$ docker run \
    -it \
    -u 1000 \
    -h foo-host \
    -v foo-tmp-volume:/workspaces/tmp \
    -w /home/ubuntu \
    --name foo-ubuntu \
    myubuntu:latest
```

2. `commit`从容器创建新镜像

```bash
$ docker commit \
    -a "Bill Dong" \
    -c "CMD [ \"/bin/bash\" ]" \
    -c "LABEL foo=bar" \
    -m "foo-ubuntu-image init" \
    foo-ubuntu foo-ubuntu-image:latest
```

## save / load

`save`将镜像保存至tarball，包括元数据；`load`将tarball装载成镜像。

## export / import

`export`将容器的文件系统导出至tarball，不包括元数据；`import`将tarball导入成文件系统镜像。

注意：该操作可以将多层文件系统变成一层；导入时可能会丢失CMD之类的信息，使用`-c`选项设置。

## build

`build`是推荐的方法。

1. 写一个`Dockerfile`

```Dockerfile
# FROM scratch
FROM myubuntu:latest
```

2. 构建镜像

```bash
$ docker build .
```
