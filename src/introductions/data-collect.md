# 数据收集方式

## GH Archive

### 收集方式

devstats 会一次性将 [GH Archive] 所有的 JSON 文件逐个下载下来进行整合然后存储到 PostgreSQL 中，并且在该过程中会将数据分为固定数据和可变数据，并且用 event_id 作为可变数据的主键来进行维护和更新。

### 收集工具

devstats 使用 [gha2db](https://github.com/cncf/devstatscode/blob/master/cmd/gha2db/gha2db.go) 工具来分析和解析数据，并将数据存储到 PostgreSQL 中。

## GitHub API

### 收集方式

devstats 主要利用 GitHub API 来补充和较正 [GH Archive] 中缺失或错误的数据。devstats 会通过定时任务来调用 GitHub API 获取两个小时以内的相关改动，并且把这些数据更新到 PostgreSQL 中。例如：Issue/PR 当前的 Label 或者 Milestone 信息，就需要 API 尽快的来收集和更新。

### 收集工具

devstats 使用 [ghapi2db](https://github.com/cncf/devstatscode/blob/master/cmd/ghapi2db/ghapi2db.go) 来调用 GitHub API 拉取数据并将数据存储到 PostgreSQL 中。

## Git

### 收集方式

devstats 会将需要分析的仓库直接 clone 或者 pull 到本地，然后查看整个仓库的所有 git log 并对它进行分析。devstats 会直接通过提交的 SHA 来定位改动并且统计改动大小信息。

### 收集工具

devstats 使用 [get_repos](https://github.com/cncf/devstatscode/blob/master/cmd/get_repos/get_repos.go) 来 clone/pull 仓库，并对仓库的 git log 逐一分析。

## CNCF gitdm

### 收集方式

[CNCF gitdm] 提供了一个所有公司信息的 [JSON 文件](https://github.com/cncf/devstats/blob/master/github_users.json)，是一个 Git LFS，devstats 会将所有的数据通过该 JSON 文件直接导入。devstats 会从中解析 GitHub 信息并且其他数据结合，这样我们就收集到了大部分贡献者的公司信息。

### 收集工具

devstats 使用 [import_affs](https://github.com/cncf/devstatscode/blob/master/cmd/import_affs/import_affs.go) 来解析 JSON 文件将员工公开信息存储到数据库中。

[gh archive]: https://www.gharchive.org
[cncf gitdm]: https://github.com/cncf/gitdm
