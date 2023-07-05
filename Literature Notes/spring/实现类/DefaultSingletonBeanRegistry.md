类型：实现Class  
实现接口：[[SingletonBeanRegistry]]
实现内容：
实现了Bean的单态获取

```java
public class DefaultSingletonBeanRegistry implements SingletonBeanRegistry {  
    private Map<String, Object> singletonObjects = new HashMap<>();  
  
    @Override  
    public Object getSingleton(String beanName) {  
        return this.singletonObjects.get(beanName);  
    }  
  
    protected void addSingleton(String beanName, Object singletonObject){  
        this.singletonObjects.put(beanName, singletonObject);  
    }  
}
```
