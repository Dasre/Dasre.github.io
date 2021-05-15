---
title: Docker 基本指令
date: 2021-04-14 22:54:06
tags:
- Docker
---

# Docker基本指令
這邊不對Docker的Image、Container和Repository多做說明，只針對常用的指令做紀錄。

## Image
一般來說，我們會從[Docker Hub](https://hub.docker.com/)抓取我們說需要的Image，如要從非Docker Hub的地方下載Image，需在指令中加入Docker Registry地址。

```shell=
$ docker pull [OPTIONS] [Registry: Port Number] NAME[:TAG|@DIGEST]
```

如果未加入TAG，會自動使用預設的`latest`這個TAG。


