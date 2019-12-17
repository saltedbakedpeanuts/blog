---
title: "用 Grafana 监控 MongoDB"
date: 2019-12-12T19:49:47+08:00
draft: true
toc: false
images:
tags: 
  - MongoDB
  - Grafana
---

### Docker

## MongoDB

### 下载


```bash
docker pull mongo:4.0    # 为了版本兼容性，这里选择 4.0 版本的 Mongo 镜像
```

### 启动

```bash
docker run -p 27233:27017 --name mongo-test mongo:4.0    # 为了方便，不设置登录验证，裸奔
```

## MongoDB Exporter

### 下载

```bash
# 这里用第三方构建好的镜像，但是其版本落后于原仓库，故之前选用的 4.0 Mongo 版本来兼容这个软件
# 还有个远古版本的 eses/mongodb_exporter ，就不建议使用了
docker pull forekshub/percona-mongodb-exporter
```

### 启动

```bash
docker run -p 27021:9216 --name mongo_exporter forekshub/percona-mongodb-exporter -mongodb.uri 'mongodb://本机公网 IP:27233'
```

## Prometheus

### 下载

```bash
docker pull prom/prometheus
```

### 编辑配置文件

 ```bash
 vi /root/prometheus/prometheus.yml
 ```

写入如下内容：


 ```yml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'mongodb_exporter'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['公网IP:27021']
 ```

### 启动

```bash
docker run -p 9090:9090 -v /root/prometheus:/etc/prometheus --name prome prom/prometheus
```

官方文档：https://prometheus.io/docs/prometheus/latest/getting_started/ 

## Grafana

### 下载

```bash
docker pull grafana/grafana
```

### 启动

```bash

```

