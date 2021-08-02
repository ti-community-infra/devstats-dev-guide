# 视图

GhArchive 上的数据是用户在 GitHub
上产生的公开活动的事件数据，然而在一些查询场景下，我们往往只需要针对数据对象的最新状态进行查询，而不需要遍历过往的历史数据。为了简化这些查询工作，可以通过数据库的视图功能，根据历史数据来构建数据对象最新状态的模型。

## Pull Requests

视图名：`pull_requests`

| 字段名        | 数据类型  | 附加说明                                                     |
|--------------|----------|------------------------------------------------------------|
| event_id     | bigint   |                                                            |
| id           | bigint   |                                                            |
| repo_name    | int      | 注意：仓库可能存在更名行为                                     |
| number       | int      | 注意：Issue 与 Pull Request 的 Number 编号是统一的            |
| title        | text     |                                                            |
| state        | text     | Pull Request 的状态，可以分为：`open` / `closed`              |
| body         | text     |                                                            |
| assignee_id  | bigint   |                                                            |
| milestone_id | bigint   |                                                            |
| milestone    | text     |                                                            |
| creator_id   | bigint   | 创建者的 ID，可以通过连接 `gha_actor` 获取创建者的更多信息        |
| created_at   | bigint   |                                                            |
| updated_at   | datetime |                                                            |
| closed_at    | datetime |                                                            |
| merged_at    | datetime |                                                            |
| comments     | int      | 评论的数量                                                   |


## Issues

视图名：`issues`

| 字段名          | 数据类型    | 附加说明                                            |
|-----------------|----------|----------------------------------------------------|
| event_id        | bigint   |                                                    |
| id              | bigint   |                                                    |
| repo_id         | bigint   |                                                    |
| repo_name       | text     | 注意：仓库可能存在更名行为                              |
| number          | Integer  | 注意：Issue 与 Pull Request 的 Number 编号是统一的     |
| title           | text     |                                                    |
| state           | text     | Issue 的状态，可以分为：`open` / `closed`             |
| is_pull_request | bool     |                                                    |
| body            | text     |                                                    |
| assignee_id     | bigint   |                                                    |
| milestone_id    | bigint   |                                                    |
| milestone       | text     |                                                    |
| creator_id      | bigint   |                                                    |
| created_at      | datetime |                                                    |
| updated_at      | datetime | 最后的更新时间                                       |
| closed_at       | datetime |                                                    |
| comments        | Integer  | 评论的数量                                           |

## Issue Labels

视图名：`issue_labels`

| 字段名      | 数据类型  | 附加说明                                    |
|------------|----------|--------------------------------------------|
| issue_id   | bigint   |                                            |
| label_id   | bigint   |                                            |
| full_label | text     | 完整的标签名称，格式如：`<prefix>/<label>`     |
| prefix     | text     | 标签的前缀，默认为 general，表示没有前缀        |
| label      | text     | 标签去掉前缀后剩下的部分                       |

## Milestones

视图名：`milestones`

| 字段名      | 数据类型  | 附加说明                    |
|------------|----------|----------------------------|
| event_id   | bigint   |                            |
| id         | bigint   |                            |
| repo_id    | bigint   |                            |
| repo_name  | text     | 注意：仓库可能存在更名行为 |
| milestone  | text     |                            |
| state      | text     |                            |
| created_at | datetime |                            |
| updated_at | datetime |                            |
| closed_at  | datetime |                            |

## Comments

视图名：`comments`

| 字段名     | 数据类型  | 附加说明                                                            |
|-----------|----------|--------------------------------------------------------------------|
| event_id  | bigint   |                                                                    |
| id        | bigint   |                                                                    |
| type      | varchar  | 可选类型：commit comment / review comment / review / issue comment   |
| body      | text     |                                                                    |
| commit_id | bigint   |                                                                    |
| diff_hunk | text     |                                                                    |


