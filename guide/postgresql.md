# PostgreSQL 操作指南

说明：本指南覆盖常见的安装、启动、管理、备份恢复与故障排查命令，面向 macOS / Linux / Docker 环境。

## 1. 安装

- macOS (Homebrew):

```bash
brew install postgresql
brew services start postgresql
```

- Debian/Ubuntu:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl enable --now postgresql
```

- CentOS/RHEL:

```bash
sudo yum install -y postgresql-server postgresql-contrib
sudo postgresql-setup initdb
sudo systemctl enable --now postgresql
```

- Docker (快速运行):

```bash
docker run -d --name pg -e POSTGRES_PASSWORD=secret -p 5432:5432 postgres:latest
```

## 2. 初始化与集群管理

- 手动初始化（当需要自定义数据目录时）:

```bash
initdb -D /var/lib/postgresql/data
```

- 使用 `pg_ctl` 启动/停止（适用于手动集群）:

```bash
pg_ctl -D /var/lib/postgresql/data start
pg_ctl -D /var/lib/postgresql/data stop
```

- systemd 管理（大多数 Linux 发行版）:

```bash
sudo systemctl start postgresql
sudo systemctl stop postgresql
sudo systemctl status postgresql
```

## 3. 连接与 psql 基本操作

- 切换为 postgres 用户并进入 psql：

```bash
sudo -u postgres psql
# 或
psql -h localhost -U youruser -d yourdb
```

- 常用 psql 命令：

```text
\l        -- 列出数据库
\du       -- 列出角色
\dt       -- 列出表
\c dbname -- 切换数据库
\q        -- 退出 psql
```

- 在 psql 里执行 SQL：

```sql
CREATE DATABASE mydb;
CREATE ROLE myuser WITH LOGIN PASSWORD 'pass';
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

## 4. 备份与恢复

- 逻辑备份（单库）：

```bash
pg_dump -U user -h host -F c -b -v -f backup_db.dump dbname
# 恢复
pg_restore -U user -h host -d dbname -v backup_db.dump
```

- 全集群物理备份（使用 base backup）：

```bash
pg_basebackup -D /path/to/backup -F tar -z -P -U replication_user -h primary_host
```

- 简单 SQL 导出/导入：

```bash
pg_dumpall -U postgres > all.sql
psql -U postgres -f all.sql
```

## 5. 远程连接与安全配置

- 编辑 `postgresql.conf` 设置监听地址：

```conf
listen_addresses = '*'
```

- 编辑 `pg_hba.conf` 增加访问控制，例如允许同网段密码认证：

```conf
# TYPE  DATABASE        USER            ADDRESS                 METHOD
host    all             all             192.168.1.0/24          md5
```

- 修改后重载配置：

```bash
sudo systemctl reload postgresql
# 或使用
pg_ctl reload -D /var/lib/postgresql/data
```

## 6. 用户与权限管理

- 创建用户/角色：

```sql
CREATE ROLE app_user WITH LOGIN PASSWORD 's3cret';
ALTER ROLE app_user CREATEDB; -- 赋予创建数据库权限
```

- 授权表/模式访问：

```sql
GRANT SELECT, INSERT ON TABLE public.mytable TO app_user;
GRANT USAGE ON SCHEMA analytics TO app_user;
```

## 7. 常用维护命令

- 清理与统计：

```sql
VACUUM;       -- 回收空间
VACUUM FULL;  -- 更彻底但会锁表
ANALYZE;      -- 更新查询统计信息
REINDEX;      -- 重建索引
```

- 检查活动连接与锁：

```sql
SELECT * FROM pg_stat_activity;
SELECT * FROM pg_locks;
```

## 8. 日志与监控

- 日志文件位置通常由 `postgresql.conf` 的 `log_directory` 和 `log_filename` 控制。
- 可以启用慢查询日志：

```conf
log_min_duration_statement = 2000  # 记录 2s 以上的查询
```

## 9. 扩展与插件

- 安装并启用扩展（例如 `uuid-ossp`、`pg_trgm`）：

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS pg_trgm;
```

## 10. 高可用与连接池（简述）

- 推荐用 `pgbouncer` 做连接池，推荐用 `replication` + `patroni` / `repmgr` 做主从自动切换。

## 11. 常见故障排查

- 无法连接：检查防火墙、`listen_addresses`、`pg_hba.conf`、以及服务是否启动。
- 权限错误：查看角色与数据库的授权；用 `\du`、`\l` 确认。
- 性能下降：检查慢查询日志、`pg_stat_activity`、并运行 `EXPLAIN ANALYZE`。

## 12. 参考链接

- 官方文档：<https://www.postgresql.org/docs/>
- pgAdmin、pgBouncer 等工具文档

---

作者：自动生成的操作指南（可按需删改）

## 与 Markdownlint 的兼容性

为了在团队中保持一致的 Markdown 风格，建议仓库采用 `markdownlint` 的配置并在 VSCode 中推荐扩展。下面为推荐的最小配置示例（可根据团队偏好调整）：

`.markdownlint.json` 示例：

```json
{
 "default": true,
 "MD013": { "line_length": 120 },
 "MD029": false,
 "MD041": false
}
```

`.vscode/extensions.json` 示例（用于推荐扩展）：

```json
{
 "recommendations": [
  "DavidAnson.vscode-markdownlint"
 ]
}
```

`.vscode/settings.json` 示例（在保存时运行 lint）：

```json
{
 "markdownlint.run": "onSave",
 "markdownlint.config": {
  "MD013": { "line_length": 120 },
  "MD029": false
 }
}
```

在 VSCode 中可以运行命令面板的 `Markdownlint: Fix all` 来自动修复支持的项目。
