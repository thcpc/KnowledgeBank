#abstract


```java
public abstract class AbstractAutowireCapableBeanFactory extends AbstractBeanFactory implements AutowireCapableBeanFactory {  
  
    private InstantiationStrategy instantiationStrategy = new CglibSubclassingInstantiationStrategy();  
  
  
    @Override  
    protected Object createBean(String beanName, BeanDefinition beanDefinition, Object... args) throws BeansException {  
        Object bean = null;  
        try {  
            bean = createBeanInstance(beanDefinition, beanName, args);  
            applyPropertyValues(beanName, bean, beanDefinition);  
            bean = initializeBean(beanName, bean, beanDefinition);  
        } catch (Exception e) {  
            throw new BeansException("Instantiation of bean failed", e);  
        }  
        addSingleton(beanName, bean);  
        return bean;  
    }  
  
    protected Object createBeanInstance(BeanDefinition beanDefinition, String beanName, Object[] args) {  
        Constructor constructor = null;  
        Class beanClass = beanDefinition.getBeanClass();  
        Constructor[] declaredConstructors = beanClass.getDeclaredConstructors();  
        for (Constructor ctor : declaredConstructors) {  
            if (null != args && ctor.getParameterTypes().length == args.length) {  
                constructor = ctor;  
                break;            }  
        }  
        return instantiationStrategy.instantiate(beanDefinition, beanName, constructor, args);  
    }  
  
    protected void applyPropertyValues(String beanName, Object bean, BeanDefinition beanDefinition) {  
        try {  
            PropertyValues propertyValues = beanDefinition.getPropertyValues();  
            for (PropertyValue propertyValue : propertyValues.getPropertyValues()) {  
                String name = propertyValue.getName();  
                Object value = propertyValue.getValue();  
                if (value instanceof BeanReference) {  
                    BeanReference beanReference = (BeanReference) value;  
                    value = getBean(beanReference.getBeanName());  
                }  
                BeanUtil.setFieldValue(bean, name, value);  
            }  
        } catch (Exception e) {  
            throw new BeansException("Error setting property values: " + beanName);  
        }  
    }  
  
    private Object initializeBean(String beanName, Object bean, BeanDefinition beanDefinition){  
        Object wrappedBean = applyBeanPostProcessorsBeforeInitialization(bean, beanName);  
        invokeInitMethods(beanName, wrappedBean, beanDefinition);  
        wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);  
        return wrappedBean;  
    }  
  
    private void invokeInitMethods(String beanName, Object wrappedBean, BeanDefinition beanDefinition){  
  
    }  
  
    @Override  
    public Object applyBeanPostProcessorsAfterInitialization(Object existingBean, String beanName) throws BeansException {  
       Object result = existingBean;  
       for(BeanPostProcessor postProcessor: getBeanPostProcessors()){  
           Object current = postProcessor.postProcessAfterInitialization(result, beanName);  
           if(null == current) return result;  
           return current;  
       }  
       return result;  
    }  
  
    @Override  
    public Object applyBeanPostProcessorsBeforeInitialization(Object existingBean, String beanName) throws BeansException {  
        Object result = existingBean;  
        for(BeanPostProcessor postProcessor: getBeanPostProcessors()){  
            Object current = postProcessor.postProcessBeforeInitialization(result, beanName);  
            if(null == current) return result;  
            return current;  
        }  
        return result;  
    }  
}
```


具体实现:
