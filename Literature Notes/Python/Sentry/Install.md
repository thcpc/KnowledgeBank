## 1. 拉去最新的分支
```shell
git clone git@github.com:getsentry/self-hosted.git
```
## 2. 执行 install.sh

## 3.安装好后访问 9000端口


## 问题
### 1. 登陆时显示CSRF
![[Pasted image 20250710144149.png]]
sentry/sentry.conf.py 添加
```py
# host 主机域名或IP
CSRF_TRUESTED_ORIGINS = ["http://host:9000", "http://127.0.0.1:9000"]
```

```shell
# 重启
docker compose restart web
docker compose restart nginx
```
## 2. 修改邮箱
sentry/config.yml

```shell
#重启
docker compose restart worker
```



# 参考文章
[Docker搭建Sentry](https://blog.csdn.net/qq_35323561/article/details/126783063?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-126783063-blog-146375220.235^v43^pc_blog_bottom_relevance_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=2)