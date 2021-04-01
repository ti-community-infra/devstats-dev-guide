# 数据来源

## GH Archive

### 介绍

[GH Archive] 是一个记录 GitHub 公共时间轴的项目，将其归档，并使其易于访问，以便进一步分析。

### 记录形式

[GH Archive] 会将 GitHub 所有的[事件](https://docs.github.com/en/developers/webhooks-and-events/webhook-events-and-payloads)通过 API [爬取](https://github.com/igrigorik/gharchive.org/tree/master/crawler)并存储为 JSON 文件，然后将其整理上传到 Google Big Query 的公开数据集。

另外， [GH Archive] 在爬取过程中会对邮件信息进行[混淆](https://github.com/igrigorik/gharchive.org/blob/a5fb1a4907e26d527f317e5494ebf573864e3d86/crawler/crawler.rb#L40)，防止数据集被利用暴露个人隐私。

devstats 会直接分析这些 JSON 文件作为数据来源。

## GitHub API

### 介绍

GitHub 提供了两套 API 来让用户获取 GitHub 上几乎所有的数据：

- [REST API](https://docs.github.com/en/rest)
- [GraphQL API](https://docs.github.com/en/graphql)

### 记录形式

无论是 [REST API] 还是 [GraphQL API] 都可以以 JSON 的格式返回数据，devstats 会定时的去使用 API 获取数据作为数据来源。

## Git

### 介绍

[Git] 作为一个分布式版本控制系统，存储了代码仓库所有的分支上的所有 commit。

### 记录形式

[Git] 存储了代码仓库的所有 commit，devstats 会克隆仓库，并从中获取文件级别的 commit 信息作为数据来源。

## CNCF gitdm

### 介绍

[CNCF gitdm] 是 CNCF 对 [gitdm](https://lwn.net/Articles/290957) 的一个 fork，用来计算开发者及其公司对开源项目的贡献。

### 记录形式

[CNCF gitdm] 将数据存储为 txt 和 JSON 文件，并且保持每个月一两次的更新。devstats 会将数据以 [JSON](https://github.com/cncf/devstats/blob/master/github_users.json) 的格式导入到其中来作为数据来源。

[gh archive]: https://github.com/reactjs/rfcs
[rest api]: https://docs.github.com/en/rest
[graphql api]: https://docs.github.com/en/graphql
[git]: https://git-scm.com
[cncf gitdm]: https://github.com/cncf/gitdm
