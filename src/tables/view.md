# 视图

GhArchive 上的数据是用户在 GitHub
上产生的公开活动的事件数据，然而在一些查询场景下，我们往往只需要针对数据对象的最新状态进行查询，而不需要遍历过往的历史数据。为了简化这些查询工作，可以通过数据库的视图功能，根据历史数据来构建数据对象最新状态的模型。

## Pull Requests

视图名：`pull_requests`

| 字段名       | 数据类型 | 附加说明                                                   |
| ------------ | -------- | ---------------------------------------------------------- |
| `event_id`     | bigint   |                                                            |
| `id`           | bigint   |                                                            |
| `repo_name`    | int      | 注意：仓库可能存在更名行为                                 |
| `number`       | int      | 注意：Issue 与 Pull Request 的 Number 编号是统一的         |
| `title`        | text     |                                                            |
| `state`        | text     | Pull Request 的状态，可以分为：`open` / `closed`           |
| `body`         | text     |                                                            |
| `assignee_id`  | bigint   |                                                            |
| `milestone_id` | bigint   |                                                            |
| `milestone`    | text     |                                                            |
| `creator_id`   | bigint   | 创建者的 ID，可以通过连接 `gha_actor` 获取创建者的更多信息 |
| `created_at`   | bigint   |                                                            |
| `updated_at`   | datetime |                                                            |
| `closed_at`    | datetime |                                                            |
| `merged_at`    | datetime |                                                            |
| `comments`     | int      | 评论的数量                                                 |


## Pull Request Assignees

Pull Request 当前分配的 Assignees。

视图名：`pull_request_assignees`

| 字段名            | 数据类型   | 附加说明                          |
| ---------------- | --------- | -------------------------------- |
| `pull_request_id` | bigint   |                                   |
| `assignee_id`     | bigint   | 可与 `gha_actor` 基础数据表进行连接   |

## Pull Request Requested Reviewers

Pull Request 当前请求过的 reviewers。

视图名：`pull_request_requested_reviewers`

| 字段名                          | 数据类型   | 附加说明                          |
| ------------------------------ | --------- | -------------------------------- |
| `pull_request_id`              | bigint   |                                   |
| `requested_reviewer_id`        | bigint   | 可与 `gha_actor` 基础数据表进行连接   |

## Issues

视图名：`issues`

| 字段名          | 数据类型 | 附加说明                                           |
| --------------- | -------- | -------------------------------------------------- |
| `event_id`        | bigint   |                                                    |
| `id`              | bigint   |                                                    |
| `repo_id`         | bigint   |                                                    |
| `repo_name`       | text     | 注意：仓库可能存在更名行为                         |
| `number`          | Integer  | 注意：Issue 与 Pull Request 的 Number 编号是统一的 |
| `title`           | text     |                                                    |
| `state`           | text     | Issue 的状态，可以分为：`open` / `closed`          |
| `is_pull_request` | bool     |                                                    |
| `body`            | text     |                                                    |
| `assignee_id`     | bigint   |                                                    |
| `milestone_id`    | bigint   |                                                    |
| `milestone`       | text     |                                                    |
| `creator_id`      | bigint   |                                                    |
| `created_at`      | datetime |                                                    |
| `updated_at`      | datetime | 最后的更新时间                                     |
| `closed_at`       | datetime |                                                    |
| `comments`        | Integer  | 评论的数量                                         |

## Issue Labels

视图名：`issue_labels`

| 字段名     | 数据类型 | 附加说明                                   |
| ---------- | -------- | ------------------------------------------ |
| `issue_id`   | bigint   |                                            |
| `label_id`   | bigint   |                                            |
| `full_label` | text     | 完整的标签名称，格式如：`<prefix>/<label>` |
| `prefix`     | text     | 标签的前缀，默认为 general，表示没有前缀   |
| `label`      | text     | 标签去掉前缀后剩下的部分                   |

## Milestones

视图名：`milestones`

| 字段名       | 数据类型   | 附加说明                       |
| ------------ | -------- | ----------------------------- |
| `event_id`   | bigint   |                            |
| `id`         | bigint   |                            |
| `repo_id`    | bigint   |                            |
| `repo_name`  | text     | 注意：仓库可能存在更名行为     |
| `milestone`  | text     |                            |
| `state`      | text     |                            |
| `created_at` | datetime |                            |
| `updated_at` | datetime |                            |
| `closed_at`  | datetime |                            |

## Comments

视图名：`comments`

| 字段名                 | 数据类型 | 附加说明                                                              |
|------------------------|----------|-------------------------------------------------------------------|
| `event_id`               | bigint   |                                                                 |
| `id`                     | bigint   |                                                                 |
| `repo_id`                | bigint   |                                                                 |
| `repo_name`              | text     | 注意：仓库可能存在更名行为                              |
| `issue_id`               | bigint   | 一个 PR 会与一个 Issue 相对应 |
| `pull_request_id`        | bigint   |                                                                 |
| `number`                 | bigint   | 针对 `review_comment` / `issue_comment`                         |
| `commit_id`              | bigint   |                                                                 |
| `comment_type`           | varchar  | 可选类型：`commit_comment` / `review_comment` / `issue_comment`  |
| `body`                   | text     |                                                                 |
| `commit_id`              | bigint   | 即 Commit SHA                                                   |
| `original_commit_id`     | bigint   | 即 Commit SHA                                                    |
| `position`               | int      |                                                                 |
| `original_position`      | int      |                                                                 |
| `path`                   | text     |                                                                 |
| `line`                   | text     |                                                                 |
| `diff_hunk`              | text     |                                                                 |
| `pull_request_review_id` | bigint   |                                                                 |
| `creator_id`             | bigint   |                                                                 |
| `creator_login`          | text     |                                                                 |
| `created_at`             | datetime |                                                                 |
| `updated_at`             | datetime |                                                                 |

### 注意

- commit_comment 类型的评论可能出现在 Pull Request 的 Commit 当中，也可能出现在仓库分支上的 Commit，针对仓库分支上 Commit 的评论是不会包含 
  `issue_id`、`pull_request_id`、`number` 几个字段，直接通过 Commit SHA 与 Comment ID 来唯一确定一条评论，例如：[chaos-mesh/chaos-mesh#41101958](https://github.com/chaos-mesh/chaos-mesh/commit/8eab4c153854955bdbc9a28736704c231ed11198#r41101958)

### 已知问题

- 由于 `gha_issues_pull_requests` 基础信息表的数据缺失，根据 `pull_request_id` 不一定能够找到对应的 `issue_id`，建议使用 `number` 字段替代。

