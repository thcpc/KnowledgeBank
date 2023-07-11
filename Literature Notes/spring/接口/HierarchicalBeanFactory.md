类型：接口
定义行为：
提供可以获取父类 Bean Factory的方法，属于一种扩展工厂的层次接口,
使BeanFactory 可层次化

```java
public interface HierarchicalBeanFactory extends BeanFactory{  
    BeanFactory getParentBeanFactory();  
}
```