# 数据库表

## 概述

单个数据库中包含了以下几类数据库表：

- 以 `gha_` 为前缀的数据表存放的是从 GHArchive 中爬取过来经过结构化存储起来的基础数据
- 以 `s` 为前缀的数据库表存放的是 series 数据，是 devstats 将基础数据经过指标计算出来的时序数据，这些数据会被 Grafana 通过查询得到，以图表或表格形式进行展示
- 以 `t` 为前缀的数据库表存放的 tags 数据，是 devstats 从基础数据查询得到的一些列表数据，这些数据会在指标数据计算过程中、构建多列宽表表结构、Grafana 图表的下拉选项列表中使用到
