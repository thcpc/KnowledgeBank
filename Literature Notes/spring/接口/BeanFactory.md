类型：接口
定义行为:
 Bean容器，获取Bean
扩展的行为
1. [[ConfigurableListableBeanFactory]]
2. [[HierarchicalBeanFactory]]
3. [[ListableBeanFactory]]
4. [[AutowireCapableBeanFactory]]
5. [[ConfigurableBeanFactory]]
```java
public interface BeanFactory {  
  
    public Object getBean(String beanName) throws BeansException;  
  
    public Object getBean(String beanName, Object ... args) throws BeansException;  
  
    <T> T getBean(String name, Class<T> requireType) throws BeansException;  
}

```