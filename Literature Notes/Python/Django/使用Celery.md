#### 安装 Celery

```bash
pip install celery
```



#### 2. 配置 Celery

在 Django 项目的根目录下创建 `celery.py`：
路径位置：
![[Pasted image 20250310154040.png]]

```python
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')
app = Celery('myproject')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()
```
在 \_\_init\_\_.py 中配置
```python
# et/__init__.py
from .et_celery import app as celery_app

__all__ = ('celery_app',)
```




#### 3. 在 `settings.py` 中配置 Celery

```python
# et/settings.py
CELERY_BROKER_URL = 'redis://localhost:6379/0'  # 使用 Redis 作为消息代理
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'  # 使用 Redis 作为结果后端
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TIMEZONE = 'UTC'
```

#### 4. 定义任务

在 `myapp/tasks.py` 中定义任务：

```python
from celery import shared_task

@shared_task
def my_background_task(arg1, arg2):
    # 执行后台任务
    pass
```



#### 5. 在视图中调用任务

```python
from myapp.tasks import my_background_task

def my_view(request):
    my_background_task.delay(arg1, arg2)  # 异步调用任务
    return HttpResponse("Task submitted!")
```

#### 6. 启动
```shell
# unix
celery -A et worker --loglevel=info
# windows, 因为WINDOW底层支持默认进程库有问题，所以制定其它库作为并发库
# 不建议用 eventlet 它运行时需要运行monkey_path(),可能会引发兼容性问题
celery.exe -A et worker --loglevel=INFO -P gevent
```
在 Windows 系统中使用 Celery 时，`-P` 选项用于指定要使用的消息传递后端（也称为池实现）。不同的后端有不同的性能和资源使用特性，适用于不同的应用场景。以下是一些可以在 `-P` 选项后使用的替代值：

1. **prefork**：这是默认的池实现，它使用多进程来并行执行任务。它适用于 CPU 密集型任务，因为它可以利用多核 CPU。
    
    ```shell
    celery.exe -A et worker --loglevel=INFO -P prefork
    ```
    
2. **solo**：solo 模式不使用任何池机制，而是单线程地运行 worker。它适用于调试和开发环境，因为它简化了任务执行的流程。
    
    ```shell
    celery.exe -A et worker --loglevel=INFO -P solo
    ```
    
3. **threads**：使用线程池来执行任务。它适用于 I/O 密集型任务，因为线程间的切换开销较小。然而，由于 Python 的全局解释器锁（GIL），多线程在 CPU 密集型任务上的性能可能不如多进程。
    
    ```shell
    celery.exe -A et worker --loglevel=INFO -P threads
    ```
    
4. **eventlet**：eventlet 是一个异步框架，用于编写网络应用程序。它使用协程来实现并发，这在 I/O 密集型任务上非常有效。eventlet 也支持异步 I/O 操作，使其在处理大量并发连接时性能优越。
    
    ```shell
    celery.exe -A et worker --loglevel=INFO -P eventlet
    ```
    
5. **gevent**：gevent 是另一个异步框架，类似于 eventlet。它也使用协程来实现并发，并支持异步 I/O 操作。gevent 在处理网络 I/O 时特别有效。
    
    ```shell
    celery.exe -A et worker --loglevel=INFO -P gevent
    ```
    

请注意，某些后端（如 `eventlet` 和 `gevent`）可能需要额外的安装步骤，并且可能不兼容所有的 Celery 版本或所有的任务类型。在选择池实现时，应考虑任务类型、系统资源以及应用程序的特定需求。

另外，Windows 系统对多线程和多进程的支持与类 Unix 系统（如 Linux）有所不同，因此在选择池实现时，可能需要进行一些性能测试来确定最适合你应用场景的池实现。