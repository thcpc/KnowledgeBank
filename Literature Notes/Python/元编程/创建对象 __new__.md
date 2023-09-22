#pymagic

1. 特殊的类方法，所以不用使用@classmehtod装饰器
2. 必须返回一个实例
3. 处理过程, new 常见实例，然后 init函数填充参数

```mermaid
flowchart LR
__new__-->__init__
```
所有 new 函数才是真正的构造函数，init只是填充初始化参数

```python
#构建对象的伪代码 
def object_maker(the_class, some_arg): 
	new_object = the_class.__new__(some_arg) 
	if isinstance(new_object, the_class): 
		the_class.__init__(new_object, some_arg) 
	return new_object 
#下述两个语句的作用基本等效 
x = Foo('bar') 
x = object_maker(Foo, 'bar')
```