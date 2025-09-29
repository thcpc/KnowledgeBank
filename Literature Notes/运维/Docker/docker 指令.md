## docker compose 常用命令
```shell
# 不使用缓存构建
docker compose build --no-cache
# 指定构建
docker compose build --no-cache etnginx

# 强制删除（运行中的容器也会被停止删除）**
docker compose down -v --remove-orphans

# 启动服务，后台运行
docker compose up -d

# 仅停止容器
docker compose stop
# 删除已停止的容器
docker compose rm

# 彻底删除所有资源
# rmi all 删除所有相关镜像（包括 `local` 和 `cache` 镜像）
# `-v` 或 `--volumes` 删除所有数据卷（⚠️ 慎用！会永久删除数据库等持久化数据）
docker-compose down --rmi all -v

```

## docker 常用命令
```shell
### 删除所有悬空得镜像
docker image prune
docker rmi $(docker images -f "dangling=true" -q)
```
