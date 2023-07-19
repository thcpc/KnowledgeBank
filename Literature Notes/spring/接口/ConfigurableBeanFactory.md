#interface
类型：接口
定义行为：
扩展了Bean Factory 增加 Bean Factory可配置话的扩展属性，比如 增加BeanPostProcessor,
使 BeanFactory 可扩展。 

```java
public interface ConfigurableBeanFactory extends SingletonBeanRegistry, HierarchicalBeanFactory {  
    String SCOPE_SINGLETON = "singleton";  
  
    String SCOPE_PROTOTYPE = "prototype";  
  
    void addBeanPostProcessor(BeanPostProcessor beanPostProcessor);  
  
}
```

扩展行文
[[ConfigurableListableBeanFactory]]

抽象实现
[[AbstractBeanFactory]]