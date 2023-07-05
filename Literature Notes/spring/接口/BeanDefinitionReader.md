类型：接口
定义行为：
使用 ResourceLoader 通过 [[BeanDefinitionRegistry]] 向Bean Factory添加[[BeanDefinition]]

```java
public interface BeanDefinitionReader {  
    BeanDefinitionRegistry getRegistry();  
    ResourceLoader getResourceLoader();  
    void loadBeanDefinition(Resource resource) throws BeansException;  
    void loadBeanDefinition(Resource... resources) throws BeansException;  
    void loadBeanDefinition(String location) throws BeansException;  
    void loadBeanDefinition(String... locations) throws BeansException;  
}
```