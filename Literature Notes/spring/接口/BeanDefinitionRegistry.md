#interface
类型：接口
定义行为:
向[[BeanFactory]]中添加[[BeanDefinition]]

```java
public interface BeanDefinitionRegistry {  
    void registerBeanDefinition(String beanName, BeanDefinition beanDefinition);  
  
    boolean containsBeanDefinition(String beanName);  
}
```