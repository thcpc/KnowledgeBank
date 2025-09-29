
# 问题描述
我期望在Docker 部署的时候，把生成的.env.production 和 alembic.ini 通过COPY 命令，COPY文件拷贝进Docker，但是失败
```Dockerfile
# 第一阶段：构建
FROM python:3.13-slim as builder

WORKDIR /app

RUN mkdir -p /root/.local && apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    libuv1-dev pkg-config libmariadb3 libmariadb-dev libmariadb-dev-compat
COPY et/. .
# 拷贝 alembic.ini
COPY alembic.ini .
# COPY et/requirements.txt .
RUN pip install --user -r requirements.txt
RUN pip install --user uvloop 

# 第二阶段：运行
FROM python:3.13-slim

WORKDIR /app
RUN apt-get update && apt-get install -y \
    libmariadb3 \         
    libuv1
                 
COPY --from=builder /root/.local /root/.local
# 拷贝 .env.production
COPY .env.production .env
COPY . .

ENV PATH=/root/.local/bin:$PATH
ENV PYTHONUNBUFFERED=1

EXPOSE 8000

# CMD ["tail", "-f", "/dev/null"] # 调试命令
CMD ["sh", "-c","alembic upgrade head && uvicorn et.asgi:application --host 0.0.0.0 --port 8000 --lifespan off"]
```
# .env.production 解决
通过Docker-Compose.yml  文件配置生产的env，不用拷贝
```yml
services:
  etbackend:
    build: ./backend
    env_file: backend/.env.production
```
# alembic.ini 问题解决
排除了路径和权限原因，可能是以下配置造成
1. Docker-Compose.yml 中配置了映射到代码路径
```yml
services:
  etbackend:
    build: ./backend
    env_file: backend/.env.production
    volumes:
      - ./backend/et:/app # 映射到backend/et -> app
      - et_backend_static_volume:/app/static
      - et_backend_media_volume:/app/media
      - et_backend_data_volume:/app/data
      - et_backend_log_volume:/var/log/django
```
2. 在DockerFile中写入了 Copy . .
```dockerfile
# 第二阶段：运行
FROM python:3.13-slim

WORKDIR /app
RUN apt-get update && apt-get install -y \
    libmariadb3 \         
    libuv1
                 
COPY --from=builder /root/.local /root/.local
# 拷贝 .env.production
COPY .env.production .env

COPY . .

ENV PATH=/root/.local/bin:$PATH
ENV PYTHONUNBUFFERED=1

EXPOSE 8000
```
这两个地方可能造成文件被覆盖，导致期望失败，所以更改如下
1. Docker-Compose.yml 删除映射。
2. Docker File 中删除Copy . .
最终配置文件如下
[[部署说明#2.2 docker-compose.yml | docker-compose.yml]] [[部署说明#2.3 Backend]]