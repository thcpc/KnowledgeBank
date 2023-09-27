
# getConstructors 和 getDeclaredConstructors 区别
|   |    |
| -------- | ---- |
| getConstructors| 只能获取 Public的的构造函数 |
| getDeclaredConstructors| 能获取所有权限的构造函数|



# getDeclaredConstructor(Class\<?\>... parameterTypes)
根据参数类型获取构造函数
```java
Class[] parameterTypes = new Class[args.length];  
for(int i = 0; i< args.length; i++){  
    parameterTypes[i] = args[i].getClass();  
 }  
return clazz.getDeclaredConstructor(parameterTypes).newInstance(args);
```



