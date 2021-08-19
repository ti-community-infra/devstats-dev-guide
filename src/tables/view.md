# 视图

GhArchive 上的数据是用户在 GitHub 上产生的公开活动的事件数据，然而在一些查询场景下，我们往往只需要针对数据对象的最新状态进行查询，而不需要遍历过往的历史数据。为了简化这些查询工作，可以通过数据库的视图功能，根据历史数据来构建数据对象最新状态的模型。

## Pull Requests

视图名：`pull_requests`

| 字段名                  | 数据类型       | 附加说明                                                          |
| ----------------------- | -------------- | ----------------------------------------------------------------- |
| `event_id`              | `bigint`       |                                                                   |
| `id`                    | `bigint`       |                                                                   |
| `repo_name`             | `varchar(160)` | 注意：仓库可能存在更名行为                                        |
| `number`                | `integer`      | 注意：Issue 与 Pull Request 的 Number 编号是统一的                |
| `title`                 | `text`         |                                                                   |
| `state`                 | `varchar(20)`  | Pull Request 的状态，可以分为：`open` / `closed`                  |
| `body`                  | `text`         |                                                                   |
| `assignee_id`           | `bigint`       |                                                                   |
| `milestone_id`          | `bigint`       |                                                                   |
| `milestone`             | `varchar(200)` |                                                                   |
| `base_sha`              | `varchar(40)`  |                                                                   |
| `creator_id`            | `bigint`       | 创建者的 Actor ID，可以通过连接 `actors` 视图获取创建者的更多信息 |
| `created_at`            | `bigint`       |                                                                   |
| `updated_at`            | `timestamp`    |                                                                   |
| `closed_at`             | `timestamp`    |                                                                   |
| `merged_at`             | `timestamp`    |                                                                   |
| `merge_commit_sha`      | `varchar(40)`  |                                                                   |
| `merger_id`             | `bigint`       | 合并者的 Actor ID                                                 |
| `maintainer_can_modify` | `boolean`      | 是否允许 Maintainers (拥有仓库 Write 权限的用户) 进行修改         |
| `comments`              | `integer`      | 评论的数量                                                        |
| `review_comments`       | `integer`      | Review 评论的数量                                                 |
| `additions`             | `integer`      | 添加代码总行数                                                    |
| `deletions`             | `integer`      | 删除代码总行数                                                    |
| `changed_files`         | `integer`      | 改变文件数量                                                      |

## Pull Request Assignees

Pull Request 当前分配的 Assignees。

视图名：`pull_request_assignees`

| 字段名            | 数据类型 | 附加说明                             |
| ----------------- | -------- | ------------------------------------ |
| `pull_request_id` | `bigint` |                                      |
| `assignee_id`     | `bigint` | 可与 `actors` 视图基础数据表进行连接 |

## Pull Request Requested Reviewers

Pull Request 当前请求过的 reviewers。

视图名：`pull_request_requested_reviewers`

| 字段名                  | 数据类型 | 附加说明                             |
| ----------------------- | -------- | ------------------------------------ |
| `pull_request_id`       | `bigint` |                                      |
| `requested_reviewer_id` | `bigint` | 可与 `actors` 视图基础数据表进行连接 |

## Issues

视图名：`issues`

| 字段名            | 数据类型       | 附加说明                                           |
| ----------------- | -------------- | -------------------------------------------------- |
| `event_id`        | `bigint`       |                                                    |
| `id`              | `bigint`       |                                                    |
| `repo_id`         | `bigint`       |                                                    |
| `repo_name`       | `varchar(160)` | 注意：仓库可能存在更名行为                         |
| `number`          | `integer`      | 注意：Issue 与 Pull Request 的 Number 编号是统一的 |
| `title`           | `text`         |                                                    |
| `state`           | `varchar(20)`  | Issue 的状态，可以分为：`open` / `closed`          |
| `is_pull_request` | `boolean`      |                                                    |
| `body`            | `text`         |                                                    |
| `assignee_id`     | `bigint`       |                                                    |
| `milestone_id`    | `bigint`       |                                                    |
| `milestone`       | `varchar(200)` |                                                    |
| `creator_id`      | `bigint`       |                                                    |
| `created_at`      | `timestamp`    |                                                    |
| `updated_at`      | `timestamp`    | 最后的更新时间                                     |
| `closed_at`       | `timestamp`    |                                                    |
| `comments`        | `integer`      | 评论的数量                                         |

## Issue Labels

视图名：`issue_labels`

| 字段名       | 数据类型       | 附加说明                                   |
| ------------ | -------------- | ------------------------------------------ |
| `issue_id`   | `bigint`       |                                            |
| `label_id`   | `bigint`       |                                            |
| `full_label` | `varchar(160)` | 完整的标签名称，格式如：`<prefix>/<label>` |
| `prefix`     | `varchar(160)` | 标签的前缀，默认为 `general`，表示没有前缀 |
| `label`      | `varchar(160)` | 标签去掉前缀后剩下的部分                   |

## Milestones

视图名：`milestones`

| 字段名       | 数据类型       | 附加说明                   |
| ------------ | -------------- | -------------------------- |
| `event_id`   | `bigint`       |                            |
| `id`         | `bigint`       |                            |
| `repo_id`    | `bigint`       |                            |
| `repo_name`  | `varchar(160)` | 注意：仓库可能存在更名行为 |
| `milestone`  | `varchar(200)` |                            |
| `state`      | `varchar(20)`  |                            |
| `created_at` | `timestamp`    |                            |
| `updated_at` | `timestamp`    |                            |
| `closed_at`  | `timestamp`    |                            |

## Comments

视图名：`comments`

| 字段名                   | 数据类型       | 附加说明                                                        |
| ------------------------ | -------------- | --------------------------------------------------------------- |
| `event_id`               | `bigint`       |                                                                 |
| `id`                     | `bigint`       |                                                                 |
| `repo_id`                | `bigint`       |                                                                 |
| `repo_name`              | `varchar(160)` | 注意：仓库可能存在更名行为                                      |
| `number`                 | `integer`      | 该值在 `commit_comment` 类型的评论中为空                        |
| `commit_id`              | `bigint`       |                                                                 |
| `comment_type`           | `varchar(20)`  | 可选类型：`commit_comment` / `review_comment` / `issue_comment` |
| `body`                   | `text`         |                                                                 |
| `commit_id`              | `bigint`       | 即 Commit SHA                                                   |
| `original_commit_id`     | `bigint`       | 即 Commit SHA                                                   |
| `position`               | `integer`      |                                                                 |
| `original_position`      | `integer`      |                                                                 |
| `path`                   | `text`         |                                                                 |
| `line`                   | `integer`      |                                                                 |
| `diff_hunk`              | `text`         |                                                                 |
| `pull_request_review_id` | `bigint`       |                                                                 |
| `creator_id`             | `bigint`       |                                                                 |
| `creator_login`          | `varchar(120)` |                                                                 |
| `created_at`             | `timestamp`    |                                                                 |
| `updated_at`             | `timestamp`    |                                                                 |
| `issue_id`               | `bigint`       |                                                                 |
| `pull_request_id`        | `bigint`       |                                                                 |

### 注意

- `commit_comment` 类型的评论可能出现在 Pull Request 的 Commit 当中，也可能出现在仓库分支上的 Commit，针对仓库分支上 Commit 的评论是不会包含
  `issue_id`、`pull_request_id`、`number` 几个字段，直接通过 Commit SHA 与 Comment ID 来唯一确定一条评论，例如：[chaos-mesh/chaos-mesh#41101958](https://github.com/chaos-mesh/chaos-mesh/commit/8eab4c153854955bdbc9a28736704c231ed11198#r41101958)

### 已知问题

- 由于 `gha_issues_pull_requests` 基础信息表的数据缺失，根据 `pull_request_id` 不一定能够找到对应的 `issue_id`，建议使用 `number` 字段替代。

## Commits

视图名：`commits`

注意：这里的 commit 不包含 Pull Request 当中的 commit，而包含

| 字段名            | 数据类型       | 附加说明 |
| ----------------- | -------------- | -------- |
| `sha`             | `bigint`       |          |
| `event_id`        | `bigint`       |          |
| `message`         | `text`         |          |
| `is_distinct`     | `boolean`      |          |
| `author_id`       | `bigint`       |          |
| `author_name`     | `varchar(160)` |          |
| `author_email`    | `varchar(160)` |          |
| `committer_id`    | `bigint`       |          |
| `committer_name`  | `varchar(160)` |          |
| `committer_email` | `varchar(160)` |          |
| `loc_added`       | `integer`      |          |
| `loc_removed`     | `integer`      |          |
| `files_changed`   | `integer`      |          |
| `repo_id`         | `bigint`       |          |
| `repo_name`       | `varchar(160)` |          |

## Actors

视图名：`actors`

| 字段名                | 数据类型       | 附加说明                                                              |
| --------------------- | -------------- | --------------------------------------------------------------------- |
| `id`                  | `bigint`       |                                                                       |
| `login`               | `varchar(120)` |                                                                       |
| `name`                | `varchar(120)` |                                                                       |
| `sex`                 | `varchar(1)`   |                                                                       |
| `sex_prob`            | `double`       |                                                                       |
| `country_id`          | `varchar(2)`   | 参照 `gha_countries` 基础数据表                                       |
| `country_name`        | `text`         |                                                                       |
| `tz`                  | `varchar(40)`  |                                                                       |
| `tz_offset`           | `integer`      |                                                                       |
| `age`                 | `integer`      |                                                                       |
| `company_name`        | `varchar(160)` |                                                                       |
| `merged_prs`          | `integer`      |                                                                       |
| `is_code_contributor` | `boolean`      | 根据 PR 数量是否大于 1 来判断是否为代码贡献者                         |
| `is_bot`              | `boolean`      | 根据 `gha_bot_logins` 表的 pattern 判断该 login 的 actor 是否为机器人 |
