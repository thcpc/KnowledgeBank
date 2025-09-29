## Django can only handle ASGI/HTTP connections, not lifespan问题
修改启动命令
应为 Sentry 和 lifespan有些不兼容
```shell
# 关闭lifespan
uvicorn et.asgi:application --host 0.0.0.0 --port 8000 --lifespan off
```