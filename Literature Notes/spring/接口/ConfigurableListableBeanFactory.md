

```java
public interface ConfigurableListableBeanFactory extends ListableBeanFactory, AutowireCapableBeanFactory, ConfigurableBeanFactory {  
    BeanDefinition getBeanDefinition(String beanName) throws BeansException;  
    void preInstantiateSingletons() throws BeansException;  
//    void addBeanPostProcessor(BeanPostProcessor beanPostProcessor);  
}
```