#interface
类型: 接口
定义行为:
扩展了[[BeanFactory]]的基本行为，扩充了BeanFactory可以获取多个Bean的行为

```java
public interface ListableBeanFactory extends BeanFactory{  
    <T> Map<String, T> getBeansOfType(Class<T> type) throws BeansException;  
    String[] getBeanDefinitionNames();  
}
```

扩展行为
[[ConfigurableListableBeanFactory]]