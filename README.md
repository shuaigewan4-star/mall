# Mall 云原生 DevOps 实践项目

> 基于开源 Mall 电商系统，在单物理机 Kubernetes 环境中完成云原生改造与 DevOps 全流程实践。

[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.35.x-blue)]()
[![Docker](https://img.shields.io/badge/Docker-29.x-blue)]()
[![Helm](https://img.shields.io/badge/Helm-3.x-blue)]()
[![Prometheus](https://img.shields.io/badge/Prometheus-2.x-orange)]()
[![Grafana](https://img.shields.io/badge/Grafana-12.x-orange)]()
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red)]()
[![Harbor](https://img.shields.io/badge/Harbor-2.x-blue)]()
[![ELK](https://img.shields.io/badge/ELK-7.17.18-yellow)]()

---

# 一、项目简介

本项目基于开源 Mall 电商业务系统，在单物理机环境中自主搭建 Kubernetes 集群，并在此基础上完成商城系统云原生部署与 DevOps 实践。

项目从基础 Kubernetes 环境建设开始，覆盖容器运行环境、集群搭建、应用部署、服务访问、持久化存储、监控、日志以及 CI/CD 自动化交付等完整流程。

项目重点不是业务功能开发，而是围绕真实业务系统场景，实践企业中常见的云原生运维与 DevOps 技术体系。

项目主要包含以下实践内容：

## 1. Kubernetes 集群搭建

在单物理机环境中完成 Kubernetes 集群部署：

* Linux 基础环境配置
* Docker 容器运行环境
* Kubernetes 集群初始化
* 节点组件部署
* 网络插件配置
* 集群状态验证

实现从裸机环境到可运行 Kubernetes 平台的完整搭建流程。

---

## 2. 应用容器化

基于 Docker 完成 Mall 应用容器化：

* Docker 镜像构建
* 应用运行环境标准化
* 容器运行验证
* 镜像仓库管理

---

## 3. Kubernetes 应用部署

完成商城系统 Kubernetes 化部署，包括：

* Namespace 资源隔离
* Deployment 应用编排
* Service 服务发现
* Ingress 外部访问
* ConfigMap 配置管理
* Secret 敏感信息管理
* ImagePullSecret 私有镜像认证

---

## 4. Kubernetes 生产化配置

针对应用运行环境进行生产化优化：

* Startup Probe
* Readiness Probe
* Liveness Probe
* Resource Requests
* Resource Limits
* HPA 自动扩缩容
* PersistentVolume
* PersistentVolumeClaim
* StorageClass 持久化存储

---

## 5. Monitoring 监控体系

基于 Prometheus + Grafana 构建 Kubernetes 监控平台：

* Prometheus 指标采集
* Grafana 数据可视化
* AlertManager 告警管理
* ServiceMonitor 自动发现
* 自定义 Mall Overview Dashboard

监控内容包括：

* Kubernetes 集群资源
* Node CPU / Memory / Disk
* Pod 运行状态
* Deployment 状态
* MySQL / Redis / Elasticsearch / RabbitMQ 指标
* Logstash Exporter 指标

---

## 6. 日志体系

基于 ELK 构建 Kubernetes 日志平台：

* Elasticsearch 日志存储
* Logstash 日志采集
* Kibana 日志查询分析

实现应用日志集中采集、存储以及查询。

---

## 7. CI/CD 持续交付

构建：

```text
GitHub

↓

Jenkins

↓

Harbor

↓

Kubernetes
```

自动化交付流程。

包含：

* Jenkins Pipeline
* Jenkins Kubernetes Dynamic Agent
* Jenkins Kubernetes RBAC
* Docker Image Build
* Harbor 私有镜像仓库
* Helm Chart
* Helm CD
* Kubernetes Rolling Update

---

## 8. Kubernetes 故障排查实践

针对 Kubernetes 运行环境进行问题定位：

* Pod 异常排查
* Container 运行问题分析
* Kubernetes 组件状态分析
* 网络访问问题定位
* Elasticsearch / Logstash 异常排查

---

最终形成完整链路：

```text
物理机 Linux 环境

↓

Docker

↓

Kubernetes 集群

↓

Mall 应用部署

↓

Ingress 服务访问

↓

Prometheus + Grafana 监控

↓

ELK 日志平台

↓

Jenkins CI/CD 自动发布
```

完成从基础环境搭建到应用持续交付的完整云原生 DevOps 实践。


# 二、项目整体架构

## 2.1 整体架构概览

本项目基于单物理机环境搭建 Kubernetes 集群，并在 Kubernetes 平台中部署 Mall 电商系统及相关基础组件。

整体架构围绕：

* Kubernetes 容器编排平台
* 微服务应用运行环境
* 中间件服务
* 监控体系
* 日志体系
* CI/CD 自动化交付

进行设计。

整体流程如下：

```text
                 用户访问

                    │

                    ▼

              Ingress-Nginx

                    │

                    ▼

              Kubernetes Service

                    │

                    ▼

              Mall 应用 Pod

                    │

        ┌───────────┼───────────┐

        ▼           ▼           ▼

     MySQL       Redis       Elasticsearch

        │           │           │

        └───────────┼───────────┘

                    │

                    ▼

          Prometheus + Grafana

                    │

                    ▼

              ELK 日志平台
```

---

# 2.2 Kubernetes 集群架构

项目 Kubernetes 环境基于单物理机搭建。

基础架构：

```text
物理机 Linux

      │

      ▼

Docker Container Runtime

      │

      ▼

Kubernetes Cluster

      │

      ├── kube-apiserver

      ├── kube-controller-manager

      ├── kube-scheduler

      ├── kubelet

      ├── kube-proxy

      └── Container Network Plugin
```

Kubernetes 集群负责：

* 应用生命周期管理
* 服务发现
* 网络访问
* 资源调度
* 存储管理
* 自动恢复

---

# 2.3 应用部署架构

Mall 应用运行于 Kubernetes Namespace 中。

主要资源：

```text
Namespace

mall-prod

      │

      ├── Deployment

      │      ├── mall-admin

      │      ├── mall-portal

      │      ├── mall-search

      │      └── mall-app-web

      │

      ├── StatefulSet

      │      ├── MySQL

      │      ├── Redis

      │      └── Elasticsearch

      │

      ├── Service

      │

      ├── ConfigMap

      │

      ├── Secret

      │

      ├── PVC

      │

      └── Ingress
```

通过 Kubernetes 资源实现应用统一管理。

---

# 2.4 网络访问架构

由于项目运行环境为单物理机 Kubernetes 集群，不具备云厂商提供的 LoadBalancer 能力，因此引入 MetalLB 为 Kubernetes 提供 LoadBalancer 类型 Service 的网络支持。

网络访问链路：

```text id="9vqlhf"
用户

  │

  ▼

MetalLB LoadBalancer IP

  │

  ▼

Ingress Controller

  │

  ▼

Ingress Rule

  │

  ▼

Service

  │

  ▼

Pod
```

## MetalLB 网络实现

MetalLB 在 Kubernetes 集群内部模拟云环境中的 LoadBalancer 功能。

主要作用：

* 为 LoadBalancer 类型 Service 分配外部访问 IP
* 提供裸机 Kubernetes 环境的服务暴露能力
* 实现 Ingress Controller 对外访问

工作流程：

```text id="k0k8k2"
Ingress Controller Service

        │

        │ type: LoadBalancer

        ▼

      MetalLB

        │

        ▼

  IP Address Pool

        │

        ▼

  External IP
```

通过 MetalLB 分配的 External IP，用户可以访问 Kubernetes 内部部署的商城系统。

---

## Kubernetes 网络组件关系

整体网络结构：

```text id="3h7f2c"
客户端

  │

  ▼

External IP

  │

  ▼

MetalLB

  │

  ▼

Ingress-Nginx Controller

  │

  ▼

Ingress Resource

  │

  ▼

Kubernetes Service

  │

  ▼

Application Pod
```

其中：

### MetalLB

负责：

* 裸机 Kubernetes LoadBalancer 实现
* IP 地址分配

### Ingress-Nginx

负责：

* HTTP/HTTPS 请求入口
* 域名转发
* 七层路由

### Service

负责：

* Pod 服务发现
* 集群内部访问

### Pod

负责：

* 实际业务运行

最终实现从物理网络到 Kubernetes 应用的完整访问链路。


# 2.5 存储架构

项目通过 Kubernetes 持久化机制保证数据可靠性。

存储流程：

```text
Pod

 │

 ▼

PersistentVolumeClaim

 │

 ▼

PersistentVolume

 │

 ▼

Storage Backend
```

主要使用 PVC 的组件：

* MySQL
* Elasticsearch
* Prometheus
* Grafana

实现：

* Pod 重启数据不丢失
* 应用生命周期与数据生命周期分离

---

# 2.6 Monitoring 架构

监控体系基于 Prometheus + Grafana 构建。

架构：

```text
Kubernetes Metrics

        │

        ▼

   Prometheus

        │

        ▼

    Grafana

        │

        ▼

 Dashboard 展示
```

采集内容包括：

* Kubernetes 集群指标
* Node资源指标
* Pod运行状态
* Container状态
* Middleware Exporter指标

主要组件：

* Prometheus
* Grafana
* AlertManager
* Exporter
* ServiceMonitor

---

# 2.7 日志架构

基于 ELK 构建统一日志平台。

流程：

```text
Application

    │

    ▼

 Logstash

    │

    ▼

Elasticsearch

    │

    ▼

 Kibana
```

实现：

* 应用日志采集
* 日志集中存储
* 日志检索分析

---

# 2.8 CI/CD 架构

自动化交付流程设计：

```text
Developer

    │

    ▼

 GitHub

    │

    ▼

 Jenkins Pipeline

    │

    ▼

 Docker Build

    │

    ▼

 Harbor Registry

    │

    ▼

 Kubernetes Deployment

    │

    ▼

 Application Release
```

实现：

* 自动构建镜像
* 自动上传镜像
* 自动更新 Kubernetes 应用
* 自动化发布

---

# 2.9 项目整体链路总结

最终形成：

```text
Linux物理机

↓

Docker

↓

Kubernetes

↓

Mall Application

↓

Ingress

↓

PVC持久化

↓

Prometheus/Grafana监控

↓

ELK日志

↓

Jenkins CI/CD
```

构建完整云原生 DevOps 实践环境。


---


## 三、技术栈

本项目围绕 Kubernetes 云原生环境，搭建完整的容器化、持续交付、监控与日志体系。

整体技术栈如下：

| 分类              | 技术                                               |
| --------------- | ------------------------------------------------ |
| 操作系统            | Ubuntu 24.04                                     |
| 服务器环境           | 单物理机 Kubernetes 集群                               |
| 容器引擎            | Docker                                           |
| 容器运行时           | containerd                                       |
| 容器编排            | Kubernetes                                       |
| Kubernetes 网络   | Calico CNI                                       |
| LoadBalancer 实现 | MetalLB                                          |
| Kubernetes 包管理  | Helm                                             |
| 镜像仓库            | Harbor                                           |
| CI/CD 平台        | Jenkins                                          |
| CI Agent        | Kubernetes Dynamic Agent                         |
| Java 构建工具       | Maven                                            |
| 代码管理            | GitHub                                           |
| 配置管理            | ConfigMap / Secret                               |
| 健康检查            | Liveness Probe / Readiness Probe / Startup Probe |
| 资源管理            | Resource Requests / Limits                       |
| 自动扩缩容           | Kubernetes HPA                                   |
| 存储管理            | PersistentVolume / PVC / StorageClass            |
| 监控体系            | Prometheus + Grafana + AlertManager              |
| Kubernetes 监控   | kube-state-metrics / node-exporter               |
| 应用监控            | Exporter 体系                                      |
| 日志体系            | Elasticsearch + Logstash + Kibana                |
| 日志采集            | Logstash Pipeline                                |
| 部署方式            | Kubernetes YAML / Helm Chart                     |
| 发布流程            | GitHub → Jenkins → Harbor → Kubernetes           |

---

## Kubernetes 集群环境

本项目 Kubernetes 集群并非直接使用云厂商托管服务，而是在单台物理机环境中自主搭建 Kubernetes 集群。

集群主要组件：

```text
Ubuntu 24.04
      |
      |
 Kubernetes Cluster
      |
      ├── kube-apiserver
      ├── kube-controller-manager
      ├── kube-scheduler
      ├── kubelet
      ├── containerd
      |
      ├── Calico CNI
      |
      └── MetalLB
```

其中：

* containerd 作为 Kubernetes 容器运行时
* Calico 负责 Pod 网络通信
* MetalLB 为裸机 Kubernetes 环境提供 LoadBalancer 能力
* Helm 用于管理 Kubernetes 应用部署

---

## DevOps 工具链

项目完整 DevOps 流程：

```text
Developer

    |
    v

GitHub

    |
    v

Jenkins Pipeline

    |
    |
    ├── Maven Build
    |
    ├── Docker Build
    |
    ├── Push Image
    |
    v

Harbor Private Registry

    |
    v

Kubernetes Deployment

    |
    v

Mall Application Running
```

通过 Jenkins 实现：

* 自动拉取代码
* Maven 编译打包
* Docker 镜像构建
* 推送 Harbor 私有仓库
* 更新 Kubernetes Deployment
* 自动发布应用

---

## 监控与日志体系

### 监控架构

```text
Kubernetes Cluster

        |
        |
 Prometheus

        |
        |
 Grafana Dashboard

        |
        |
 AlertManager
```

监控内容包括：

* Kubernetes Node 资源
* Pod 状态
* Deployment 状态
* Container Restart
* CPU / Memory 使用率
* Exporter 服务状态
* 应用组件运行状态

### 日志架构

```text
Application

      |
      v

Logstash

      |
      v

Elasticsearch

      |
      v

Kibana
```

实现：

* 应用日志采集
* 日志解析处理
* Elasticsearch 存储
* Kibana 查询分析

---

## 项目核心能力覆盖

通过以上技术栈，实现了：

* 从传统部署方式向 Kubernetes 云原生部署迁移
* 从手工发布向 CI/CD 自动化发布转变
* 从单节点服务管理向容器编排管理转变
* 从人工查看日志向集中化日志平台转变
* 从基础资源监控向 Prometheus 指标监控体系建设

最终形成完整的：

```text
应用容器化
        |
Kubernetes 编排
        |
CI/CD 自动发布
        |
Prometheus 监控
        |
ELK 日志分析
```

云原生 DevOps 实践体系。

---

# 四、项目目录

```text
mall/
├── docker/
│   ├── mall-admin/
│   ├── mall-portal/
│   └── mall-search/
│
├── mall-admin/
├── mall-portal/
├── mall-search/
├── mall-common/
├── mall-mbg/
├── mall-security/
├── mall-demo/
│
├── mall-helm/
│   └── mall-admin/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│
├── Jenkinsfile
│
└── document/
```

---

# 四、项目整体架构设计

## 4.1 项目整体架构

本项目基于开源 Mall 电商系统，在自主搭建的 Kubernetes 集群环境中完成云原生改造。

项目重点围绕：

* 应用容器化
* Kubernetes 编排部署
* 服务访问治理
* 数据持久化
* 监控体系建设
* 日志集中管理
* 自动化持续交付

构建完整 DevOps 实践环境。

整体架构如下：

```text
                         用户访问

                            |
                            v

                    MetalLB External IP

                            |
                            v

                    Ingress-Nginx

                            |
                            v

                    Kubernetes Ingress

                            |
                            v

                    Kubernetes Service

                            |
        -------------------------------------

        |                 |                 |

        v                 v                 v


 mall-admin-web     mall-app-web      mall-portal

        |
        |
        v

    后端服务层

        |
        |
 ------------------------------------------------

 |              |              |                |

 v              v              v                v

mall-admin   mall-portal   mall-search     其他业务服务


 ------------------------------------------------

        |
        |
        v


             基础中间件层


 ------------------------------------------------

 |              |              |                |

 v              v              v                v


 MySQL        Redis       RabbitMQ       Elasticsearch


 ------------------------------------------------


        |
        |
        v


           运维支撑体系


 ------------------------------------------------

 |                     |                       |

 v                     v                       v


Prometheus          ELK                  Jenkins


Grafana             Logstash             Harbor


AlertManager        Kibana               Helm
```

---

# 4.2 Kubernetes 部署架构

项目所有业务组件均运行于 Kubernetes 集群内部。

整体 Kubernetes 架构：

```text
                    Kubernetes Cluster


                         |

        -------------------------------------

        |                                   |

        v                                   v


    Application Namespace             Monitoring Namespace


        |                                   |

        |                                   |

        v                                   v


   mall-prod                         monitoring


        |                                   |

        |                                   |

 -------------------              -------------------

 |        |        |              |        |        |

Pod     Svc    ConfigMap        Prometheus Grafana AlertManager


```

---

# 4.3 应用访问流程

用户访问 Mall 系统流程：

```text
用户浏览器

      |

      v

MetalLB 分配 External IP

      |

      v

Ingress-Nginx Controller

      |

      v

Ingress Rule

      |

      v

Service

      |

      v

Application Pod

      |

      v

MySQL / Redis / Elasticsearch / RabbitMQ
```

其中：

## MetalLB

负责：

* 裸机 Kubernetes 环境提供 LoadBalancer 能力
* 为 Ingress Controller 分配外部访问地址

## Ingress-Nginx

负责：

* HTTP 请求入口
* 域名转发
* 七层路由管理

## Service

负责：

* Pod 服务发现
* 负载均衡
* 集群内部通信

---

# 4.4 项目 Namespace 规划

项目按照功能划分 Kubernetes Namespace：

| Namespace      | 作用                      |
| -------------- | ----------------------- |
| mall-prod      | Mall 业务应用               |
| monitoring     | Prometheus / Grafana 监控 |
| ingress-nginx  | Ingress 控制器             |
| metallb-system | MetalLB 网络组件            |

业务运行环境：

```text
mall-prod

 |

 |-- mall-admin

 |-- mall-admin-web

 |-- mall-app-web

 |-- mall-portal

 |-- mall-search

 |-- mysql

 |-- redis

 |-- rabbitmq

 |-- elasticsearch

 |-- logstash
```

---

# 4.5 项目 Kubernetes 资源设计

业务组件主要使用 Kubernetes 原生资源：

## Deployment

用于：

* 无状态应用部署
* 前后端服务运行

例如：

* mall-admin
* mall-admin-web
* mall-portal

---

## StatefulSet

用于：

* 有状态服务部署
* 数据稳定运行

例如：

* Elasticsearch
* MySQL

---

## Service

用于：

* 服务发现
* Pod访问

---

## ConfigMap

用于：

* 应用配置管理

例如：

* 数据库连接配置
* Redis配置
* Elasticsearch配置

---

## Secret

用于：

* 敏感信息管理

例如：

* 数据库密码
* Harbor认证信息

---

## PersistentVolume / PVC

用于：

* 数据持久化
* 防止 Pod 删除导致数据丢失

主要应用：

* MySQL
* Elasticsearch
* Grafana

---

# 4.6 项目目录结构

项目代码结构：

```text
mall

├── Application

│
├── Docker

│
├── Kubernetes

│
├── Helm

│
├── Monitoring

│
├── CICD

│
├── Frontend

│
└── README.md
```

目录说明：

| 目录          | 作用                     |
| ----------- | ---------------------- |
| Application | 后端应用相关配置               |
| Docker      | Dockerfile及镜像构建        |
| Kubernetes  | Kubernetes YAML资源文件    |
| Helm        | Helm Chart部署文件         |
| Monitoring  | Prometheus/Grafana监控配置 |
| CICD        | Jenkins Pipeline配置     |
| Frontend    | 前端项目代码                 |

---

# 4.7 项目最终目标

通过以上架构，实现：

```text
代码提交

    ↓

Docker镜像构建

    ↓

Harbor镜像管理

    ↓

Helm部署

    ↓

Kubernetes运行

    ↓

Ingress访问

    ↓

Prometheus监控

    ↓

ELK日志分析

    ↓

Jenkins自动化发布
```

形成完整 Kubernetes + DevOps 云原生实践平台。

---

# 五、Mall 应用 Kubernetes 部署

## 5.1 应用部署概述

本项目基于开源 Mall 电商系统，将原有应用进行容器化改造，并部署到 Kubernetes 集群中。

部署目标：

* 将应用由传统部署方式迁移至 Kubernetes
* 使用 Kubernetes 统一管理应用生命周期
* 实现应用配置与代码分离
* 实现服务自动发现
* 增加健康检查机制
* 增加资源管理能力

整体部署流程：

```text id="c6lq9v"
源码

 |

 v

Docker Build

 |

 v

Harbor镜像仓库

 |

 v

Kubernetes Deployment

 |

 v

Pod运行

 |

 v

Service暴露

 |

 v

Ingress访问
```

---

# 5.2 Namespace 规划

为了实现业务隔离，创建独立业务 Namespace：

```bash
kubectl create namespace mall-prod
```

项目所有 Mall 业务组件均部署在：

```text
mall-prod
```

Namespace结构：

```text id="p6h7s9"
mall-prod

 |

 |-- 前端服务

 |    |

 |    |-- mall-admin-web

 |    |-- mall-app-web


 |

 |-- 后端服务

 |    |

 |    |-- mall-admin

 |    |-- mall-portal

 |    |-- mall-search


 |

 |-- 基础组件

      |

      |-- MySQL

      |-- Redis

      |-- RabbitMQ

      |-- Elasticsearch

      |-- Logstash
```

---

# 5.3 镜像管理

项目采用 Harbor 作为 Kubernetes 私有镜像仓库。

镜像流程：

```text id="gk2n4r"
Dockerfile

     |

     v

docker build

     |

     v

Harbor Registry

     |

     v

Kubernetes Image Pull

     |

     v

Pod启动
```

Kubernetes 通过：

```yaml
imagePullSecrets
```

访问 Harbor 私有仓库。

创建 Harbor 密钥：

```bash
kubectl create secret docker-registry harbor-secret \
-n mall-prod \
--docker-server=<harbor-address> \
--docker-username=<username> \
--docker-password=<password>
```

Deployment中引用：

```yaml
spec:
  imagePullSecrets:
    - name: harbor-secret
```

---

# 5.4 ConfigMap 配置管理

项目使用 ConfigMap 管理非敏感配置。

主要用途：

* 应用配置
* 环境变量
* 服务地址

例如：

```text id="6cg3wm"
ConfigMap

    |

    v

Deployment

    |

    v

Container Environment
```

实现：

* 配置与镜像分离
* 修改配置无需重新构建镜像
* 支持不同环境部署

---

# 5.5 Secret 密钥管理

敏感信息使用 Kubernetes Secret 管理。

主要包括：

* 数据库密码
* Redis密码
* Harbor认证信息

示例：

```text id="1w7mhi"
Secret

 |

 v

Pod

 |

 v

Application
```

避免：

* 密码写入代码
* 密码写入镜像
* 配置文件明文保存

---

# 5.6 Deployment 部署应用

无状态业务组件采用 Deployment 部署。

主要服务：

* mall-admin
* mall-admin-web
* mall-app-web
* mall-portal
* mall-search

Deployment负责：

* Pod副本管理
* 滚动更新
* 故障自动恢复

部署示例：

```yaml
apiVersion: apps/v1
kind: Deployment

spec:
  replicas: 1

  selector:
    matchLabels:
      app: mall-admin

  template:
    metadata:
      labels:
        app: mall-admin

    spec:
      containers:
      - name: mall-admin
        image: harbor/mall-admin:latest
```

---

# 5.7 Service 服务发现

Kubernetes Service 用于提供稳定访问入口。

服务访问模型：

```text id="4fjxj8"
Pod

 |

 v

Service

 |

 v

Other Service / Ingress
```

Service作用：

* Pod动态发现
* IP地址抽象
* 服务负载均衡

例如：

```text
mall-admin-service

        |

        v

mall-admin Pod
```

---

# 5.8 健康检查机制

为了提高应用可靠性，为业务组件增加 Kubernetes Probe。

包含：

## Startup Probe

用于：

* 判断应用是否完成启动

## Readiness Probe

用于：

* 判断是否可以接收流量

## Liveness Probe

用于：

* 判断应用是否异常

流程：

```text id="j9n9p8"
Container启动

        |

        v

Startup Probe

        |

        v

Readiness Probe

        |

        v

加入Service流量

        |

        v

Liveness Probe

        |

        v

持续健康检查
```

---

# 5.9 Resource Requests / Limits

为 Pod 设置资源限制。

目的：

* 防止单个服务占用过多资源
* 提高节点稳定性
* 支持 Kubernetes 调度

示例：

```yaml
resources:

  requests:
    cpu: 500m
    memory: 512Mi

  limits:
    cpu: 1000m
    memory: 1Gi
```

---

# 5.10 Ingress 流量入口

项目使用 Ingress-Nginx 作为统一访问入口。

访问流程：

```text id="u7h7ji"
用户

 |

 v

Ingress-Nginx

 |

 v

Ingress Rule

 |

 v

Service

 |

 v

Pod
```

实现：

* 域名访问
* 前后端路由
* 统一入口管理

---

# 5.11 应用部署验证

部署完成后进行验证：

查看 Namespace：

```bash
kubectl get all -n mall-prod
```

查看 Pod：

```bash
kubectl get pods -n mall-prod
```

查看 Service：

```bash
kubectl get svc -n mall-prod
```

查看 Ingress：

```bash
kubectl get ingress -n mall-prod
```

最终状态：

```text
所有业务 Pod Running

Service 正常

Ingress 可访问

业务系统正常运行
```

---

# 5.12 Kubernetes 部署总结

通过 Kubernetes 部署 Mall 应用，实现：

* 应用容器化运行
* Kubernetes 自动管理生命周期
* 配置与密钥分离
* 服务自动发现
* 健康检查
* 资源控制
* 统一入口访问

完成从传统应用部署到 Kubernetes 云原生部署的迁移。


---

# 六、Kubernetes 生产化优化

在完成 Mall 电商系统 Kubernetes 部署后，进一步按照生产环境标准对 Kubernetes 资源进行优化。

优化目标：

* 提高应用稳定性
* 增强故障自动恢复能力
* 合理利用集群资源
* 提升应用发布可靠性
* 实现更加符合企业生产环境的 Kubernetes 部署方式

主要优化内容包括：

* 配置管理优化
* 应用健康检查
* 资源限制
* 自动扩缩容
* 持久化存储
* 服务访问优化

---

## 6.1 Namespace 资源隔离

项目使用独立 Namespace 对业务资源进行隔离。

业务 Namespace：

```text
mall-prod
```

所有 Mall 业务组件均部署在该 Namespace 下，包括：

```text
mall-admin

mall-admin-web

mall-app-web

mall-portal

mall-search

mysql

redis

elasticsearch

logstash
```

通过 Namespace 实现：

* 资源逻辑隔离
* 权限控制
* 方便统一管理

查看：

```bash
kubectl get all -n mall-prod
```

---

# 6.2 ConfigMap 配置管理

传统部署中，应用配置通常直接写入配置文件或者镜像内部。

本项目使用 Kubernetes ConfigMap 管理非敏感配置。

例如：

* 数据库连接地址
* Redis 地址
* Elasticsearch 地址
* 应用环境配置

创建：

```yaml
apiVersion: v1

kind: ConfigMap

metadata:
  name: mall-config
  namespace: mall-prod
```

应用通过：

```yaml
envFrom:
- configMapRef:
    name: mall-config
```

加载配置。

优势：

* 配置与镜像分离
* 修改配置无需重新构建镜像
* 方便不同环境复用

---

# 6.3 Secret 敏感信息管理

对于密码等敏感数据，不直接写入 Deployment。

使用 Kubernetes Secret 管理：

包括：

* MySQL 密码
* Redis 密码
* Harbor 登录认证

例如：

```yaml
apiVersion: v1

kind: Secret

metadata:
  name: mall-secret
  namespace: mall-prod
```

Pod 通过 Secret 注入：

```yaml
env:
- name: MYSQL_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mall-secret
      key: password
```

避免：

* 密码明文暴露
* 配置文件泄露

---

# 6.4 Harbor 私有镜像认证

项目使用 Harbor 作为私有镜像仓库。

Kubernetes 通过 ImagePullSecret 拉取私有镜像。

创建：

```bash
kubectl create secret docker-registry harbor-secret \
-n mall-prod \
--docker-server=<harbor地址> \
--docker-username=<username> \
--docker-password=<password>
```

Deployment 配置：

```yaml
imagePullSecrets:

- name: harbor-secret
```

实现：

```text
Harbor

↓

Kubernetes

↓

Pod
```

自动拉取私有镜像。

---

# 6.5 Health Check 健康检查优化

为了避免应用启动异常或者运行异常后仍然接收流量，对业务 Deployment 增加 Kubernetes 探针。

## StartupProbe

用于检测应用是否完成启动。

避免：

* Spring Boot 启动时间过长
* Kubernetes 误判失败

---

## ReadinessProbe

判断 Pod 是否可以接收业务流量。

流程：

```text
Pod启动

↓

Readiness检测

↓

通过

↓

加入Service Endpoint
```

---

## LivenessProbe

检测应用是否存活。

失败后：

```text
Kubernetes

↓

自动重启Pod
```

提高业务自恢复能力。

---

# 6.6 Resource Requests / Limits

为业务 Pod 设置资源限制。

示例：

```yaml
resources:

 requests:
   cpu: 500m
   memory: 512Mi

 limits:
   cpu: 1000m
   memory: 1Gi
```

作用：

* 保证应用基础资源
* 防止单个服务占满节点
* 支持 Kubernetes 调度

---

# 6.7 HPA 自动扩缩容

针对业务压力变化，引入 Horizontal Pod Autoscaler。

根据 CPU 使用率自动调整 Pod 副本数量。

例如：

```text
CPU > 70%

↓

增加Pod数量


CPU降低

↓

减少Pod数量
```

查看：

```bash
kubectl get hpa -n mall-prod
```

实现业务服务弹性伸缩。

---

# 6.8 PVC 持久化存储

对于有状态服务，使用 Kubernetes 持久化存储。

包括：

* MySQL
* Redis
* Elasticsearch

存储链路：

```text
Pod

↓

PVC

↓

PV

↓

Storage
```

避免：

* Pod 删除导致数据丢失
* 容器重启数据消失

---

# 6.9 StatefulSet 有状态服务管理

对于需要稳定网络标识和数据持久化的组件，使用 StatefulSet。

例如：

```text
Elasticsearch

MySQL

Redis
```

相比 Deployment：

StatefulSet 提供：

* 固定 Pod 名称
* 稳定网络身份
* 有序启动
* PVC 绑定

---

# 6.10 Deployment 更新策略

业务应用采用 Kubernetes RollingUpdate 滚动更新。

发布流程：

```text
新版本镜像

↓

创建新Pod

↓

健康检查

↓

逐步替换旧Pod
```

避免：

* 服务中断
* 全量重启风险

---

通过以上优化，Mall 项目从基础 Kubernetes 部署进一步提升到接近生产环境的运行方式。

优化后的 Kubernetes 平台具备：

* 配置管理能力
* 故障自动恢复能力
* 资源控制能力
* 弹性伸缩能力
* 持久化能力
* 稳定发布能力

为后续监控、日志以及 CI/CD 自动化流程提供基础。


---

# 七、Prometheus + Grafana 监控体系建设

为了实现 Kubernetes 集群以及 Mall 业务系统运行状态可视化，本项目搭建 Prometheus + Grafana 云原生监控体系。

监控目标：

* Kubernetes 集群资源监控
* Node 节点资源监控
* Pod 运行状态监控
* 应用组件健康状态监控
* 中间件运行状态监控
* 业务系统整体运行情况展示

整体监控架构：

```text
                    Kubernetes Cluster

                           |

              kube-state-metrics
              node-exporter
              exporters

                           |

                           ↓

                    Prometheus

                           |

                           ↓

                    Grafana

                           |

                           ↓

                 Mall Monitoring Dashboard
```

---

# 7.1 Prometheus 监控平台部署

项目使用 kube-prometheus-stack 部署 Prometheus 监控体系。

组件包括：

* Prometheus
* Grafana
* AlertManager
* kube-state-metrics
* node-exporter

其中：

Prometheus：

负责指标采集与存储。

Grafana：

负责指标展示和可视化。

AlertManager：

负责告警管理。

kube-state-metrics：

提供 Kubernetes 对象状态指标。

node-exporter：

提供节点 CPU、Memory、Disk 等系统指标。

---

# 7.2 Prometheus 数据持久化

默认情况下 Prometheus 数据存储在 Pod 内部。

为了避免 Pod 重启导致监控历史数据丢失，为 Prometheus 配置 PVC 持久化。

存储链路：

```text
Prometheus Pod

        |

        ↓

PersistentVolumeClaim

        |

        ↓

PersistentVolume

        |

        ↓

Storage
```

查看 PVC：

```bash
kubectl get pvc -n monitoring
```

实现：

* Prometheus 数据长期保存
* Grafana 配置持久化
* 提升监控系统可靠性

---

# 7.3 Grafana 持久化配置

Grafana 保存：

* Dashboard
* 用户配置
* 数据源配置

通过 PVC 保存 Grafana 数据。

查看：

```bash
kubectl get pvc -n monitoring
```

避免：

* Grafana Pod 重建导致 Dashboard 丢失
* 手工重新导入监控配置

---

# 7.4 Ingress 暴露监控服务

为了方便外部访问 Prometheus 和 Grafana，通过 Ingress 暴露 Web 服务。

访问链路：

```text
Client

↓

Ingress Controller

↓

Service

↓

Grafana / Prometheus Pod
```

例如：

Grafana：

```text
grafana.mall.xxx.com
```

Prometheus：

```text
prometheus.mall.xxx.com
```

相比直接 NodePort：

Ingress 具有：

* 统一入口
* 域名访问
* 后续 HTTPS 扩展方便

---

# 7.5 ServiceMonitor 自动发现机制

Prometheus Operator 使用 ServiceMonitor 自动发现监控目标。

传统方式：

需要手动修改 Prometheus 配置。

本项目采用：

```text
Service

↓

ServiceMonitor

↓

Prometheus Operator

↓

Prometheus Target
```

例如：

创建 ServiceMonitor：

```yaml
apiVersion: monitoring.coreos.com/v1

kind: ServiceMonitor
```

Prometheus 自动发现对应 Service。

优势：

* Kubernetes 原生方式
* 动态发现
* 无需修改 Prometheus 配置文件

---

# 7.6 Kubernetes 集群基础监控

Grafana 接入 Prometheus 数据源后，实现 Kubernetes 集群监控。

主要指标：

## Node 资源

包括：

* CPU 使用率
* Memory 使用率
* Disk 使用率

例如：

```promql
100-(avg(rate(node_cpu_seconds_total{mode="idle"}[5m]))*100)
```

---

## Pod 状态

监控：

* Pod 总数量
* Running Pod 数量
* Pending Pod
* Pod Restart 次数

例如：

```promql
count(kube_pod_info{namespace="mall-prod"})
```

---

## Deployment 状态

监控：

* Deployment 副本数量
* Available Replica

用于判断：

业务 Pod 是否正常运行。

---

# 7.7 中间件 Exporter 接入

为了监控 Mall 依赖组件，引入对应 Exporter。

包括：

| 组件            | Exporter               |
| ------------- | ---------------------- |
| MySQL         | MySQL Exporter         |
| Redis         | Redis Exporter         |
| Elasticsearch | Elasticsearch Exporter |
| RabbitMQ      | RabbitMQ Exporter      |
| Logstash      | Logstash Exporter      |

监控方式：

```text
Exporter

↓

Prometheus

↓

Grafana
```

---

# 7.8 Logstash Exporter 接入实践

Logstash 本身提供：

```text
9600 HTTP API
```

但是返回 JSON 数据：

```text
/_node/stats
```

无法直接被 Prometheus 采集。

因此增加：

```text
Logstash

↓

9600 API

↓

Logstash Exporter

↓

9198 /metrics

↓

Prometheus
```

Exporter 部署：

```text
leroymerlinbr/logstash-exporter
```

验证：

```bash
curl http://127.0.0.1:9198/metrics
```

关键指标：

```text
logstash_node_up 1
```

代表：

* Exporter 正常运行
* Logstash API 可访问
* Prometheus 可以采集

---

# 7.9 自定义 Mall Overview Dashboard

为了展示 Mall 项目整体运行状态，自定义 Grafana Dashboard。

Dashboard：

```text
Mall Overview
```

包含：

## Kubernetes 状态

* Pod 总数
* Running Pod
* Pending Pod
* Pod Restart

## Deployment 状态

* Deployment Available
* StatefulSet Ready

## 节点资源

* CPU 使用率
* Memory 使用率
* Disk 使用率

## 中间件健康状态

* MySQL Exporter
* Redis Exporter
* Elasticsearch Exporter
* RabbitMQ Exporter
* Logstash Exporter

## Prometheus 状态

* Target UP 数量

---

# 7.10 监控验证

验证 Prometheus：

```bash
kubectl get pods -n monitoring
```

确认：

```text
Prometheus Running

Grafana Running

AlertManager Running
```

验证 Target：

访问：

```text
Prometheus → Status → Targets
```

确认：

```text
State: UP
```

验证 Grafana：

查看：

```text
Mall Overview Dashboard
```

能够展示：

* Kubernetes 状态
* 节点资源
* 应用组件状态
* Exporter 状态

说明完整监控链路正常。

---

# 7.11 监控体系总结

最终监控链路：

```text
Kubernetes

↓

node-exporter
kube-state-metrics
Exporter

↓

Prometheus

↓

Grafana

↓

Mall Overview Dashboard
```

通过 Prometheus + Grafana 实现了：

* Kubernetes 集群监控
* 应用运行状态监控
* 中间件监控
* 自定义业务 Dashboard
* 监控数据持久化

完成 Mall 项目的云原生监控体系建设。


---

# 八、Elasticsearch + Logstash + Kibana 日志体系建设

为了实现 Mall 云原生环境下业务日志统一采集、存储、查询和分析，本项目搭建 Elasticsearch + Logstash + Kibana（ELK）日志体系。

日志系统目标：

* 收集 Kubernetes 应用容器日志
* 集中化存储日志数据
* 提供日志检索和分析能力
* 支持业务故障快速定位

整体日志架构：

```text
                 Kubernetes Pod

                       |

                       ↓

              Container Log

                       |

                       ↓

                 Logstash

                       |

                       ↓

              Elasticsearch

                       |

                       ↓

                   Kibana

                       |

                       ↓

              日志查询分析
```

---

# 8.1 ELK 组件介绍

## Elasticsearch

Elasticsearch 负责日志数据存储和检索。

主要作用：

* 保存业务日志
* 提供全文搜索
* 支持日志聚合分析

特点：

* 分布式搜索引擎
* JSON 文档存储
* 高效查询能力

---

## Logstash

Logstash 负责日志采集和处理。

主要功能：

* 接收日志
* 日志解析
* 字段过滤
* 数据转发

处理流程：

```text
Input

↓

Filter

↓

Output
```

---

## Kibana

Kibana 提供日志可视化查询界面。

功能：

* 日志搜索
* 条件过滤
* 时间范围查询
* Dashboard 展示

---

# 8.2 Elasticsearch Kubernetes 部署

Elasticsearch 属于有状态服务，因此采用 StatefulSet 部署。

相比 Deployment：

StatefulSet 提供：

* 固定 Pod 名称
* 稳定网络标识
* 数据持久化
* 有序启动

部署结构：

```text
Elasticsearch StatefulSet

        |

        ↓

 Elasticsearch Pod

        |

        ↓

        PVC

        |

        ↓

        PV
```

查看：

```bash
kubectl get statefulset -n mall-prod
```

查看 Pod：

```bash
kubectl get pods -n mall-prod | grep elastic
```

---

# 8.3 Elasticsearch 数据持久化

为了避免 Elasticsearch Pod 重启导致索引数据丢失，为 Elasticsearch 配置 PVC。

存储链路：

```text
Elasticsearch

↓

PVC

↓

PV

↓

Storage
```

查看：

```bash
kubectl get pvc -n mall-prod
```

持久化内容：

* Index 数据
* Segment 文件
*

---

# 九、Jenkins + Harbor + Kubernetes CI/CD 流水线建设

为了实现 Mall 项目从代码提交到 Kubernetes 自动部署的完整自动化流程，本项目搭建基于 Jenkins + Harbor + Kubernetes + Helm 的 CI/CD 流水线。

实现目标：

* 自动拉取代码
* 自动编译构建
* 自动生成 Docker 镜像
* 自动推送 Harbor 私有仓库
* 自动更新 Kubernetes 应用版本
* 实现持续交付

整体 CI/CD 架构：

```text
                Developer

                    |

                    ↓

                 GitHub

                    |

                    ↓

                Jenkins

                    |

        -----------------------

        |                     |

        ↓                     ↓

     Maven Build        Docker Build


                              |

                              ↓

                          Harbor


                              |

                              ↓

                         Helm Deploy


                              |

                              ↓

                       Kubernetes Cluster

                              |

                              ↓

                          Application
```

---

# 9.1 Jenkins 部署

项目使用 Jenkins 作为持续集成与持续部署平台。

Jenkins 主要负责：

* Pipeline 编排
* 自动化构建
* 镜像发布
* Kubernetes 部署

部署环境：

```text
Jenkins

↓

Kubernetes
```

通过 Kubernetes Dynamic Agent 实现构建任务动态创建。

---

# 9.2 Jenkins Kubernetes Dynamic Agent

传统 Jenkins Agent：

* 固定服务器
* 资源利用率低
* 环境容易污染

本项目采用 Kubernetes Dynamic Agent。

工作流程：

```text
Jenkins Master

        |

        ↓

创建临时 Pod

        |

        ↓

执行 Pipeline

        |

        ↓

任务完成

        |

        ↓

Pod 自动销毁
```

优势：

* 按需创建
* 环境隔离
* 自动释放资源
* 提升 CI 扩展能力

---

# 9.3 Jenkins Kubernetes RBAC 配置

由于 Jenkins 需要操作 Kubernetes 资源，需要配置 RBAC 权限。

权限范围包括：

* 创建 Pod
* 查询 Deployment
* 更新资源
* 执行 Helm 部署

访问链路：

```text
Jenkins

↓

ServiceAccount

↓

Role / ClusterRole

↓

Kubernetes API Server
```

示例：

```yaml
apiVersion: v1

kind: ServiceAccount

metadata:

  name: jenkins

  namespace: jenkins
```

绑定权限：

```yaml
kind: RoleBinding
```

实现 Jenkins 对 Kubernetes 的自动化管理。

---

# 9.4 Maven 自动构建

代码提交后，Jenkins 首先执行 Maven 构建。

流程：

```text
Source Code

↓

Maven

↓

Compile

↓

Package

↓

生成 Jar
```

执行：

```bash
mvn clean package
```

生成：

```text
target/*.jar
```

用于后续 Docker 镜像构建。

---

# 9.5 Docker 镜像自动构建

Jenkins 根据 Dockerfile 自动构建应用镜像。

流程：

```text
Jar

↓

Dockerfile

↓

Docker Image
```

例如：

```bash
docker build -t mall-admin:v1 .
```

镜像包含：

* 基础运行环境
* Java 应用
* 配置文件

实现应用运行环境标准化。

---

# 9.6 Harbor 私有镜像仓库

项目使用 Harbor 作为 Kubernetes 私有镜像仓库。

作用：

* 存储业务镜像
* 镜像版本管理
* 权限控制
* 安全扫描

镜像流程：

```text
Jenkins

↓

Docker Build

↓

Harbor Push

↓

Kubernetes Pull
```

推送：

```bash
docker push harbor.xxx.com/mall/mall-admin:v1
```

---

# 9.7 Kubernetes ImagePullSecret

由于 Harbor 为私有仓库，Kubernetes 需要认证信息。

创建：

```bash
kubectl create secret docker-registry
```

Deployment 使用：

```yaml
imagePullSecrets:

- name: harbor-secret
```

实现：

```text
Kubernetes

↓

认证 Harbor

↓

拉取私有镜像
```

---

# 9.8 Jenkins Pipeline 设计

项目使用 Jenkins Pipeline 定义完整发布流程。

主要阶段：

```text
Checkout

↓

Build

↓

Docker Build

↓

Push Harbor

↓

Helm Deploy

↓

Verify
```

---

## Stage 1：代码拉取

从 GitHub 获取最新代码。

```text
GitHub

↓

Jenkins Workspace
```

---

## Stage 2：应用构建

执行 Maven：

```bash
mvn clean package
```

生成应用 Jar。

---

## Stage 3：Docker 镜像构建

执行：

```bash
docker build
```

生成：

```text
mall-admin:v${BUILD_NUMBER}
```

---

## Stage 4：推送 Harbor

登录 Harbor：

```bash
docker login harbor.xxx.com
```

推送镜像：

```bash
docker push
```

---

## Stage 5：Helm 发布

使用 Helm 更新 Kubernetes。

流程：

```text
Helm Chart

↓

修改 image tag

↓

helm upgrade

↓

Kubernetes Deployment
```

例如：

```bash
helm upgrade mall ./helm
```

---

## Stage 6：发布验证

检查：

```bash
kubectl get pods -n mall-prod
```

确认：

* Pod Running
* 镜像版本更新
* 服务正常

---

# 9.9 Helm 管理 Kubernetes 应用

项目使用 Helm 管理 Kubernetes 应用。

Helm 主要解决：

* 大量 YAML 管理困难
* 环境配置差异
* 版本回滚

目录结构：

```text
Helm

├── Chart.yaml

├── values.yaml

└── templates

    ├── deployment.yaml

    ├── service.yaml

    └── ingress.yaml
```

---

# 9.10 CI/CD 发布流程验证

完整发布流程：

```text
开发提交代码

        ↓

GitHub

        ↓

Jenkins Trigger

        ↓

Maven Build

        ↓

Docker Image Build

        ↓

Push Harbor

        ↓

Helm Upgrade

        ↓

Kubernetes Rolling Update

        ↓

新版本上线
```

验证：

查看 Deployment：

```bash
kubectl rollout status deployment xxx -n mall-prod
```

查看 Pod：

```bash
kubectl get pods -n mall-prod
```

确认：

```text
新版本 Pod Running
```

---

# 9.11 CI/CD 体系总结

通过 Jenkins + Harbor + Kubernetes + Helm，实现 Mall 项目的自动化交付。

最终实现：

* GitHub 代码管理
* Jenkins 自动构建
* Docker 镜像生成
* Harbor 镜像管理
* Helm 自动发布
* Kubernetes 滚动更新

形成完整 DevOps 流程：

```text
Code

↓

Build

↓

Image

↓

Registry

↓

Deploy

↓

Run

↓

Monitor
```

实现从代码提交到生产运行的自动化闭环。


---

# 十、Kubernetes 故障排查与生产问题实践

在 Mall 云原生项目部署和运行过程中，不仅完成了 Kubernetes 应用部署，同时针对实际运行过程中出现的各种异常进行了排查和处理。

通过这些问题实践，形成了一套基于 Kubernetes 分层架构的问题定位方法。

主要涉及：

* Kubernetes 控制面异常
* kubelet 节点异常
* containerd 容器运行时异常
* Pod 生命周期异常
* 应用启动失败
* Service 网络访问异常
* Ingress 访问异常
* PVC 存储异常
* ELK 日志链路异常
* Prometheus 监控异常

整体排查思路：

```text
用户访问

    ↓

Ingress

    ↓

Service

    ↓

Endpoint

    ↓

Pod

    ↓

Container

    ↓

Node

    ↓

Container Runtime

    ↓

Kubernetes Control Plane
```

通过从上到下、从应用到基础设施的方式进行定位。

---

# 10.1 Kubernetes 控制面异常排查

## 问题现象

在 Kubernetes 集群运行过程中，出现：

```bash
kubectl get nodes
```

无法正常返回。

报错：

```text
The connection to the server localhost:6443 was refused
```

现象说明：

kubectl 无法连接 Kubernetes API Server。

---

## 问题分析

Kubernetes 架构中：

```text
kubectl

↓

kube-apiserver

↓

etcd

↓

controller-manager

↓

scheduler
```

所有 Kubernetes 操作都需要经过 kube-apiserver。

因此：

API Server 异常会导致：

* kubectl 无法执行
* 集群状态无法获取
* Controller 无法调谐资源

---

## 排查过程

### 1. 检查 kubelet 状态

查看节点 kubelet：

```bash
systemctl status kubelet
```

确认 kubelet 是否正常运行。

---

### 2. 查看 kubelet 日志

使用：

```bash
journalctl -u kubelet -xe
```

查看：

* kubelet 启动状态
* API Server 连接状态
* Pod 创建情况

---

### 3. 查看 Kubernetes 控制组件

检查：

```bash
kubectl get pods -n kube-system
```

重点关注：

```text
kube-apiserver

kube-controller-manager

kube-scheduler

etcd
```

---

## 解决思路

根据异常组件恢复 Kubernetes 控制面。

恢复后验证：

```bash
kubectl get nodes
```

正常返回：

```text
NAME
STATUS
ROLE
```

说明 Kubernetes 控制面恢复。

---

## 问题总结

通过该问题掌握 Kubernetes 控制面排查流程：

```text
kubectl异常

↓

检查 kube-apiserver

↓

检查 kubelet

↓

检查 kube-system组件

↓

恢复控制面
```

---

# 10.2 containerd 容器运行时异常排查

## 问题现象

部分 Pod 出现：

```text
ContainerCreating

CrashLoopBackOff
```

或者：

```text
Pod 创建失败
```

查看：

```bash
kubectl get pods -n mall-prod
```

发现部分业务 Pod 无法正常启动。

---

## 问题分析

Kubernetes 不直接管理容器。

运行链路：

```text
Kubernetes

↓

kubelet

↓

containerd

↓

container

```

如果 containerd 异常：

会导致：

* 镜像无法创建
* 容器无法启动
* Pod 生命周期异常

---

## 排查过程

### 1. 查看 Pod 事件

执行：

```bash
kubectl describe pod <pod-name> -n mall-prod
```

查看 Events。

发现类似：

```text
failed to create container
```

---

### 2. 查看 containerd 状态

检查：

```bash
systemctl status containerd
```

确认运行状态。

---

### 3. 查看 containerd 日志

执行：

```bash
journalctl -u containerd -xe
```

发现异常信息：

```text
failed to delete task

container not created: not found

received message on inactive stream
```

---

### 4. 使用 crictl 排查

查看容器：

```bash
crictl ps
```

查看详细信息：

```bash
crictl inspect <container-id>
```

确认：

* 容器状态
* Sandbox 状态
* Runtime 信息

---

## 解决方案

根据运行时异常情况恢复 containerd。

例如：

```bash
systemctl restart containerd
```

重新检查：

```bash
kubectl get pods -n mall-prod
```

确认：

```text
STATUS Running
```

---

## 问题总结

该问题验证了 Kubernetes 容器运行链路：

```text
Pod

↓

kubelet

↓

containerd

↓

container
```

排查顺序：

```text
Pod状态

↓

describe查看事件

↓

kubelet日志

↓

containerd日志

↓

crictl深入分析
```

形成 Kubernetes 节点运行时故障排查能力。

---

# 10.3 Pod 生命周期异常排查

在 Mall 项目运行过程中，多个业务组件出现过 Pod 异常状态。

通过 Kubernetes 生命周期状态分析，结合 kubectl、日志和资源监控进行定位。

常见异常状态：

| 状态                | 说明          |
| ----------------- | ----------- |
| Pending           | Pod 无法调度    |
| ContainerCreating | 容器创建失败      |
| ImagePullBackOff  | 镜像拉取失败      |
| CrashLoopBackOff  | 容器启动后持续退出   |
| OOMKilled         | 超过内存限制被系统杀死 |
| Running但不可用       | 应用健康检查失败    |

---

# 10.3.1 CrashLoopBackOff 排查

## 问题现象

查看 Pod：

```bash
kubectl get pods -n mall-prod
```

发现：

```text
STATUS

CrashLoopBackOff
```

例如：

```text
mall-admin-xxx

CrashLoopBackOff
```

说明：

Pod 已经创建，但是容器启动失败并不断重启。

---

## 问题分析

Pod 生命周期：

```text
Pod创建

↓

Container启动

↓

Application启动

↓

Process退出

↓

Kubelet重启Container
```

如果应用进程持续退出：

Kubernetes 会进入：

```text
CrashLoopBackOff
```

---

## 排查步骤

### 1. 查看 Pod 详细信息

执行：

```bash
kubectl describe pod <pod-name> -n mall-prod
```

重点查看：

```text
Events
```

例如：

```text
Back-off restarting failed container
```

---

### 2. 查看容器日志

执行：

```bash
kubectl logs <pod-name> -n mall-prod
```

如果 Pod 有多个容器：

```bash
kubectl logs <pod-name> -c <container-name> -n mall-prod
```

查看：

* 应用启动异常
* 配置错误
* 连接失败

---

### 3. 查看上一次崩溃日志

如果容器已经重启：

使用：

```bash
kubectl logs <pod-name> --previous -n mall-prod
```

查看：

上一次 Container 崩溃原因。

---

## 常见原因

### 配置错误

例如：

* 数据库地址错误
* Redis 地址错误
* 环境变量错误

检查：

```bash
kubectl describe deployment <deployment-name>
```

---

### 依赖服务未启动

例如：

应用启动需要：

```text
MySQL

Redis

RabbitMQ

Elasticsearch
```

如果依赖不可用：

Spring Boot 应用可能直接退出。

---

### 端口配置错误

检查：

Deployment：

```yaml
containerPort
```

Service：

```yaml
targetPort
```

是否一致。

---

# 10.3.2 OOMKilled 内存问题排查

## 问题现象

业务 Pod 不断重启。

查看：

```bash
kubectl get pods -n mall-prod
```

发现：

```text
RESTARTS
不断增加
```

---

## 排查

查看：

```bash
kubectl describe pod <pod-name> -n mall-prod
```

发现：

```text
Reason: OOMKilled
```

说明：

容器超过 Kubernetes 设置的 Memory Limit。

---

## 问题分析

Kubernetes 资源限制：

```yaml
resources:

  requests:

    memory: 512Mi

  limits:

    memory: 1Gi
```

当：

```text
实际使用内存 > limit
```

Linux OOM Killer 会终止进程。

---

## 解决方式

### 调整资源限制

修改 Deployment：

```yaml
resources:

  requests:

    memory: 1Gi

  limits:

    memory: 2Gi
```

---

### 使用 Prometheus 分析

通过 Grafana 查看：

* Container Memory Usage
* Container Restart Count

结合：

```promql
container_memory_usage_bytes
```

判断：

是否存在持续增长。

---

# 10.3.3 ImagePullBackOff 镜像拉取失败

## 问题现象

Pod：

```text
ImagePullBackOff
```

---

## 排查

查看：

```bash
kubectl describe pod <pod-name> -n mall-prod
```

Events：

可能出现：

```text
Failed to pull image
```

---

## 常见原因

### 镜像不存在

检查：

```bash
docker images
```

或者：

Harbor 仓库。

---

### 私有仓库认证失败

Mall 项目使用 Harbor 私有仓库。

Kubernetes 需要：

```yaml
imagePullSecrets:

- name: harbor-secret
```

检查：

```bash
kubectl get secret -n mall-prod
```

---

### 镜像地址错误

例如：

错误：

```text
harbor/mall-admin
```

正确：

```text
harbor.xxx.com/mall/mall-admin:v1
```

---

# 10.3.4 Pending 状态排查

## 问题现象

Pod：

```text
Pending
```

说明：

Pod 已创建，但是没有成功调度。

---

## 排查

查看：

```bash
kubectl describe pod <pod-name> -n mall-prod
```

查看：

```text
Events
```

---

## 常见原因

### 资源不足

例如：

Node 剩余资源不足：

```text
Insufficient cpu

Insufficient memory
```

查看：

```bash
kubectl describe node
```

---

### 节点污点限制

查看：

```bash
kubectl describe node
```

检查：

```text
Taints
```

---

### PVC 未绑定

如果 Pod 使用 PVC：

检查：

```bash
kubectl get pvc -n mall-prod
```

如果：

```text
Pending
```

Pod 也无法启动。

---

# 10.3.5 Readiness / Liveness Probe 异常

为了提高 Mall 应用可靠性，项目配置 Kubernetes 健康检查。

包括：

* Startup Probe
* Readiness Probe
* Liveness Probe

---

## Readiness Probe

作用：

判断应用是否可以接收流量。

流程：

```text
Pod启动

↓

Readiness检测

↓

成功

↓

加入Service Endpoint
```

失败：

Pod 不会进入 Service。

---

## Liveness Probe

作用：

检测应用是否存活。

失败：

Kubernetes 自动重启容器。

---

## Startup Probe

用于：

启动时间较长的应用。

例如：

Spring Boot 应用启动时间较长。

避免：

应用还未启动完成就被 Liveness 重启。

---

# 10.3.6 Pod 排查总结

实际生产排查流程：

```text
kubectl get pods

↓

确认异常状态

↓

kubectl describe pod

↓

查看 Events

↓

kubectl logs

↓

查看资源配置

↓

检查依赖服务

↓

定位根因

↓

修复验证
```

通过以上实践，掌握 Kubernetes 应用运行阶段常见故障定位方法。

---

# 10.4 Kubernetes 网络访问异常排查

在 Mall 项目部署过程中，针对应用访问异常问题，按照 Kubernetes 网络链路进行逐层排查。

Kubernetes 服务访问链路：

```text
用户请求

↓

域名解析

↓

Ingress Controller

↓

Ingress Rule

↓

Service

↓

Endpoint

↓

Pod

↓

Container Port
```

任何一层配置异常都会导致业务无法访问。

---

# 10.4.1 Service 无 Endpoint 问题排查

## 问题现象

访问业务服务失败。

查看 Service：

```bash
kubectl get svc -n mall-prod
```

发现 Service 存在。

但是：

```bash
kubectl get endpoints -n mall-prod
```

发现：

```text
ENDPOINTS

<none>
```

---

## 问题分析

Service 工作流程：

```text
Service

↓

Selector

↓

Pod Label

↓

Endpoint
```

如果 Endpoint 为空：

说明 Service 没有找到对应 Pod。

---

## 排查步骤

### 1. 查看 Service Selector

执行：

```bash
kubectl describe svc <service-name> -n mall-prod
```

查看：

```text
Selector:
```

例如：

```text
app=mall-admin
```

---

### 2. 查看 Pod Label

执行：

```bash
kubectl get pods -n mall-prod --show-labels
```

确认：

Pod 是否存在对应 Label。

---

### 3. 检查 Selector 是否匹配

例如：

Service：

```yaml
selector:

  app: mall-admin
```

Pod：

```yaml
labels:

  app: mall-admin-web
```

由于 Label 不一致：

Service 无法找到 Pod。

---

## 解决

统一：

Service selector

和

Deployment labels。

重新检查：

```bash
kubectl get endpoints -n mall-prod
```

出现：

```text
10.x.x.x:8080
```

说明 Service 已关联 Pod。

---

# 10.4.2 Service Port / TargetPort 配置错误排查

## 问题现象

Service 存在：

```text
Running
```

Endpoint 也存在。

但是访问失败。

---

## 排查

查看 Service：

```bash
kubectl describe svc <service-name> -n mall-prod
```

重点查看：

```text
Port

TargetPort
```

---

例如：

Service：

```yaml
ports:

- port:80

  targetPort:8080
```

表示：

```text
客户端访问

80

↓

Pod

8080
```

---

如果：

应用实际监听：

```text
8081
```

但是：

targetPort：

```text
8080
```

会导致：

Service 转发失败。

---

## 检查应用监听端口

进入 Pod：

```bash
kubectl exec -it <pod-name> -n mall-prod -- bash
```

查看：

```bash
netstat -tlnp
```

或者：

```bash
ss -tlnp
```

确认应用监听端口。

---

# 10.4.3 Ingress 502 问题排查

## 问题现象

访问业务域名：

```text
http://xxx.com
```

返回：

```text
502 Bad Gateway
```

---

## 问题分析

Ingress 访问链路：

```text
用户

↓

Ingress Controller

↓

Ingress Rule

↓

Service

↓

Endpoint

↓

Pod
```

502 通常表示：

Ingress 到后端 Service 通信失败。

---

# 排查流程

## 第一步：检查域名解析

确认 DNS：

```bash
nslookup xxx.com
```

或者：

```bash
dig xxx.com
```

确认：

域名是否解析到 Ingress 地址。

---

## 第二步：检查 Ingress 配置

查看：

```bash
kubectl get ingress -n mall-prod
```

详细信息：

```bash
kubectl describe ingress <name> -n mall-prod
```

检查：

* Host
* Path
* Backend Service
* Service Port

---

## 第三步：检查 Service

查看：

```bash
kubectl get svc -n mall-prod
```

确认：

* Service 存在
* Port 正确

---

## 第四步：检查 Endpoint

执行：

```bash
kubectl get endpoints -n mall-prod
```

如果：

```text
<none>
```

说明：

Ingress 后面没有可用 Pod。

---

## 第五步：检查 Pod

查看：

```bash
kubectl get pods -n mall-prod
```

确认：

```text
STATUS Running
```

查看日志：

```bash
kubectl logs <pod-name> -n mall-prod
```

---

# 10.4.4 Kubernetes DNS 异常排查

## 问题现象

Pod 内访问服务名称失败。

例如：

应用连接：

```text
mysql.mall-prod.svc.cluster.local
```

失败。

---

## 排查 CoreDNS

查看：

```bash
kubectl get pods -n kube-system | grep coredns
```

确认：

CoreDNS Running。

---

查看日志：

```bash
kubectl logs -n kube-system <coredns-pod>
```

---

## Pod 内测试 DNS

进入 Pod：

```bash
kubectl exec -it <pod-name> -n mall-prod -- bash
```

测试：

```bash
nslookup mysql
```

或者：

```bash
ping mysql
```

---

## 常见原因

### Service 不存在

检查：

```bash
kubectl get svc -n mall-prod
```

---

### Namespace 错误

例如：

应用：

```text
mall-prod
```

但是访问：

```text
mysql.default
```

导致解析失败。

---

### CoreDNS 异常

检查：

```bash
kubectl describe pod -n kube-system <coredns>
```

---

# 10.4.5 Calico 网络问题排查

项目 Kubernetes 网络插件采用：

```text
Calico
```

负责：

* Pod 网络
* 网络策略
* Pod 间通信

---

查看 Calico 状态：

```bash
kubectl get pods -n kube-system | grep calico
```

确认：

```text
Running
```

---

查看：

```bash
kubectl describe pod <calico-pod> -n kube-system
```

---

如果出现：

Pod 间无法通信：

排查：

```text
Pod IP

↓

Node 网络

↓

Calico

↓

iptables
```

---

# 10.4.6 网络问题总结

Kubernetes 网络问题排查顺序：

```text
域名

↓

Ingress

↓

Service

↓

Endpoint

↓

Pod

↓

Container Port

↓

应用监听
```

通过该流程，可以快速定位：

* 访问 404
* 502
* 超时
* Service 无响应
* Pod 间通信失败

---

# 10.5 Kubernetes 存储异常排查

在 Mall 云原生项目中，Elasticsearch、Prometheus 等组件属于有状态服务，需要依赖 Kubernetes 持久化存储。

因此针对：

* PersistentVolume
* PersistentVolumeClaim
* StorageClass
* 数据持久化

进行了相关实践和问题排查。

Kubernetes 存储链路：

```text id="8j6b6m"
Application

↓

Pod

↓

PVC

↓

PV

↓

StorageClass

↓

Storage Backend
```

任何一层异常都会导致应用启动失败。

---

# 10.5 Kubernetes 存储异常排查

在 Mall 云原生项目中，Elasticsearch、Prometheus 等组件属于有状态服务，需要依赖 Kubernetes 持久化存储。

因此针对：

* PersistentVolume
* PersistentVolumeClaim
* StorageClass
* 数据持久化

进行了相关实践和问题排查。

Kubernetes 存储链路：

```text id="8j6b6m"
Application

↓

Pod

↓

PVC

↓

PV

↓

StorageClass

↓

Storage Backend
```

任何一层异常都会导致应用启动失败。

---

# 10.5.1 PVC Pending 问题排查

## 问题现象

创建应用后：

```bash id="5j1zq2"
kubectl get pvc -n mall-prod
```

发现：

```text id="4lq5so"
STATUS

Pending
```

说明：

PVC 创建成功，但是没有绑定可用 PV。

---

## 问题分析

PVC 绑定流程：

```text id="pxb2we"
PVC

↓

寻找匹配 PV

↓

绑定

↓

Pod挂载
```

如果没有满足条件的 PV：

PVC 会一直 Pending。

---

# 排查步骤

## 1. 查看 PVC 详细信息

执行：

```bash id="f7y6xi"
kubectl describe pvc <pvc-name> -n mall-prod
```

查看：

```text id="e1f9o7"
Events
```

可能出现：

```text id="qv49bh"
no persistent volumes available
```

---

## 2. 查看 PV 状态

执行：

```bash id="i3x3nq"
kubectl get pv
```

状态：

```text id="4up3kc"
Available

Bound

Released

Failed
```

---

## 3. 检查 StorageClass

查看：

```bash id="z3x9d4"
kubectl get storageclass
```

确认：

* StorageClass 是否存在
* provisioner 是否正确

---

# 10.5.2 PV 绑定失败排查

## 常见原因

### 1. 容量不匹配

例如：

PVC：

```yaml id="y5u3pf"
resources:

 requests:

   storage: 20Gi
```

PV：

```yaml id="9g2zj4"
capacity:

 storage: 10Gi
```

无法绑定。

---

### 2. AccessMode 不匹配

PVC：

```yaml id="e9d8tq"
accessModes:

- ReadWriteOnce
```

PV：

```yaml id="0h35yp"
ReadWriteMany
```

可能导致匹配失败。

---

### 3. StorageClass 不一致

PVC：

```yaml id="82o5oo"
storageClassName: local-storage
```

但是 PV：

```yaml id="x4j8lz"
storageClassName: nfs-storage
```

无法绑定。

---

# 10.5.3 Elasticsearch 持久化问题排查

Mall 项目使用：

```text id="94o9jp"
Elasticsearch
```

作为日志和搜索组件。

由于 Elasticsearch 存储大量数据，需要持久化。

---

## 问题现象

Pod 重启后：

* 索引丢失
* 数据不存在
* Elasticsearch 启动异常

---

## 排查

查看：

```bash id="1rkmh4"
kubectl get pvc -n mall-prod
```

确认：

```text id="i6t3rj"
elasticsearch-data

Bound
```

---

查看 Pod：

```bash id="g4e7qj"
kubectl describe pod <es-pod>
```

确认：

Volume 是否挂载。

---

查看：

```bash id="1g5q1f"
kubectl exec -it <es-pod> -n mall-prod -- df -h
```

确认：

数据目录是否来自 PVC。

---

## 正确数据链路

```text id="yq1x1m"
Elasticsearch Container

↓

/usr/share/elasticsearch/data

↓

PVC

↓

PV

↓

Disk
```

保证：

Pod 删除重新创建后：

数据仍然存在。

---

# 10.5.4 Prometheus 数据持久化问题排查

Prometheus 默认：

```text id="3rj4mb"
EmptyDir
```

数据生命周期：

跟随 Pod。

如果 Pod 删除：

历史监控数据丢失。

---

## 解决方式

配置：

```yaml id="n5ql7m"
volumeClaimTemplates
```

或者：

```yaml id="skh1fi"
persistentVolumeClaim
```

挂载：

```text id="twxj3k"
/prometheus
```

---

## 验证

查看：

```bash id="qx0xdr"
kubectl get pvc -n monitoring
```

确认：

```text id="2m9z0y"
Bound
```

查看 Prometheus：

```bash id="j9n8f3"
kubectl describe pod <prometheus-pod> -n monitoring
```

确认：

Volume 来源：

```text id="1h8xgp"
PersistentVolumeClaim
```

---

# 10.5.5 存储问题排查流程总结

Kubernetes 存储异常排查：

```text id="p9m6m2"
Pod启动失败

↓

检查Volume

↓

检查PVC

↓

检查PV

↓

检查StorageClass

↓

检查底层存储
```

常用命令：

查看 PVC：

```bash id="2f9rwl"
kubectl get pvc -A
```

查看 PV：

```bash id="t5kh0x"
kubectl get pv
```

查看 StorageClass：

```bash id="qj6j89"
kubectl get storageclass
```

查看详细事件：

```bash id="px2u1f"
kubectl describe pvc <name>
```

---

# 10.5.6 存储实践总结

通过 Mall 项目实践，掌握 Kubernetes 有状态服务部署过程中：

* 存储资源规划
* PVC 生命周期管理
* PV 绑定机制
* StorageClass 动态供应
* 数据持久化验证

理解：

```text id="qv1b73"
无状态应用

Deployment

↓

Pod


有状态应用

StatefulSet

↓

PVC

↓

PV
```

为后续 Elasticsearch、数据库等中间件生产化部署提供基础。

---

# 10.6 日志与监控系统异常排查

在 Mall 云原生项目中，部署了完整的日志和监控体系：

日志体系：

```text id="1fkz8h"
应用

↓

Logstash

↓

Elasticsearch

↓

Kibana
```

监控体系：

```text id="y4g2m7"
Kubernetes

↓

Prometheus

↓

Grafana

↓

Dashboard
```

针对运行过程中出现的问题进行了排查。

---

# 10.6.1 Logstash 日志采集异常排查

## 问题现象

应用日志无法正常进入 Elasticsearch。

表现：

* Kibana 查询不到日志
* Elasticsearch 没有新增索引
* Logstash Pipeline 无数据

---

## 排查流程

### 1. 查看 Logstash Pod 状态

执行：

```bash id="t2kh2v"
kubectl get pods -n mall-prod
```

确认：

```text id="s9d7jw"
logstash

Running
```

---

### 2. 查看 Logstash 日志

执行：

```bash id="x6m3sm"
kubectl logs <logstash-pod> -n mall-prod
```

关注：

* Pipeline 启动状态
* Input 是否正常
* Output 是否异常

---

### 3. 检查 Pipeline 配置

查看：

```bash id="r9qz3f"
kubectl get configmap -n mall-prod
```

确认：

Logstash 配置是否正确挂载。

检查：

```text id="8hm5wq"
input

filter

output
```

---

## 常见问题

### Elasticsearch 地址错误

例如：

```text id="jq3o8f"
output elasticsearch {

hosts => ["http://es:9200"]

}
```

如果：

Service 名称错误：

Logstash 无法连接 Elasticsearch。

---

### Elasticsearch 未启动

检查：

```bash id="ytq9nx"
kubectl get pods -n mall-prod
```

确认：

```text id="f0f7w1"
elasticsearch Running
```

---

### Pipeline 配置错误

查看：

```bash id="h4r7xh"
kubectl logs logstash -n mall-prod
```

定位：

语法错误。

---

# 10.6.2 Logstash Exporter 监控异常排查

## 问题现象

部署：

```text id="m7x0t8"
logstash-exporter
```

Pod 状态：

```text id="db3q1n"
Running
```

但是指标异常：

```text id="y9j8z0"
logstash_node_up 0
```

---

## 问题分析

Exporter 工作链路：

```text id="9x7l8d"
Prometheus

↓

logstash-exporter

↓

Logstash API :9600
```

Exporter Running：

只能说明：

```text id="p1e5s0"
Exporter进程正常
```

不能说明：

```text id="0c2d7n"
Exporter能够访问Logstash
```

---

## 排查步骤

### 1. 检查 Logstash API

进入集群测试：

```bash id="r2pk9d"
curl http://logstash:9600/
```

正常返回：

```json id="7yz8oa"
{
 "version":"7.17.18",
 "status":"green"
}
```

说明：

Logstash API 正常。

---

### 2. 检查 Logstash Service

查看：

```bash id="3f2w4m"
kubectl get svc logstash -n mall-prod
```

发现：

原 Service：

```text id="1v6y2g"
4560

4561

4562

4563
```

但是：

缺少：

```text id="8kh6h2"
9600
```

导致：

Exporter 无法访问 Logstash API。

---

### 3. 修改 Service

增加：

```yaml id="n5d8gr"
- name: api

  port: 9600

  targetPort: 9600
```

重新验证：

```bash id="4m6s0y"
curl http://logstash:9600/
```

---

## 最终结果

查看指标：

```bash id="qf5n1a"
curl http://localhost:9198/metrics | grep logstash_node_up
```

返回：

```text id="q2j3h4"
logstash_node_up 1
```

说明：

```text id="2o1f5x"
Prometheus

↓

Exporter

↓

Logstash API

↓

采集成功
```

---

# 10.6.3 Prometheus Target Down 排查

## 问题现象

Prometheus 页面：

```text id="5wrj3v"
Status

Targets
```

发现：

```text id="1o0i9m"
DOWN
```

---

## 排查流程

### 1. 查看 ServiceMonitor

执行：

```bash id="k8p6j9"
kubectl get servicemonitor -A
```

确认：

目标 ServiceMonitor 存在。

---

### 2. 检查 Service

Prometheus 通过 Service 发现 Target。

查看：

```bash id="d3l8q2"
kubectl get svc -n monitoring
```

确认：

metrics 端口存在。

---

### 3. 检查 Endpoint

执行：

```bash id="8s0qz6"
kubectl get endpoints -n monitoring
```

如果：

```text id="1yk2hh"
<none>
```

说明：

Service 没找到 Pod。

---

### 4. 检查 Label

Service：

```yaml id="r8u6hs"
selector:

 app: exporter
```

Pod：

```yaml id="v7r9pj"
labels:

 app: exporter-test
```

不匹配：

Prometheus 无法发现。

---

# 10.6.4 Grafana Dashboard 自动发现异常排查

## 问题现象

已经创建：

```bash id="i7s4ku"
kubectl get cm -n monitoring
```

存在：

```text id="p9s6t2"
mall-overview-dashboard
```

但是：

Grafana 中没有 Dashboard。

---

## 排查

### 1. 检查 ConfigMap Label

Grafana sidecar 根据 Label 自动同步。

查看：

```bash id="3f5g0v"
kubectl get cm -n monitoring -l grafana_dashboard=1
```

确认：

```text id="q8w2bz"
mall-overview-dashboard
```

存在。

---

### 2. 查看 Grafana sidecar 日志

执行：

```bash id="7x2k6s"
kubectl logs -n monitoring deploy/monitoring-grafana \
-c grafana-sc-dashboard
```

查看：

```text id="3h6m8r"
Writing /tmp/dashboards/mall-overview.json
```

说明：

ConfigMap 已同步。

---

### 3. 检查 Dashboard Provisioning

确认：

Grafana sidecar：

```text id="s8v3n1"
ConfigMap

↓

临时目录

↓

Grafana Dashboard
```

链路正常。

---

# 10.6.5 监控问题总结

Prometheus + Grafana 排查流程：

```text id="7g2d9s"
Exporter

↓

Service

↓

ServiceMonitor

↓

Prometheus Target

↓

Grafana Query

↓

Dashboard
```

常见问题：

| 问题            | 排查                 |
| ------------- | ------------------ |
| Exporter Down | 检查 exporter 服务     |
| Target Down   | 检查 ServiceMonitor  |
| 无指标           | 检查 metrics 接口      |
| Dashboard 不显示 | 检查 ConfigMap Label |
| 数据为空          | 检查 PromQL          |

---

# 10.7 Kubernetes 故障排查总结

通过 Mall 项目实践，形成 Kubernetes 分层故障排查方法：

```text id="k6v4r1"
应用层

↓

Pod

↓

Service

↓

Ingress

↓

网络

↓

存储

↓

节点

↓

容器运行时

↓

Kubernetes控制面
```

常用排查工具：

```text id="3n8h4p"
kubectl

kubectl describe

kubectl logs

journalctl

crictl

curl

nslookup

ss

Prometheus

Grafana
```

通过以上案例，完成从应用到基础设施的 Kubernetes 全链路故障定位实践。

---


# 第十一章 Helm Chart 模板化部署实践

在 Mall 云原生项目中，前期 Kubernetes 部署主要通过 YAML 文件进行管理。

随着项目资源增加：

* Deployment
* Service
* Ingress
* ConfigMap
* Secret
* PVC

配置文件数量不断增加，直接维护 YAML 存在：

* 环境切换困难
* 参数修改重复
* 配置管理复杂

因此引入 Helm 对 Kubernetes 应用进行模板化管理。

---

# 11.1 Helm 简介

Helm 是 Kubernetes 的包管理工具。

类似 Linux 中：

```text
apt

yum
```

Helm 用于：

* 创建 Kubernetes 应用模板
* 管理应用生命周期
* 实现版本发布和回滚

Helm 核心组成：

```text
Chart

↓

Template

↓

Values

↓

Kubernetes Resource
```

---

# 11.2 为什么 Mall 项目引入 Helm

传统 Kubernetes 部署方式：

```text
mall-admin.yaml

mall-web.yaml

mysql.yaml

redis.yaml

ingress.yaml

configmap.yaml
```

存在问题：

## 1. 配置重复

不同环境：

```text
dev

test

prod
```

需要修改大量 YAML。

---

## 2. 参数管理困难

例如：

镜像版本：

```yaml
image:

  tag: latest
```

资源限制：

```yaml
resources:

 limits:

   cpu: 2
```

如果多个环境需要修改：

维护成本较高。

---

## 3. 不方便版本管理

如果升级失败：

需要手动恢复 YAML。

---

引入 Helm 后：

通过：

```yaml
values.yaml
```

统一管理变量。

---

# 11.3 Mall Helm Chart 目录结构

项目目录：

```text
Helm/

└── mall/

    ├── Chart.yaml

    ├── values.yaml

    ├── templates/

    │

    ├── deployment.yaml

    ├── service.yaml

    ├── ingress.yaml

    ├── configmap.yaml

    └── secret.yaml
```

---

# 11.4 Chart.yaml 配置

Chart.yaml 用于定义 Helm Chart 基础信息。

示例：

```yaml
apiVersion: v2

name: mall

description: Mall Kubernetes Deployment

type: application

version: 1.0.0

appVersion: "1.0"
```

主要字段：

| 字段          | 作用      |
| ----------- | ------- |
| name        | Chart名称 |
| version     | Chart版本 |
| appVersion  | 应用版本    |
| description | 描述信息    |

---

# 11.5 values.yaml 参数管理

values.yaml 用于统一管理部署参数。

例如：

```yaml
replicaCount: 1


image:

  repository: harbor.example.com/mall/mall-admin

  tag: latest


service:

  type: ClusterIP

  port: 8080


resources:

  requests:

    cpu: 500m

    memory: 512Mi

  limits:

    cpu: 1000m

    memory: 1Gi
```

部署时：

模板读取 values 参数。

---

# 11.6 Deployment 模板化

传统 Deployment：

```yaml
replicas: 1

image:

  mall-admin:v1
```

改为 Helm Template：

```yaml
replicas: {{ .Values.replicaCount }}


image:

{{ .Values.image.repository }}:{{ .Values.image.tag }}
```

优势：

不同环境只需要修改：

```yaml
values.yaml
```

无需修改模板。

---

# 11.7 使用 Helm 部署 Mall 应用

## 安装

进入 Helm 目录：

```bash
cd ~/mall-project/mall/Helm
```

执行：

```bash
helm install mall ./mall \
-n mall-prod
```

查看 Release：

```bash
helm list -n mall-prod
```

输出：

```text
NAME

mall


STATUS

deployed
```

---

# 11.8 Helm 查看部署状态

查看资源：

```bash
helm status mall \
-n mall-prod
```

查看 Kubernetes 资源：

```bash
kubectl get all \
-n mall-prod
```

---

# 11.9 Helm 升级应用

修改：

```yaml
values.yaml
```

例如：

镜像版本：

```yaml
tag: v2
```

执行：

```bash
helm upgrade mall ./mall \
-n mall-prod
```

Helm 会自动：

```text
更新Deployment

↓

创建新Pod

↓

滚动替换旧Pod
```

---

# 11.10 Helm 回滚

如果升级失败：

查看历史：

```bash
helm history mall \
-n mall-prod
```

执行回滚：

```bash
helm rollback mall 1 \
-n mall-prod
```

恢复之前版本。

---

# 11.11 Helm 与 CI/CD 集成

在 Jenkins Pipeline 中：

构建完成后：

```text
GitHub

↓

Jenkins

↓

Maven Build

↓

Docker Build

↓

Harbor Push

↓

Helm Upgrade

↓

Kubernetes Deployment
```

实现：

代码提交后自动发布。

---

# 11.12 Helm 实践总结

通过 Helm 对 Mall 项目进行模板化管理，实现：

* Kubernetes YAML 标准化管理
* 应用版本管理
* 参数集中配置
* 快速部署
* 快速升级
* 快速回滚

最终部署流程：

```text
开发代码

↓

Docker镜像

↓

Harbor仓库

↓

Helm Chart

↓

Kubernetes

↓

业务运行
```

通过 Helm 实践，将 Kubernetes 应用管理从手工维护 YAML 转变为标准化云原生交付方式。


---


# 第十二章 Kubernetes 安全与权限管理实践

在 Mall 云原生项目中，随着应用组件增加：

* 业务应用
* 数据库
* 中间件
* CI/CD
* 监控系统

需要对 Kubernetes 资源进行合理隔离和权限控制。

本项目主要实践：

* Namespace 资源隔离
* Secret 敏感信息管理
* ServiceAccount 身份认证
* RBAC 权限控制

---

# 12.1 Kubernetes Namespace 资源隔离

Namespace 是 Kubernetes 中用于资源逻辑隔离的机制。

项目中根据功能划分不同 Namespace：

```text id="5qk7e8"
mall-prod

    ↓

业务应用

    - mall-admin

    - mall-portal

    - mall-search

    - mysql

    - redis

    - elasticsearch


monitoring

    ↓

监控系统

    - Prometheus

    - Grafana

    - Exporter


kube-system

    ↓

Kubernetes系统组件

    - CoreDNS

    - Calico

    - Controller
```

---

## Namespace 创建

例如：

```bash id="8v0r4c"
kubectl create namespace mall-prod
```

查看：

```bash id="n5xq6s"
kubectl get namespace
```

---

## Namespace 作用

### 1. 资源隔离

不同业务之间：

```text id="6z5jtp"
mall-prod

≠

monitoring
```

避免资源混乱。

---

### 2. 权限隔离

可以针对不同 Namespace：

设置不同用户权限。

---

### 3. 资源限制

可以配置：

```text id="q8n3w4"
ResourceQuota

LimitRange
```

限制资源使用。

---

# 12.2 ConfigMap 配置管理

Kubernetes 中：

ConfigMap 用于保存非敏感配置。

Mall 项目中：

使用 ConfigMap 管理：

* 应用配置
* Logstash Pipeline
* Nginx 配置

---

## 创建 ConfigMap

示例：

```yaml id="2b5vxx"
apiVersion: v1

kind: ConfigMap

metadata:

  name: mall-config

  namespace: mall-prod


data:

  application.yml:

    server:

      port: 8080
```

---

## Pod 挂载 ConfigMap

方式一：

环境变量：

```yaml id="x8j8kv"
envFrom:

- configMapRef:

    name: mall-config
```

---

方式二：

Volume 挂载：

```yaml id="qv6n9f"
volumes:

- name: config

  configMap:

    name: mall-config
```

---

# 12.3 Secret 敏感信息管理

数据库密码、Token 等敏感信息：

不应该直接写入 YAML。

例如：

错误方式：

```yaml id="4qj7ka"
password: 123456
```

存在风险：

* 配置泄露
* Git 提交泄露
* 权限无法控制

---

## 使用 Secret

创建：

```bash id="s6q8pp"
kubectl create secret generic mysql-secret \
-n mall-prod \
--from-literal=password=123456
```

查看：

```bash id="j4v6ws"
kubectl get secret -n mall-prod
```

---

## Pod 使用 Secret

例如：

```yaml id="t9f2ap"
env:

- name: MYSQL_PASSWORD

  valueFrom:

    secretKeyRef:

      name: mysql-secret

      key: password
```

应用启动时：

从 Kubernetes Secret 获取密码。

---

# 12.4 ServiceAccount 身份认证

Kubernetes 中：

Pod 访问 Kubernetes API：

需要身份认证。

ServiceAccount：

提供 Pod 身份。

关系：

```text id="n0d5jh"
Pod

↓

ServiceAccount

↓

Token

↓

Kubernetes API
```

---

查看：

```bash id="j6t7rw"
kubectl get serviceaccount -n mall-prod
```

---

# 12.5 RBAC 权限控制

RBAC：

Role Based Access Control

基于角色的访问控制。

核心对象：

```text id="w2h6vz"
Role

↓

RoleBinding

↓

ServiceAccount
```

---

# 12.6 Jenkins Kubernetes Agent RBAC 实践

项目 CI/CD 中：

Jenkins 使用 Kubernetes Dynamic Agent。

流水线流程：

```text id="9m5w3x"
Jenkins

↓

创建临时Agent Pod

↓

执行Pipeline

↓

构建镜像

↓

部署Kubernetes
```

Agent 需要：

* 创建 Pod
* 查看 Pod
* 更新 Deployment

因此需要配置 RBAC。

---

## 创建 ServiceAccount

示例：

```yaml id="x4b8cz"
apiVersion: v1

kind: ServiceAccount

metadata:

  name: jenkins-agent

  namespace: mall-prod
```

---

## 创建 Role

限制 Jenkins 权限范围：

```yaml id="2k8y5m"
apiVersion: rbac.authorization.k8s.io/v1

kind: Role

metadata:

  name: jenkins-role

  namespace: mall-prod


rules:

- apiGroups:[""]

  resources:

  - pods

  - services

  verbs:

  - get

  - list

  - create

  - delete
```

---

## RoleBinding 绑定权限

```yaml id="3f6h2q"
apiVersion: rbac.authorization.k8s.io/v1

kind: RoleBinding

metadata:

  name: jenkins-binding

  namespace: mall-prod


subjects:

- kind: ServiceAccount

  name: jenkins-agent


roleRef:

  kind: Role

  name: jenkins-role

  apiGroup: rbac.authorization.k8s.io
```

---

# 12.7 最小权限原则

生产环境中：

不应该直接使用：

```text id="8p1h7r"
cluster-admin
```

原因：

权限过大。

例如：

Jenkins 如果拥有：

```text id="z6r9sx"
cluster-admin
```

一旦 Jenkins 泄露：

整个 Kubernetes 集群存在风险。

---

应该：

根据需求：

分配最小权限。

例如：

Jenkins：

只允许：

```text id="h3f8mz"
mall-prod namespace

create pod

update deployment

get service
```

---

# 12.8 Kubernetes 安全实践总结

通过 Mall 项目实践，实现：

```text id="n3y9cz"
Namespace

↓

ConfigMap

↓

Secret

↓

ServiceAccount

↓

RBAC

↓

应用权限控制
```

提升 Kubernetes 集群：

* 配置安全性
* 权限管理能力
* CI/CD 安全性

同时理解生产环境 Kubernetes：

不是所有组件都应该拥有管理员权限。

合理的权限设计：

可以降低误操作风险，提高系统安全性。

---

# 第十三章 项目生产化优化与未来演进方向

Mall 云原生项目目前已经完成：

* Kubernetes 容器化部署
* 服务编排
* 持久化存储
* 监控体系建设
* 日志体系建设
* CI/CD 自动化发布

形成了一套完整的云原生应用交付流程。

当前实验环境基于：

```text id="kq8z1p"
单物理机 Kubernetes 集群
```

主要目标是：

验证 Kubernetes、DevOps、监控、日志等技术体系。

在真实生产环境中，还需要进一步优化。

---

# 13.1 Kubernetes 高可用架构优化

## 当前架构

当前：

```text id="9v2w6d"
物理机

↓

Kubernetes

↓

业务Pod
```

所有组件运行在单节点环境。

优点：

* 部署简单
* 学习成本低
* 方便测试

缺点：

* 节点故障影响全部业务
* 无法实现高可用

---

## 生产环境优化

生产 Kubernetes 通常采用：

```text id="m7p5zk"
Load Balancer

        |

        |

+----------------+

| Master Node 1 |

| Master Node 2 |

| Master Node 3 |

+----------------+

        |

        |

Worker Node

Worker Node

Worker Node
```

---

优化方向：

### 1. 多 Master 节点

保证 Kubernetes 控制面高可用。

组件：

* kube-apiserver
* etcd
* controller-manager
* scheduler

---

### 2. etcd 高可用

etcd 保存 Kubernetes 集群状态。

生产环境：

通常采用：

```text id="2m6f4s"
3节点 etcd 集群
```

避免单节点数据丢失。

---

# 13.2 存储系统生产化优化

## 当前方案

项目中：

使用 Kubernetes：

```text id="y6j3wx"
PV

PVC

StorageClass
```

完成数据持久化。

---

## 生产环境优化

实验环境：

```text id="q2w8nz"
本地磁盘
```

生产环境：

可以使用：

### 云环境

例如：

* 阿里云云盘 CSI
* AWS EBS
* Azure Disk

---

### 私有化环境

例如：

* NFS
* Ceph
* Longhorn

---

生产存储需要考虑：

* 数据可靠性
* 多节点访问
* 自动扩容
* 数据备份

---

# 13.3 监控体系生产化优化

当前：

```text id="9r4v8n"
Prometheus

+

Grafana
```

已经实现：

* Kubernetes 指标采集
* Exporter 监控
* Dashboard 展示

---

生产环境进一步增加：

## AlertManager 告警

实现：

```text id="z7k5q2"
Prometheus

↓

AlertManager

↓

企业微信/钉钉/邮件

↓

运维人员
```

例如：

CPU 使用率过高：

```text
CPU > 90%

持续5分钟

触发告警
```

---

## SLO / SLA 管理

从：

关注资源指标：

```text
CPU

Memory

Disk
```

升级到：

业务指标：

```text
接口成功率

响应时间

可用性
```

---

# 13.4 日志系统生产化优化

当前：

```text id="p9h3cz"
Elasticsearch

↓

Logstash

↓

Kibana
```

实现：

日志采集和查询。

---

生产环境优化：

增加：

## Filebeat

应用：

```text id="m8d2vz"
Container Log

↓

Filebeat

↓

Logstash

↓

Elasticsearch
```

优势：

* 轻量
* 资源占用低
* 专门负责日志采集

---

## Kafka 缓冲

大型生产环境：

```text id="w6s1qa"
Filebeat

↓

Kafka

↓

Logstash

↓

Elasticsearch
```

解决：

* 日志突发
* 消费速度不匹配

---

# 13.5 CI/CD 流程优化

当前流程：

```text id="x8c3vp"
GitHub

↓

Jenkins

↓

Maven

↓

Docker

↓

Harbor

↓

Kubernetes
```

已经实现自动化发布。

---

生产环境优化：

## GitOps

引入：

* ArgoCD
* Flux

流程：

```text id="f4m8jd"
开发提交代码

↓

CI构建镜像

↓

修改Git配置仓库

↓

ArgoCD同步

↓

Kubernetes发布
```

优势：

* 声明式部署
* 自动同步
* 回滚方便

---

# 13.6 镜像安全优化

当前：

```text id="v5r2hm"
Harbor
```

负责：

* 私有镜像存储
* 镜像管理

---

生产环境增加：

## 镜像漏洞扫描

例如：

* Trivy
* Clair

流程：

```text id="q8m1kc"
Docker Build

↓

漏洞扫描

↓

通过

↓

Push Harbor
```

---

## 镜像签名

保证：

部署镜像来源可信。

---

# 13.7 Kubernetes 安全增强

当前：

已经实现：

* Secret
* RBAC
* ServiceAccount

进一步：

增加：

## NetworkPolicy

限制：

Pod之间访问。

例如：

```text id="f5k2mm"
Frontend

↓

允许访问

Backend


Backend

↓

允许访问

Database
```

禁止：

任意 Pod 互通。

---

## Pod Security

限制：

* 特权容器
* Root 用户
* HostPath

提升运行安全。

---

# 13.8 备份与灾备

生产环境需要考虑：

## Kubernetes 资源备份

例如：

* Velero

备份：

* Deployment
* Service
* ConfigMap
* Secret

---

## 数据备份

例如：

数据库：

* MySQL 定时备份
* binlog

Elasticsearch：

* Snapshot

---

# 13.9 项目演进路线总结

Mall 项目当前：

```text id="x3z7kd"
Docker

↓

Kubernetes

↓

Prometheus/Grafana

↓

ELK

↓

Jenkins CI/CD

↓

Helm
```

生产化演进：

```text id="w9m4pz"
高可用 Kubernetes

↓

云原生存储

↓

GitOps

↓

安全增强

↓

自动化运维

↓

灾备体系
```

通过该项目实践，不仅完成 Kubernetes 基础部署和应用运行，也进一步理解真实生产环境中云原生平台需要关注的：

* 可用性
* 可扩展性
* 安全性
* 自动化
* 可维护性


---

# 第十四章 项目总结

通过 Mall 云原生 DevOps 实践项目，围绕一个完整电商业务系统，完成了从传统应用部署方式向云原生架构迁移的全过程。

项目重点不在业务代码开发，而是围绕：

* 容器化
* Kubernetes 编排
* 自动化交付
* 监控体系
* 日志体系
* 运维自动化

完成完整的 DevOps 实践。

---

# 14.1 项目整体架构

最终形成的整体架构如下：

```text id="1l4s8z"
                Developer

                    |

                    |

                 GitHub

                    |

                    |

                Jenkins CI/CD

                    |

        +-----------+-----------+

        |                       |

     Maven Build          Docker Build

                                |

                                |

                             Harbor

                                |

                                |

                           Kubernetes

                                |

        +-----------------------+-----------------------+

        |                       |                       |

     Deployment             Service                 Ingress

        |

        |

      Pod


        |

        +-------------------------------+

        |                               |

   Prometheus/Grafana              ELK Stack

        |                               |

   Metrics Monitoring             Log Analysis

```

---

# 14.2 完成的主要实践内容

## 1. Kubernetes 集群搭建

独立完成单物理机 Kubernetes 环境搭建。

包括：

* Ubuntu 环境配置
* containerd 安装配置
* Kubernetes 部署
* Calico 网络插件
* MetalLB LoadBalancer 实现

掌握：

* Kubernetes 集群初始化
* 网络组件配置
* 节点问题排查

---

## 2. 应用容器化

针对 Mall 应用进行 Docker 化。

完成：

* Dockerfile 编写
* 镜像构建
* 镜像优化
* Harbor 私有仓库管理

实现：

```text id="9r2x5n"
源码

↓

Docker Image

↓

Harbor

↓

Kubernetes
```

---

## 3. Kubernetes 应用部署

完成业务系统 Kubernetes 化。

涉及资源：

* Namespace
* Deployment
* Service
* Ingress
* ConfigMap
* Secret
* PVC

实现：

应用由传统服务器部署方式迁移到 Kubernetes 平台运行。

---

## 4. Kubernetes 生产化优化

针对应用运行稳定性进行优化。

包括：

### 健康检查

配置：

* StartupProbe
* ReadinessProbe
* LivenessProbe

实现：

自动检测应用状态。

---

### 资源管理

配置：

* Resource Requests
* Resource Limits

避免：

单个应用影响节点稳定性。

---

### 自动扩缩容

配置：

* HPA

实现：

根据资源使用情况自动调整副本数量。

---

## 5. 持久化存储建设

针对有状态服务：

* Elasticsearch
* Prometheus

配置：

* PV
* PVC
* StorageClass

实现：

Pod 重建后数据仍然保留。

---

## 6. 监控与日志体系建设

搭建完整可观测体系。

监控：

```text id="6q4k7s"
Prometheus

↓

Grafana

↓

Dashboard
```

实现：

* Kubernetes 指标监控
* Node 资源监控
* Pod 状态监控
* Exporter 监控

日志：

```text id="7n9v2m"
Logstash

↓

Elasticsearch

↓

Kibana
```

实现：

* 应用日志采集
* 日志查询
* 故障分析

---

## 7. CI/CD 自动化发布

建设：

GitHub + Jenkins + Harbor + Kubernetes 自动化发布流程。

实现：

```text id="q2m8yx"
代码提交

↓

Jenkins Pipeline

↓

Maven 编译

↓

Docker Build

↓

Push Harbor

↓

Helm Upgrade

↓

Kubernetes 发布
```

实现：

代码提交后自动完成应用部署。

---

## 8. Helm 应用管理

使用 Helm 对 Kubernetes 应用进行模板化管理。

实现：

* Chart 管理
* values 参数配置
* 应用升级
* 应用回滚

提升：

应用部署标准化能力。

---

## 9. Kubernetes 故障排查能力

通过实际部署过程中遇到的问题，形成 Kubernetes 分层排查方法。

包括：

### Pod 问题

例如：

* CrashLoopBackOff
* ImagePull


---

# 项目目录结构

Mall 云原生 DevOps 项目按照功能模块进行划分，主要包含：

* 应用代码
* Docker 镜像构建
* Kubernetes 部署文件
* CI/CD 配置
* 监控配置
* Helm Chart

整体目录结构如下：

```text
mall/

├── Application/

│   ├── mall-admin

│   ├── mall-portal

│   ├── mall-search

│   └── ...

│
├── Docker/

│   ├── Dockerfile

│   └── docker-compose.yaml

│
├── Kubernetes/

│   ├── namespace.yaml

│   ├── deployment/

│   ├── service/

│   ├── ingress/

│   ├── configmap/

│   ├── secret/

│   └── storage/

│
├── Helm/

│   └── mall/

│       ├── Chart.yaml

│       ├── values.yaml

│       └── templates/

│
├── CICD/

│   ├── Jenkinsfile

│   └── pipeline/

│
├── Monitoring/

│   ├── prometheus/

│   ├── grafana/

│   ├── servicemonitor/

│   └── exporters/

│
├── README.md

└── LICENSE
```

---

# 目录说明

## Application

存放 Mall 原始业务代码。

主要包含：

* 后端服务
* 前端服务
* Maven 项目

通过 Docker 构建为 Kubernetes 使用的镜像。

---

## Docker

存放容器化相关文件。

包括：

* Dockerfile
* 镜像构建脚本

主要流程：

```text
源码

↓

Docker Build

↓

镜像

↓

Harbor
```

---

## Kubernetes

存放 Kubernetes 部署资源。

包括：

### Namespace

用于资源隔离。

### Deployment

管理应用 Pod。

### Service

提供服务发现。

### Ingress

提供外部访问入口。

### ConfigMap / Secret

管理应用配置和敏感信息。

### Storage

管理：

* PV
* PVC
* StorageClass

---

## Helm

存放 Helm Chart。

用于：

* Kubernetes 模板化部署
* 参数管理
* 版本升级
* 回滚

---

## CICD

存放 Jenkins CI/CD 配置。

实现：

```text
GitHub

↓

Jenkins

↓

Maven

↓

Docker

↓

Harbor

↓

Kubernetes
```

自动化发布流程。

---

## Monitoring

存放监控相关配置。

包括：

### Prometheus

指标采集。

### Grafana

Dashboard 展示。

### ServiceMonitor

自动发现监控目标。

### Exporter

采集中间件指标。

---

# 项目整体交付流程

```text
开发代码

↓

GitHub

↓

Jenkins Pipeline

↓

Docker Image

↓

Harbor

↓

Helm

↓

Kubernetes

↓

Prometheus/Grafana

↓

ELK日志系统
```

最终形成完整云原生 DevOps 项目闭环。


---

# 第十六章 项目部署流程

本项目基于 Kubernetes 平台运行，通过 Docker 完成应用容器化，通过 Harbor 管理镜像，通过 Kubernetes 完成应用编排，最终实现完整云原生部署流程。

整体部署流程：

```text id="c8m3qv"
环境准备

↓

Kubernetes 集群

↓

Harbor镜像仓库

↓

Docker镜像构建

↓

Kubernetes资源部署

↓

Ingress访问

↓

监控与日志接入
```

---

# 16.1 环境准备

## 硬件环境

实验环境：

```text id="7z9kdm"
单物理机 Kubernetes 集群
```

主要组件：

| 组件         | 版本           |
| ---------- | ------------ |
| 操作系统       | Ubuntu 24.04 |
| Docker     | 29.x         |
| containerd | 1.x          |
| Kubernetes | 1.35.x       |
| Helm       | 3.x          |

---

# 16.2 Kubernetes 集群部署

初始化 Kubernetes 环境。

主要步骤：

## 安装容器运行时

部署：

```text id="w3x8k1"
containerd
```

配置 Kubernetes 使用 containerd 作为 Runtime。

---

## 初始化 Kubernetes

完成：

* kubeadm 初始化
* kubelet 配置
* kubectl 管理

验证：

```bash id="4n7vkm"
kubectl get nodes
```

查看节点状态：

```text id="9s4xkq"
NAME

STATUS

k8s-master

Ready
```

---

## 部署网络插件

安装 Calico：

作用：

* Pod 网络通信
* 网络策略支持

验证：

```bash id="5kz2ny"
kubectl get pods -n kube-system
```

---

## 部署 MetalLB

由于实验环境没有云厂商 LoadBalancer：

使用 MetalLB 提供 LoadBalancer 能力。

实现：

```text id="7xq8bw"
Service Type=LoadBalancer

↓

MetalLB

↓

物理网络IP
```

---

# 16.3 Harbor 镜像仓库配置

项目使用 Harbor 作为私有镜像仓库。

镜像流程：

```text id="8d4m6x"
源码

↓

Docker Build

↓

Harbor Push

↓

Kubernetes Pull
```

---

登录 Harbor：

```bash id="k9w3zs"
docker login harbor.xxx.com
```

构建镜像：

```bash id="m6q8tp"
docker build \
-t harbor.xxx.com/mall/mall-admin:v1 .
```

推送：

```bash id="v8c2rm"
docker push \
harbor.xxx.com/mall/mall-admin:v1
```

---

# 16.4 Kubernetes 应用部署

进入 Kubernetes 配置目录：

```bash id="x6p2qv"
cd Kubernetes
```

创建 Namespace：

```bash id="g8m5qs"
kubectl apply -f namespace.yaml
```

---

部署 ConfigMap：

```bash id="r9z6fw"
kubectl apply -f configmap/
```

---

部署 Secret：

```bash id="k3v7zs"
kubectl apply -f secret/
```

---

部署应用：

```bash id="q8m4nt"
kubectl apply -f deployment/
```

---

创建 Service：

```bash id="f2q7xm"
kubectl apply -f service/
```

---

配置 Ingress：

```bash id="j6w9kp"
kubectl apply -f ingress/
```

---

查看运行状态：

```bash id="p7x3mv"
kubectl get pods -n mall-prod
```

正常状态：

```text id="n4v8qw"
Running
```

---

# 16.5 Helm 部署

使用 Helm 管理 Kubernetes 应用。

安装：

```bash id="z5m2ck"
helm install mall ./Helm/mall \
-n mall-prod
```

查看：

```bash id="v3q9sx"
helm list -n mall-prod
```

升级：

```bash id="b6k8rm"
helm upgrade mall ./Helm/mall \
-n mall-prod
```

---

# 16.6 CI/CD 自动发布

项目通过 Jenkins 实现自动化发布。

流程：

```text id="p4n8yz"
代码提交

↓

GitHub

↓

Jenkins Trigger

↓

Maven Build

↓

Docker Build

↓

Push Harbor

↓

Helm Upgrade

↓

Kubernetes滚动更新
```

发布完成后：

检查：

```bash id="t7x5mq"
kubectl rollout status deployment \
mall-admin \
-n mall-prod
```

---

# 16.7 监控系统部署

部署 Prometheus + Grafana。

包括：

* Prometheus Server
* Grafana
* Exporter
* ServiceMonitor

查看：

```bash id="w9k3zp"
kubectl get pods -n monitoring
```

访问 Grafana：

```text id="r6m2vx"
http://<IP>:3000
```

查看：

* Kubernetes资源
* Node状态
* Pod状态
* Middleware指标

---

# 16.8 日志系统部署

部署：

```text id="z8q5ny"
Elasticsearch

↓

Logstash

↓

Kibana
```

日志流程：

```text id="q6m8vw"
Container stdout

↓

Logstash

↓

Elasticsearch

↓

Kibana查询
```

访问 Kibana：

```text id="h4p7ms"
http://<IP>:5601
```

---

# 16.9 部署验证

最终验证：

## Kubernetes

```bash
kubectl get pods -n mall-prod
```

所有业务 Pod：

```text
Running
```

---

## Service

```bash
kubectl get svc -n mall-prod
```

确认：

* ClusterIP
* LoadBalancer

---

## Ingress

验证访问入口：

```bash
curl http://mall.example.com
```

---

## CI/CD

验证 Jenkins：

```text
Build Success
```

---

## Monitoring

验证 Grafana：

```text
Dashboard正常展示
```

---

# 16.10 项目启动总结

完整启动链路：

```text id="w6x9mq"
Kubernetes环境

↓

Harbor镜像仓库

↓

Mall应用部署

↓

Ingress流量入口

↓

PVC数据持久化

↓

Prometheus监控

↓

ELK日志

↓

Jenkins自动发布
```

通过以上流程，可以完成 Mall 电商系统从代码到生产运行环境的完整云原生部署。


---

# 第十七章 项目运行效果展示

本章节主要展示 Mall 云原生 DevOps 项目的实际运行效果。

通过 Kubernetes、CI/CD、监控、日志等系统截图，直观展示项目运行状态。

---

# 17.1 Kubernetes 集群运行状态

通过 Kubernetes 查看节点状态：

```bash
kubectl get nodes
```

运行结果：

```text
NAME          STATUS    ROLES

k8s-master    Ready     control-plane
```

说明：

Kubernetes 集群正常运行。

截图：

> Kubernetes Node 状态截图

---

# 17.2 Mall 应用运行状态

查看 Mall 应用 Pod：

```bash
kubectl get pods -n mall-prod
```

运行结果：

```text
NAME                         READY   STATUS

mall-admin                   1/1     Running

mall-portal                  1/1     Running

mall-search                  1/1     Running

mysql                        1/1     Running

redis                        1/1     Running
```

说明：

业务应用已经成功运行在 Kubernetes 平台。

截图：

> Kubernetes Pod运行截图

---

# 17.3 Kubernetes Service 与 Ingress

查看 Service：

```bash
kubectl get svc -n mall-prod
```

展示：

* ClusterIP
* LoadBalancer

通过 Ingress 提供外部访问入口。

查看：

```bash
kubectl get ingress -n mall-prod
```

实现：

```text
用户

↓

Ingress Controller

↓

Service

↓

Pod
```

截图：

> Ingress访问配置截图

---

# 17.4 Harbor 镜像仓库

项目使用 Harbor 管理 Kubernetes 使用的镜像。

镜像流程：

```text
Docker Build

↓

Harbor Push

↓

Kubernetes Pull
```

Harbor 中保存：

```text
mall-admin

mall-portal

mall-search

```

截图：

> Harbor 镜像仓库截图

---

# 17.5 Jenkins CI/CD 流水线

项目通过 Jenkins 实现自动化发布。

Pipeline 流程：

```text
GitHub

↓

Jenkins

↓

Maven Build

↓

Docker Build

↓

Push Harbor

↓

Helm Upgrade

↓

Kubernetes Deployment
```

构建成功后：

```text
BUILD SUCCESS
```

截图：

> Jenkins Pipeline执行成功截图

---

# 17.6 Grafana 监控展示

项目部署 Prometheus + Grafana 监控体系。

监控内容包括：

## Kubernetes资源

* Node CPU
* Node Memory
* Node Disk

## Pod状态

* Pod数量
* Running Pod
* Pending Pod
* Restart次数

## 中间件监控

* MySQL Exporter
* Redis Exporter
* Elasticsearch Exporter
* RabbitMQ Exporter
* Logstash Exporter

Dashboard 示例：

```text
Mall Overview Dashboard
```

截图：

> Grafana Dashboard截图

---

# 17.7 Prometheus Target状态

Prometheus 自动发现监控目标。

查看：

```text
Status

↓

Targets
```

正常状态：

```text
UP
```

包括：

* Kubernetes组件
* Node Exporter
* Middleware Exporter
* Logstash Exporter

截图：

> Prometheus Targets截图

---

# 17.8 ELK 日志系统展示

项目部署：

```text
Elasticsearch

↓

Logstash

↓

Kibana
```

日志流程：

```text
Pod stdout

↓

Logstash采集

↓

Elasticsearch存储

↓

Kibana查询
```

Kibana 中可以查询：

* 应用日志
* 容器日志
* 错误信息

截图：

> Kibana日志查询截图

---

# 17.9 Helm 部署管理

查看 Helm Release：

```bash
helm list -n mall-prod
```

结果：

```text
NAME

mall

STATUS

deployed
```

说明：

应用已经通过 Helm 进行生命周期管理。

截图：

> Helm Release截图

---

# 17.10 项目最终效果

最终 Mall 云原生 DevOps 平台实现：

```text
                GitHub

                   |

                   |

               Jenkins CI/CD

                   |

                   |

                Harbor

                   |

                   |

              Kubernetes

                   |

        +----------+----------+

        |                     |

     Mall业务              Middleware


        |

        |

 Prometheus/Grafana


        |

        |

    ELK日志系统

```

实现：

* 应用容器化
* Kubernetes自动部署
* 镜像统一管理
* 自动化发布
* 资源监控
* 日志分析
* 故障排查

最终形成完整：

```text
Cloud Native DevOps Workflow
```


---



