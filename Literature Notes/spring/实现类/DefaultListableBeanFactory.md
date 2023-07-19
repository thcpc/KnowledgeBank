#实现类

```java
public  class DefaultListableBeanFactory extends AbstractAutowireCapableBeanFactory implements BeanDefinitionRegistry, ConfigurableListableBeanFactory {  
    private Map<String, BeanDefinition> beanDefinitionMap = new HashMap<>();  
  
    @Override  
    public BeanDefinition getBeanDefinition(String beanName) {  
        BeanDefinition beanDefinition = beanDefinitionMap.get(beanName);  
        if(beanDefinition == null){  
            throw new BeansException("No Bean Named" + beanName + "is defined");  
        }  
        return beanDefinition;  
    }  
  
    @Override  
    public void preInstantiateSingletons() throws BeansException {  
        beanDefinitionMap.keySet().forEach(this::getBean);  
    }  
  
//    @Override  
//    public void addBeanPostProcessor(BeanPostProcessor beanPostProcessor) {  
//  
//    }  
  
    @Override  
    public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition) {  
        this.beanDefinitionMap.put(beanName, beanDefinition);  
    }  
  
    @Override  
    public boolean containsBeanDefinition(String beanName) {  
        return beanDefinitionMap.containsKey(beanName);  
    }  
  
    @Override  
    public <T> Map<String, T> getBeansOfType(Class<T> type) throws BeansException {  
        Map<String, T> result = new HashMap<>();  
        beanDefinitionMap.forEach((beanName, beanDefinition)->{  
            Class beanClass = beanDefinition.getBeanClass();  
            if(type.isAssignableFrom(beanClass)){  
                result.put(beanName, (T) getBean(beanName));  
            }  
        });  
        return result;  
    }  
  
    @Override  
    public String[] getBeanDefinitionNames() {  
        return beanDefinitionMap.keySet().toArray(new String[0]);  
    }  
  
    @Override  
    public BeanFactory getParentBeanFactory() {  
        return null;  
    }  
}
```