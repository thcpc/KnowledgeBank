#interface

类型：接口
定义行为:
 Bean容器，获取Bean
扩展的行为
1. [[HierarchicalBeanFactory]]
2. [[ListableBeanFactory]]
3. [[AutowireCapableBeanFactory]]

```java
public interface BeanFactory {  
  
    public Object getBean(String beanName) throws BeansException;  
  
    public Object getBean(String beanName, Object ... args) throws BeansException;  
  
    <T> T getBean(String name, Class<T> requireType) throws BeansException;  
}

```