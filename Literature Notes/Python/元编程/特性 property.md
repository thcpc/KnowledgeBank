
# 标准用法

```python
class LineItem: 
	def __init__(self, description, weight, price): 
		self.description = description 
		self.weight = weight  
		self.price = price 
	def subtotal(self): 
		return self.weight * self.price 
	@property
	def weight(self):  
		return self.__weight 
	@weight.setter 
	def weight(self, value): 
		if value > 0: self.__weight = value 
		else: raise ValueError('value must be > 0') 
```

# 动态的构造方法
property 实际上是一个类
```python
# property构造方法，
property(fget=None, fset=None, fdel=None, doc=None)

class LineItem: 
	def __init__(self, description, weight, price): 
		self.description = description 
		self.weight = weight 
		self.price = price 
	def subtotal(self): 
		return self.weight * self.price 
	def get_weight(self): 
		return self.__weight 
	def set_weight(self, value): 
		if value > 0: self.__weight = value 
		else: raise ValueError('value must be > 0') 
	weight = property(get_weight, set_weight) 



```

# 属性在class 和 实例中的读取顺序

property 是 属性(attr) 中的一种。
obj.attr这样的表达式不会从obj开始寻找attr，而是从obj.\__class\__ 开始，而且，仅当类中没有名为attr的特性时，Python才会在obj实例中寻找。

```python
class Class: 
    data = 'the class data attr'
    @property
    def prop(self):
        return 'the prop value'
```
## 实例属性遮盖类的数据属性
```python
>>> obj = Class（　）
>>> vars(obj) 
{}
>>> obj.data
'the class data attr'
>>> obj.data = 'bar'
>>> vars(obj)
{'data': 'bar'}
>>> obj.data
'bar'
```

## 实例属性不会遮盖类特性
```python
>>> Class.prop  # 
<property object at 0x1072b7408>
>>> obj.prop  # 
'the prop value'
>>> obj.prop = 'foo'  # 
Traceback (most recent call last):
  ...
AttributeError: can't set attribute
>>> obj.__dict__['prop'] = 'foo'  # 修改实例属性 
>>> vars(obj)  # 
{ 'data': 'bar','prop': 'foo'}
>>> obj.prop  # 没有修改
'the prop value'
>>> Class.prop = 'baz'  # prop 不再是 property 
>>> obj.prop  # 则修改成功
'foo'
```

## 新添的类特性遮盖现有的实例属性
```python
>>> obj.data  # 
'bar'
>>> Class.data  # 
'the class data attr'
>>> Class.data = property(lambda self: 'the "data" prop value')  # 
>>> obj.data  # 
'the "data" prop value'
>>> del Class.data  # 
>>> obj.data  # 
'bar'
```
## 应用场景
###  特性工厂函数 
#### 函数定义
```python
def quantity(storage_name):  
    def qty_getter(instance):  
        return instance.__dict__[storage_name]  
    def qty_setter(instance, value):  
        if value > 0:
            instance.__dict__[storage_name] = value  
        else:
            raise ValueError('value must be > 0')
    return property(qty_getter, qty_setter) 
```
#### 应用
`属性需要手动输入，可能导致错误 
解决办法[[描述符#进阶实现 | 描述符]] [[元类#定制描述符的类装饰器 | 元类]] 的方法解决

```python
weight = quantity('weight') 
```


```python
class LineItem:
    weight = quantity('weight')  
    price = quantity('price')  
    def __init__(self, description, weight, price):
        self.description = description
        self.weight = weight  
        self.price = price
    def subtotal(self):
        return self.weight * self.price  
```

## 特性删除

```python
class BlackKnight:
    def __init__(self):
        self.members = ['an arm', 'another arm',
                        'a leg', 'another leg']
        self.phrases = ["'Tis but a scratch.",
                        "It's just a flesh wound.",
                        "I'm invincible!",
                        "All right, we'll call it a draw."]
    @property
    def member(self):
        print('next member is:')
        return self.members[0]
    @member.deleter
    def member(self):
        text = 'BLACK KNIGHT (loses {})\n--{}'
        print(text.format(self.members.pop(0), self.phrases.pop(0)))
```

```python
>>> knight = BlackKnight（　）
>>> knight.member
next member is:
'an arm'
>>> del knight.member
BLACK KNIGHT (loses an arm)
--'Tis but a scratch.
>>> del knight.member
BLACK KNIGHT (loses another arm)
--It's just a flesh wound.
>>> del knight.member
BLACK KNIGHT (loses a leg)
--I'm invincible!
>>> del knight.member
BLACK KNIGHT (loses another leg)
--All right, we'll call it a draw.
```

# 影响属性处理的特殊属性
### \_\_class\_\_
对象所属类的引用（即obj.\_\_class\_\_与type(obj)的作用相同）。Python的某些特殊方法，例如\_\_getattr\_\_，只在对象的类中寻找，而不在实例中寻找。
### \_\_dict\_\_
一个映射，存储对象或类的可写属性。有\_\_dict\_\_属性的对象，任何时候都能随意设置新属性。如果类有[[特性 property#_ _slots _ _ | __slots__]]属性，它的实例可能没有\_\_dict\_\_属性。参见下面对\_\_slots\_\_属性的说明。
### \_\_slots\_\_
类可以定义这个这属性，限制实例能有哪些属性。\_\_slots\_\_属性的值是一个字符串组成的元组，指明允许有的属性。如果\_\_slots\_\_中没有'\_\_dict\_\_'，那么该类的实例没有\_\_dict\_\_属性，实例只允许有指定名称的属性。

# 处理属性的内置函数
### dir(\[object\])

列出对象的大多数属性。官方文档说，dir函数的目的是交互式使用，因此没有提供完整的属性列表，只列出一组“重要的”属性名。dir函数能审查有或没有__dict__属性的对象。dir函数不会列出__dict__属性本身，但会列出其中的键。dir函数也不会列出类的几个特殊属性，例如__mro__、\_\_bases\_\_和\_\_name\_\_。如果没有指定可选的object参数，dir函数会列出当前作用域中的名称

### getattr(object, name\[, default\])
从object对象中获取name字符串对应的属性。获取的属性可能来自对象所属的类或超类。如果没有指定的属性，getattr函数抛出AttributeError异常，或者返回default参数的值（如果设定了这个参数的话）。
### hasattr(object, name)
如果object对象中存在指定的属性，或者能以某种方式（例如继承）通过object对象获取指定的属性，返回True。文档说道：“这个函数的实现方法是调用getattr(object, name)函数，看看是否抛出AttributeError异常。”
### setattr(object, name, value)
把object对象指定属性的值设为value，前提是object对象能接受那个值。这个函数可能会创建一个新属性，或者覆盖现有的属性。

# 特殊处理函数
### \_\_delattr\_\_(self, name)
#pymagic 
只要使用del语句删除属性，就会调用这个方法。例如，del obj.attr语句触发Class.\_\_delattr\_\_(obj, 'attr')方法。
### \_\_dir\_\_(self)
#pymagic 
把对象传给dir函数时调用，列出属性。例如，dir(obj)触发Class.\_\_dir\_\_(obj)方法。

### \_\_getattr\_\_(self, name)
#pymagic 
仅当获取指定的属性失败，搜索过obj、Class和超类之后调用。表达式obj.no_such_attr、getattr(obj, 'no_such_attr')和hasattr(obj, 'no_such_attr')可能会触发Class.\_\_getattr\_\_(obj, 'no_such_attr')方法，但是，仅当在obj、Class和超类中找不到指定的属性时才会触发。

### 总结

其实，特殊方法__getattribute__和__setattr__不管怎样都会调用，几乎会影响每一次属性存取，因此比__getattr__方法（只处理不存在的属性名）更难正确使用。与定义这些特殊方法相比，使用特性或描述符相对不易出错。


--- 引用<<流畅的Python 19章>>
