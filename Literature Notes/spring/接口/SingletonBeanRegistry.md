类型：接口
定义行为：
单态方式获取Bean
实现类：
[[DefaultSingletonBeanRegistry]]

```java
public interface SingletonBeanRegistry {  
    Object getSingleton(String beanName);  
}
```