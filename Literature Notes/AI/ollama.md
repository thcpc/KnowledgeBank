## Docker 操作
```shell
# 安装命令
docker run -d -v ollama:/root/.ollama -p 444:11434 --name ollama ollama/ollama
# 部署模型
# 1. 进入Dockers 内部
docker exec -it ollama bash
# 2. 拉去模型
ollama pull deepseek-r1
# 3. 运行模型
ollama run deepseek-r1
```