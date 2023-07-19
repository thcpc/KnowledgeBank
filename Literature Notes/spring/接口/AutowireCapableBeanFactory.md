#interface
类型: 接口
定义行为:
扩展了[[BeanFactory]]的基本行为，增加定义了实例化 Bean过程中处理前置任务和后置任务

```java
public interface AutowireCapableBeanFactory extends BeanFactory {  
    Object applyBeanPostProcessorsBeforeInitialization(Object existingBean, String beanName) throws BeansException;  
    Object applyBeanPostProcessorsAfterInitialization(Object existingBean, String beanName) throws BeansException;  
  
}
```

扩展行为
[[ConfigurableListableBeanFactory]]
抽象实现
[[AbstractAutowireCapableBeanFactory]]