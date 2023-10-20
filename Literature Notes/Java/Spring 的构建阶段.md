
# 需求1. 02
1. 希望有一个对象，可以保存已经常见的对象，并且可以复用

## 方案
```plantuml
@startuml
package tinny.springframework.beans{
	class BeanDefinition{
		- Object Bean
	}

	class BeanFactory{
		- HashMap<String,BeanDefinition> beans
		Object getBean(String beanName)
		void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
	}

	BeanDefinition o-- BeanFactory
}
@enduml
```

## Bean
```java
public class UserService {  
    public void queryUserInfo(){  
        System.out.println("query user info");  
    }  
}
```


## 使用Bean Factory
```java
BeanDefinition beanDefinition = new BeanDefinition(new UserService());
BeanFactory beanFactory = new BeanFactoru();
beanFactory.registerBeanDefinition("userService Bean",beanDefinition);
UserService userService = beanFactory.getBean("userService Bean"); # 获取生成的Bean对象
userService.queryInfo():
```
# 需求2. 03
1. BeanDefinition 期望不要保存已经实例的对象，只需保存期望对象的Class
2. 期望已单态模式添加的Bean对象，但未来不排除可能会希望每次多是新的实例对象
## 方案
1. 抽象 BeanDefinition 注册过程 - BeanDefinitionRegistry
2. 抽象 Bean的 添加过程 - SingletonBeanRegistry
3. 抽象 Bean的 获取过程 - BeanFactory

```plantuml
@startuml
package tinny.springframework.beans.factory{
	interface BeanFactory{
		Object getBean(String beanName)
	}

}



package tinny.springframework.beans.factory.config{
	
	interface SingletonBeanRegistry {  
	    Object getSingleton(String beanName)  
	}
	class BeanDefinition{
		-Class clazz
	}

}


package tinny.springframework.beans.factory.support{
	interface BeanDefinitionRegistry{
		void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
	}


	abstract class AbstractBeanFactory extends DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.BeanFactory{
		{field} Object getBean(String beanName)
		{abstract} {method} BeanDefinition getBeanDefinition(String beanName)
		{abstract} {method} Object createBean(BeanDefintion beanDefinition)
	}

	
	abstract class AbstractAutowireCapableBeanFactory extends AbstractBeanFactory{
		{method} Object createBean(BeanDefintion beanDefinition)
	}

	class DefaulitListableBeanFactory extends AbstractAutowireCapableBeanFactory implements BeanDefinitionRegistry{
		{field} -HashMap<String,BeanDefinition> beanDefinitionMaps
		{method} void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
		{method} BeanDefinition getBeanDefinition(String beanName)
	}

	class DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.config.SingletonBeanRegistry{  
	    {field} #HashMap<String,Object> beans    
	    {method} Object getSingleton(String beanName)
	    {method} void addSingleton(String beanName, Object bean)
	}
	tinny.springframework.beans.factory.config.BeanDefinition o-- DefaulitListableBeanFactory
	
	note top of BeanFactory : 抽象出获取getBean的方法，解耦获取过程
	note left of SingletonBeanRegistry : 抽象定义Bean得方法，解耦定义Bean的过程
	note top of DefaultSingletonBeanRegistry
		真正存放 <font color="Red">Bean对象</font>的对象
	end note
	
	note left of BeanDefinitionRegistry: 抽象注册BeanDefintion的方法，解耦 注册的过程
	
	note left of AbstractBeanFactory
		模版模式，定义了getBean的过程，
			1.单态获取Bean - getSingleton(String beanName)
			2.如果 Bean 不存在，则 获取BeanDefinition - getBeanDefinition(String beanName)
			3.创建Bean， 并单例保存 - createBean(BeanDefintion beanDefinition)
	end note
	note left of AbstractAutowireCapableBeanFactory: 实现了 createBean
	note left of DefaulitListableBeanFactory
		实现了 registerBeanDefintion 注册BeanDefintion, 存放 <font color="Red">BeanDefinitions</font>
	end note
}
@enduml
```

## Bean
```java
public class UserService {  
    public void queryUserInfo(){  
        System.out.println("query user info");  
    }  
}
```

## 使用Bean Factory

```java
BeanDefinition beanDefinition = new BeanDefinition(UserService.class);  
  
DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();  
beanFactory.registerBeanDefinition("UserService", beanDefinition);  
UserService userService = (UserService) beanFactory.getBean("UserService");  
userService.queryUserInfo();
```



# 需求3.  04
1. Bean对象带有构造函数
2. 实例化策略可能选择JDK本身的方式，也可以选择Cglib


```plantuml
@startuml
package tinny.springframework.beans.factory{
	interface BeanFactory{
		Object getBean(String beanName)
	}

}



package tinny.springframework.beans.factory.config{
	
	interface SingletonBeanRegistry {  
	    Object getSingleton(String beanName)  
	}
	class BeanDefinition{
		-Class clazz
	}

}


package tinny.springframework.beans.factory.support{
	interface InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}
	
	interface BeanDefinitionRegistry{
		void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
	}


	abstract class AbstractBeanFactory extends DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.BeanFactory{
		{field} Object getBean(String beanName)
		{abstract} {method} BeanDefinition getBeanDefinition(String beanName)
		{abstract} {method} Object createBean(BeanDefintion beanDefinition)
	}

	
	abstract class AbstractAutowireCapableBeanFactory extends AbstractBeanFactory{
		{field} InstantiationStrategy instantiationStrategy
		{method} Object createBean(BeanDefintion beanDefinition)
		{method} Object createBeanInstance(String beanName, BeanDefinition beanDefinition, Object[] args)
	}

	class CglibSubclassingInstantiationStrategy implements InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}

	class SimpleInstantiationStrategy implements InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}

	class DefaulitListableBeanFactory extends AbstractAutowireCapableBeanFactory implements BeanDefinitionRegistry{
		{field} -HashMap<String,BeanDefinition> beanDefinitionMaps
		{method} void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
		{method} BeanDefinition getBeanDefinition(String beanName)
	}

	class DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.config.SingletonBeanRegistry{  
	    {field} #HashMap<String,Object> beans    
	    {method} Object getSingleton(String beanName)
	    {method} void addSingleton(String beanName, Object bean)
	}
	tinny.springframework.beans.factory.config.BeanDefinition o-- DefaulitListableBeanFactory
	AbstractAutowireCapableBeanFactory --> InstantiationStrategy: use

	note top of BeanFactory : 抽象出获取getBean的方法，解耦获取过程
	note left of SingletonBeanRegistry : 抽象定义Bean得方法，解耦定义Bean的过程
	note top of DefaultSingletonBeanRegistry
		真正存放 <font color="Red">Bean对象</font>的对象
	end note
	
	note left of BeanDefinitionRegistry: 抽象注册BeanDefintion的方法，解耦 注册的过程
	
	note left of AbstractBeanFactory
		模版模式，定义了getBean的过程，
			1.单态获取Bean - getSingleton(String beanName)
			2.如果 Bean 不存在，则 获取BeanDefinition - getBeanDefinition(String beanName)
			3.创建Bean， 并单例保存 - createBean(BeanDefintion beanDefinition)
	end note
	note left of AbstractAutowireCapableBeanFactory: 实现了 createBean
	note left of DefaulitListableBeanFactory
		实现了 registerBeanDefintion 注册BeanDefintion, 存放 <font color="Red">BeanDefinitions</font>
	end note
}
@enduml
```

# 需求4. 05
1. 增加Bean对象的属性定义
	- 原生类型
	- 引用对象
```plantuml
@startuml
package tinny.springframework.beans.factory{
	interface BeanFactory{
		Object getBean(String beanName)
	}
	class PropertyValue{
		-String name
		-Object value
	}

	class PropertyValues{
		-List propertyValues
		{method} addProperty(Property proertyValue)
		{method} getPropertyValues()
		
	}

}



package tinny.springframework.beans.factory.config{
	
	interface SingletonBeanRegistry {  
	    Object getSingleton(String beanName)  
	}
	class BeanDefinition{
		-Class clazz
	}

}


package tinny.springframework.beans.factory.support{
	interface InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}
	
	interface BeanDefinitionRegistry{
		void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
	}


	abstract class AbstractBeanFactory extends DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.BeanFactory{
		{field} Object getBean(String beanName)
		{abstract} {method} BeanDefinition getBeanDefinition(String beanName)
		{abstract} {method} Object createBean(BeanDefintion beanDefinition)
	}

	
	abstract class AbstractAutowireCapableBeanFactory extends AbstractBeanFactory{
		{field} InstantiationStrategy instantiationStrategy
		{method} Object createBean(BeanDefintion beanDefinition)
		{method} Object createBeanInstance(String beanName, BeanDefinition beanDefinition, Object[] args)
	}

	class CglibSubclassingInstantiationStrategy implements InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}

	class SimpleInstantiationStrategy implements InstantiationStrategy{
		Object instantiate(BeanDefinition beanDefinition, Object[] args)
	}

	class DefaulitListableBeanFactory extends AbstractAutowireCapableBeanFactory implements BeanDefinitionRegistry{
		{field} -HashMap<String,BeanDefinition> beanDefinitionMaps
		{method} void registerBeanDefintion(String beanName, BeanDefinition beanDefinition)
		{method} BeanDefinition getBeanDefinition(String beanName)
	}

	class DefaultSingletonBeanRegistry implements tinny.springframework.beans.factory.config.SingletonBeanRegistry{  
	    {field} #HashMap<String,Object> beans    
	    {method} Object getSingleton(String beanName)
	    {method} void addSingleton(String beanName, Object bean)
	}
	tinny.springframework.beans.factory.config.BeanDefinition o-- DefaulitListableBeanFactory
	PropertyValue o-- PropertyValues
	tinny.springframework.beans.factory.config.BeanDefinition --> PropertyValues: use
	AbstractAutowireCapableBeanFactory --> InstantiationStrategy: use

	note top of BeanFactory : 抽象出获取getBean的方法，解耦获取过程
	note left of SingletonBeanRegistry : 抽象定义Bean得方法，解耦定义Bean的过程
	note top of DefaultSingletonBeanRegistry
		真正存放 <font color="Red">Bean对象</font>的对象
	end note
	
	note left of BeanDefinitionRegistry: 抽象注册BeanDefintion的方法，解耦 注册的过程
	
	note left of AbstractBeanFactory
		模版模式，定义了getBean的过程，
			1.单态获取Bean - getSingleton(String beanName)
			2.如果 Bean 不存在，则 获取BeanDefinition - getBeanDefinition(String beanName)
			3.创建Bean， 并单例保存 - createBean(BeanDefintion beanDefinition)
	end note
	note left of AbstractAutowireCapableBeanFactory
	 实现了 createBean
	 注入了属性和依赖对象
	end note
	note left of DefaulitListableBeanFactory
		实现了 registerBeanDefintion 注册BeanDefintion, 存放 <font color="Red">BeanDefinitions</font>
	end note
}
@enduml
```
