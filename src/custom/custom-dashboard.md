# 自定义 Grafana 仪表盘

## 步骤

1. 在已经运行的 Grafana 上添加新的 Dashboard 或对已有的 Dashboard 进行修改，并点击保存按钮进行保存；
2. 通过 `kubectl cp` 命令将 Grafana 实例所使用的 sqlite 文件 dump 下来；

```bash
kubectl cp devstats-grafana-pingcap-xxx:/root/grafana.pingcap.db ~/dump/grafana.pingcap.db
kubectl cp devstats-grafana-chaosmesh-xxx:/root/grafana.chaosmesh.db ~/dump/grafana.chaosmesh.db
kubectl cp devstats-grafana-tikv-xxx:/root/grafana.tikv.db ~/dump/grafana.tikv.db
```

3. 切换到开发环境配置目录

```
cd configs/dev/
```

4. 将 dump 下来的 sqlite 文件复制到 `configs/dev/sqlite` 目录内；
5. 执行 `dump_dashboard_json.sh` 脚本, 将 sqlite 目录下的 \*.db 文件当中的 Dashboard 数据记录转换为 json 文件，存在在 sqlite 目录下。如果希望只对单个项目进行处理，可以通过在命令行前添加变量 `ONLY=<project_lowername>` 来指定；

```bash
ONLY=tikv dump_dashboard_json.sh
```

5. 如果希望将新增或修改过后的**单个** Dashboard 配置共享到其它项目当中，可以使用 `add_dashboard.sh` 脚本进行复制；

```bash
FROM_PROJ=tikv add_dashboard.sh dashboard_name.json
```

6. 如果希望将新增或修改过后的**所有** Dashboard 配置共享到其它项目当中，可以使用 `add_dashboards.sh` 脚本进行复制；

```bash
FROM_PROJ=tikv add_dashboards.sh
```

7. 核对复制出来的 Dashboard 是否存在错误，因为复制脚本在复制之后后将配置当中的项目名修改为新的项目名，这个过程中可能会修改了不应该修改的项目名，例如：dashboards.json 配置当中包含了所有项目名的链接列表；

8. 提交代码，并在开发环境服务器上进行测试。
