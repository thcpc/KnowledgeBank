
# on_start
on_start 是 Locust 用例实例启动的时候执行的方法，
启动时，启动多少个实例该方法就会执行多少此，
所以通过该方法构造测试数据时，最好使用单态模式

```python
class Tester(HttpUser):  
    def on_start(self):  
        .....
```
