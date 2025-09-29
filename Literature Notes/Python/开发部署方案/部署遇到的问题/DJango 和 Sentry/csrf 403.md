## 1. 网络访问问题和csrf 403 的问题

应为 Django 和 Sentry是在2个不同Docker 内，所以无法通过服务器域名访问，
### 修改方案1 
把Django 和 Sentry 放在一个网络中, 修改Docker Compose.yml
```yml
networks:
  eclinicalet_default: # name 通过 docker network ls 查询
    external: true  # 声明为已存在的网络
  sentry-self-hosted_default:
    external: true
services:
  etbackend:
	networks:
      - eclinicalet_default
      - sentry-self-hosted_default
```
并且修改dsn="http://9e7448173924abce3d39403b15a469a8@web:9000/2",

但这种方法有CSRF 403 问题，因为修改了DNS

### 修改方案2
直接在Docker Compose.yml中添加宿主机的映射
```yml
services:
  etbackend:
    extra_hosts:
      # 在 Docker 内增加 host 的映射，不然很多服务不可用如Sentry，数据库等
      # 在 Docker 内宿主机的IP为172.17.0.1
      - "automation-01.chengdudev.edetekapps.cn:172.17.0.1" 
```