---
doc_type: weread-highlights-reviews
bookId: "22806910"
author: 卢西亚诺·拉马略
cover: https://wfqqreader-1252317822.image.myqcloud.com/cover/910/22806910/t7_22806910.jpg
reviewCount: 2
noteCount: 54
isbn: 9787115454157
category: 计算机-编程设计
lastReadDate: 2023-08-02
---
# 元数据
> [!abstract] 流畅的Python
> - ![ 流畅的Python|200](https://wfqqreader-1252317822.image.myqcloud.com/cover/910/22806910/t7_22806910.jpg)
> - 书名： 流畅的Python
> - 作者： 卢西亚诺·拉马略
> - 简介： 本书致力于帮助Python开发人员挖掘这门语言及相关程序库的优秀特性，避免重复劳动，同时写出简洁、流畅、易读、易维护，并且具有地道Python风格的代码。本书尤其深入探讨了Python语言的高级用法，涵盖数据结构、Python风格的对象、并行与并发，以及元编程等不同的方面。
> - 出版时间 2017-05-01 00:00:00
> - ISBN： 9787115454157
> - 分类： 计算机-编程设计
> - 出版社： 人民邮电出版社

# 高亮划线

## 第二部分 数据结构


- 📌 元组其实是对数据的记录：元组中的每个元素都存放了记录中一个字段的数据，外加这个字段的位置。正是这个位置信息给数据赋予了意义。 ^22806910-6-11055-11117
    - ⏱ 2023-07-13 09:20:57 

- 📌 示例2-7中的元组就被当作记录加以利用。如果在任何的表达式里我们在元组内对元素排序，这些元素所携带的信息就会丢失，因为这些信息是跟它们的位置有关的。示例2-7 把元组用作记录>>> lax_coordinates = (33.9425,-118.408056)  ➊>>> city, year, pop, chg, area = ('Tokyo', 2003, 32450, 0.66, 8014)  ➋ ^22806910-6-11261-11541
    - ⏱ 2023-07-13 09:22:44 

- 📌 for循环可以分别提取元组里的元素，也叫作拆包（unpacking） ^22806910-6-12240-12274
    - ⏱ 2023-07-13 09:23:11 

- 📌 最好辨认的元组拆包形式就是平行赋值 ^22806910-6-13172-13208
    - ⏱ 2023-07-13 09:25:36 

- 📌 lax_coordinates = (33.9425,-118.408056)>>> latitude, longitude = lax_coordinates #元组拆包>>> latitude ^22806910-6-13279-13383
    - ⏱ 2023-07-13 09:25:47 

- 📌 import os>>> _, filename = os.path.split('/home/luciano/.ssh/idrsa.pub')>>> filename'idrsa.pub'在进行拆包的时候，我们不总是对元组里所有的数据都感兴趣，_占位符能帮助处理这种情况，上面这段代码也展示了它的用法。 ^22806910-6-13980-14199
    - ⏱ 2023-07-13 09:27:02 

- 📌 用*args来获取不确定数量的参数算是一种经典写法了。于是Python 3里，这个概念被扩展到了平行赋值中：>>> a, b, *rest = range(5)>>> a, b, rest(0, 1, [2, 3, 4])>>> a, b, *rest = range(3)>>> a, b, rest(0, 1, [2])>>> a, b, *rest = range(2)>>> a, b, rest(0, 1, [])在平行赋值中，*前缀只能用在一个变量名前面，但是这个变量可以出现在赋值表达式的任意位置：>>> a, *body, c, d = range(5)>>> a, body, c, d(0, [1, 2], 3, 4)>>> *head, b, c, d = range(5)>>> head, b, c, d([0, 1], 2, 3, 4) ^22806910-6-14669-15225
    - ⏱ 2023-07-13 09:27:50 

- 📌 具名元组collections.namedtuple是一个工厂函数，它可以用来构建一个带字段名的元组和一个有名字的类 ^22806910-6-17151-17239
    - ⏱ 2023-07-14 19:02:38 

- 📌 City = namedtuple('City', 'name country population coordinates')  ➊>>> tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))  ➋>>> tokyoCity(name='Tokyo', country='JP', population=36.933, coordinates=(35.689722,139.691667))>>> tokyo.population  ➌36.933>>> tokyo.coordinates(35.689722, 139.691667)>>> tokyo[1] ^22806910-6-17841-18176
    - ⏱ 2023-07-14 19:03:35 

- 📌 [插图] ^22806910-6-20087-20088
    - ⏱ 2023-07-14 19:04:19 

- 📌 +和*都遵循这个规律，不修改原有的操作对象，而是构建一个全新的序列。[插图]如果在a * n这个语句中，序列a里的元素是对其他可变对象的引用的话，你就需要格外注意了，因为这个式子的结果可能会出乎意料。比如，你想用my_list=[[]] * 3来初始化一个由列表组成的列表，但是你得到的列表里包含的3个元素其实是3个引用，而且这3个引用指向的都是同一个列表。这可能不是你想要的效果。 ^22806910-6-26063-26419
    - ⏱ 2023-07-14 19:09:05 

- 📌 +=背后的特殊方法是__iadd__ （用于“就地加法”）。但是如果一个类没有实现这个方法的话，Python会退一步调用__add__ 。 ^22806910-6-29043-29112
    - ⏱ 2023-07-14 19:11:54 

- 📌 可变序列一般都实现了__iadd__方法，因此+=是就地加法。而不可变序列根本就不支持这个 ^22806910-6-29441-29486
    - ⏱ 2023-07-15 08:37:55 

- 📌 *=，不同的是，后者相对应的是__imul__。 ^22806910-6-29551-29575
    - ⏱ 2023-07-15 08:37:58 

- 📌 t = (1, 2, [30, 40])>>> t[2]+= [50, 60]Traceback (most recent call last):  File "<stdin>", line 1, in <module>TypeError: 'tuple' object does not support item assignment>>> t(1, 2, [30, 40, 50, 60]) ^22806910-6-31696-31907
    - ⏱ 2023-07-15 09:10:59 

- 📌 不要把可变对象放在元组里面。增量赋值不是一个原子操作。我们刚才也看到了，它虽然抛出了异常，但还是完成了操作。 ^22806910-6-33379-33463
    - ⏱ 2023-07-15 09:02:25 

- 📌 list.sort方法会就地排序列表，也就是说不会把原列表复制一份 ^22806910-6-33723-33756
    - ⏱ 2023-07-15 09:13:09 

- 📌 返回None其实是Python的一个惯例：如果一个函数或者方法对对象进行的是就地改动，那它就应该返回None， ^22806910-6-33798-33853
    - ⏱ 2023-07-15 09:13:21 

- 📌 list.sort相反的是内置函数sorted，它会新建一个列表作为返回值。 ^22806910-6-34296-34334
    - ⏱ 2023-07-15 09:13:32 

- 📌 用bisect来管理已排序的序列 ^22806910-6-36677-36693
    - ⏱ 2023-07-15 09:18:59 

- 📌 我们需要一个只包含数字的列表，那么array.array比list更高效 ^22806910-6-42279-42315
    - ⏱ 2023-07-15 09:20:49 

- 📌 示例2-20 一个浮点型数组的创建、存入文件和从文件读取的过程>>> from array import array  ➊>>> from random import random>>> floats = array('d', (random（　） for i in range(10**7)))  ➋>>> floats[-1]  ➌0.07802343889111107>>> fp = open('floats.bin', 'wb')>>> floats.tofile(fp)  ➍>>> fp.close（　）>>> floats2 = array('d')  ➎>>> fp = open('floats.bin', 'rb')>>> floats2.fromfile(fp, 10**7)  ➏>>> fp.close（　）>>> floats2[-1]  ➐0.07802343889111107>>> floats2 == floats  ➑True ^22806910-6-42757-43244
    - ⏱ 2023-07-15 09:24:23 

- 📌 array.tofile和array.fromfile用起来很简单。把这段代码跑一跑，你还会发现它的速度也很快。一个小试验告诉我，用array.fromfile从一个二进制文件里读出1000万个双精度浮点数只需要0.1秒，这比从文本文件里读取的速度要快60倍 ^22806910-6-43807-43936
    - ⏱ 2023-07-15 09:24:04 

- 📌 memoryview是一个内置类，它能让用户在不复制内容的情况下操作同一个数组的不同切片 ^22806910-6-45476-45520
    - ⏱ 2023-07-15 09:31:11 

- 📌 memoryview.cast的概念跟数组模块类似，能用不同的方式读写同一块内存数据，而且内容字节不会随意移动。这听上去又跟C语言中类型转换的概念差不多 ^22806910-6-45902-45978
    - ⏱ 2023-07-15 09:36:39 

- 📌 collections.deque类（双向队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型。而且如果想要有一种数据类型来存放“最近用到的几个元素”，deque也是一个很好的选择。这是因为在新建一个双向队列的时候，你可以指定这个队列的大小，如果这个队列满员了，还可以从反向端删除过期的元素，然后在尾端添加新的元素。示例2-23中有几个双向队列的典型操作。示例2-23 使用双向队列>>> from collections import deque>>> dq = deque(range(10), maxlen=10)  ➊>>> dqdeque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)>>> dq.rotate(3)  ➋>>> dqdeque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6], maxlen=10)>>> dq.rotate(-4)>>> dqdeque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], maxlen=10)>>> dq.appendleft(-1)  ➌>>> dqdeque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)>>> dq.extend([11, 22, 33])  ➍>>> dqdeque([3, 4, 5, 6, 7, 8, 9, 11, 22, 33], maxlen=10)>>> dq.extendleft([10, 20, 30, 40])  ➎>>> dqdeque([40, 30, 20, 10, 3, 4, 5, 6, 7, 8], maxlen=10)❶ maxlen是一个可选参数，代表这个队列可以容纳的元素的数量，而且一旦设定，这个属性就不能修改了。❷ 队列的旋转操作接受一个参数n，当n > 0时，队列的最右边的n个元素会被移动到队列的左边。当n < 0时，最左边的n个元素会被移动到右边。❸ 当试图对一个已满（len(d)==d.maxlen）的队列做头部添加操作的时候，它尾部的元素会被删除掉。注意在下一行里，元素0被删除了。❹ 在尾部添加3个元素的操作会挤掉-1、1和2。❺ extendleft(iter)方法会把迭代器里的元素逐个添加到双向队列的左边，因此迭代器里的元素会逆序出现在队列里。表2-3总结了列表和双向队列这两个类型的方法（object类包含的方法除外）。双向队列实现了大部分列表所拥有的方法，也有一些额外的符合自身设计的方法，比如说popleft和rotate。但是为了实现这些方法，双向队列也付出了一些代价，从队列中间删除元素的操作会慢一些，因为它只对在头尾的操作进行了优化。append和popleft都是原子操作，也就说是deque可以在多线程程序中安全地当作先进先出的队列使用，而使用者不需要担心资源锁的问题。 ^22806910-6-51338-53089
    - ⏱ 2023-07-16 18:30:37 

- 📌 sorted和list.sort背后的排序算法是Timsort，它是一种自适应算法，会根据原始数据的顺序特点交替使用插入排序和归并排序，以达到最佳效率。 ^22806910-6-61945-62021
    - ⏱ 2023-07-16 18:36:00 
## 第3章 字典和集合


- 📌 collections.abc模块中有Mapping和MutableMapping这两个抽象基类，它们的作用是为dict和其他类似的类型定义形式接口 ^22806910-7-1296-1370
    - ⏱ 2023-07-18 19:19:02 

- 📌 非抽象映射类型一般不会直接继承这些抽象基类，它们会直接对dict或是collections.UserDict进行扩展。这些抽象基类的主要作用是作为形式化的文档，它们定义了构建一个映射类型所需要的最基本的接口。然后它们还可以跟isinstance一起被用来判定某个数据是不是广义上的映射类型 ^22806910-7-1789-1933
    - ⏱ 2023-07-18 19:18:35 

- 📌 my_dict = {}>>> isinstance(my_dict, abc.Mapping)True ^22806910-7-2070-2128
    - ⏱ 2023-07-18 19:18:42 

- 📌 字典推导的应用>>> DIAL_CODES = [                ➊...         (86, 'China'),...         (91, 'India'),...         (1, 'United States'),...         (62, 'Indonesia'),...         (55, 'Brazil'),...         (92, 'Pakistan'),...         (880, 'Bangladesh'),...         (234, 'Nigeria'),...         (7, 'Russia'),...         (81, 'Japan'),...     ]>>> country_code = {country: code for code, country in DIAL_CODES} ^22806910-7-4707-5147
    - ⏱ 2023-07-21 19:27:24 

- 📌 所有的映射类型在处理找不到的键的时候，都会牵扯到__missing__方法 ^22806910-7-12980-13017
    - ⏱ 2023-07-23 08:55:18 

- 📌 _missing__方法只会被__getitem__调用（比如在表达式d[k]中）。提供__missing__方法对get或者__contains__（in运算符会用到这个方法）这些方法的使用没有影响。 ^22806910-7-13347-13448
    - ⏱ 2023-07-23 08:55:27 

- 📌 自定义一个映射类型，更合适的策略其实是继承collections.UserDict类 ^22806910-7-15137-15179
    - ⏱ 2023-07-23 08:55:02 

- 📌 collections.OrderedDict这个类型在添加键的时候会保持顺序，因此键的迭代次序总是一致的 ^22806910-7-17697-17779
    - ⏱ 2023-07-23 16:18:45 

- 📌 collections.ChainMap该类型可以容纳数个不同的映射对象，然后在进行键查找操作的时候，这些对象会被当作一个整体被逐个查找，直到键被找到为止。 ^22806910-7-17909-18016
    - ⏱ 2023-07-23 16:19:15 

- 📌 collections.Counter这个映射类型会给键准备一个整数计数器。 ^22806910-7-18343-18410
    - ⏱ 2023-07-23 16:19:32 

- 📌 >>> ct = collections.Counter('abracadabra')>>> ctCounter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})>>> ct.update('aaaaazzz')>>> ctCounter({'a': 10, 'z': 3, 'b': 2, 'r': 2, 'c': 1, 'd': 1})>>> ct.most_common(2)[('a', 10), ('z', 3)] ^22806910-7-18712-18957
    - ⏱ 2023-07-23 16:19:41 

- 📌 collections.UserDict这个类其实就是把标准dict用纯Python又实现了一遍。 ^22806910-7-19033-19111
    - ⏱ 2023-07-23 16:19:45 

- 📌 标准库里所有的映射类型都是可变的，但有时候你会有这样的需求，比如不能让用户错误地修改某个映射 ^22806910-7-22584-22630
    - ⏱ 2023-07-23 16:23:29 

- 📌 从Python 3.3开始，types模块中引入了一个封装类名叫MappingProxyType ^22806910-7-22809-22857
    - ⏱ 2023-07-23 16:23:42 

- 📌 如果给这个类一个映射，它会返回一个只读的映射视图。 ^22806910-7-22858-22883
    - ⏱ 2023-07-23 16:24:07 

- 📌 用MappingProxyType来获取字典的只读实例mappingproxy>>> from types import MappingProxyType>>> d = {1:'A'}>>> d_proxy = MappingProxyType(d)>>> d_proxymappingproxy({1: 'A'})>>> d_proxy[1]  ➊'A'>>> d_proxy[2] = 'x'  ➋Traceback (most recent call last):  File "<stdin>", line 1, in <module>TypeError: 'mappingproxy' object does not support item assignment ^22806910-7-23035-23407
    - ⏱ 2023-07-23 16:24:21 

- 📌 示例3-10 needles的元素在haystack里出现的次数，两个变量都是set类型found = len(needles & haystack) ^22806910-7-25397-25496
    - ⏱ 2023-07-23 16:25:23 

- 📌 3.8.2 集合推导Python 2.7带来了集合推导（setcomps）和之前在3.2节里讲到过的字典推导。示例3-13是个简单的例子。示例3-13 新建一个Latin-1字符集合，该集合里的每个字符的Unicode名字里都有“SIGN”这个单词>>> from unicodedata import name  ➊>>> {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}  ➋{'§', '=', '¢', '#', '¤', '<', '¥', 'μ', '×', '$', '¶', '£', '©','°', '+', '÷', '±', '>', '¬', '®', '%'} ^22806910-7-28583-29026
    - ⏱ 2023-07-23 16:27:58 

- 📌 如果两个对象在比较的时候是相等的，那它们的散列值必须相等，否则散列表就不能正常运行了。例如，如果1==1.0为真，那么hash(1)==hash(1.0)也必须为真 ^22806910-7-35564-35646
    - ⏱ 2023-07-27 09:14:23 

- 📌 如果你实现了一个类的__eq__方法，并且希望它是可散列的，那么它一定要有个恰当的__hash__方法，保证在a==b为真的情况下hash(a)==hash(b)也必定为真。否则就会破坏恒定的散列表算法，导致由这些对象所组成的字典和集合完全失去可靠性，这个后果是非常可怕的。另一方面，如果一个含有自定义的__eq__依赖的类处于可变的状态，那就不要在这个类中实现__hash__方法，因为它的实例是不可散列的。 ^22806910-7-39829-40034
    - ⏱ 2023-07-27 09:17:54 

- 📌 字典在内存上的开销巨大 ^22806910-7-40081-40092
    - ⏱ 2023-07-27 09:16:34 

- 📌 如果你需要存放数量巨大的记录，那么放在由元组或是具名元组构成的列表中会是比较好的选择；最好不要根据JSON的风格，用由字典组成的列表来存放这些记录。用元组取代字典就能节省空间的原因有两个：其一是避免了散列表所耗费的空间，其二是无需把记录中字段的名字在每个元素里都存一遍。 ^22806910-7-40164-40299
    - ⏱ 2023-07-27 09:17:06 

- 📌 在用户自定义的类型中，__slots__属性可以改变实例属性的存储方式，由dict变成tuple， ^22806910-7-40329-40378
    - ⏱ 2023-07-27 09:17:16 

- 📌 因为优化往往是可维护性的对立面。 ^22806910-7-40489-40505
    - ⏱ 2023-07-27 09:17:35 

- 📌 不要对字典同时进行迭代和修改。如果想扫描并修改一个字典，最好分成两步来进行：首先对字典迭代，以得出需要添加的内容，把这些内容放在一个新字典里；迭代结束之后再对原有字典进行更新。 ^22806910-7-42731-42819
    - ⏱ 2023-07-27 09:18:59 

- 📌 集合很消耗内存 ^22806910-7-43572-43579
    - ⏱ 2023-07-27 09:19:28 
## 第4章 文本和字节序列


- 📌 memoryview类不是用于创建或存储字节序列的，而是共享内存，让你访问其他二进制序列、打包的数组和缓冲中的数据切片，而无需复制字节序列 ^22806910-8-7565-7634
    - ⏱ 2023-08-02 09:27:12 
# 读书笔记

## 第二部分 数据结构

### 划线评论
- 📌 collections.namedtuple  ^98325-7JKEk3i7o
    - 💭 目前官方建议用dataclass代替，除非超大量数据且追求效率
    - ⏱ 2023-07-16 18:25:48

### 划线评论
- 📌 dis.dis('s[a]+= b')  ^98325-7JIxIkdBp
    - 💭 查看字节码
    - ⏱ 2023-07-15 09:11:34
   
# 本书评论
