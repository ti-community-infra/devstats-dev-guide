# 架构

## 整体架构

### 架构图

![arch](../img/arch.png)

### 架构说明

当贡献者们在 GitHub 上活动时，我们会采用定时任务的方式每隔一小时将数据同步到 PostgreSQL 中，然后通过 Grafana 将数据展示出来给大家看。另外我们利用 Travis CI 对我们的改动进行测试和部署。

## 核心组件

- [Grafana](https://github.com/cncf/devstats-docker-images/blob/master/images/Dockerfile.grafana) 用于展示数据
- [PostgreSQL](https://github.com/cncf/devstats-docker-images/blob/master/images/Dockerfile.patroni) 用于存储数据
- [Provisions](https://github.com/cncf/devstats-docker-images/blob/master/images/Dockerfile.full.test) 用于从各个数据源收集数据
  - 在该组件中主要是使用 [devstatscode](https://github.com/cncf/devstatscode) 的代码来从各个数据源收集数据
