
# 阶段1. 最简单的容器
## 1. 定义BeanFactory
- 注册 BeanDefinition 到 Hash Map构建的Bean容器中
- 从BeanDefinition中获取Bean对象
## 2. 定义BeanDefintion
- 存储构造好的Bean对象

## 使用
```java
//构造 Bean对象，并把构造好的Bean对象存放在 BeanDefintion中
BeanDefinition beanDefinition = new BeanDefinition(new UserService());  
BeanFactory beanFactory = new BeanFactory();  
//通过 Bean Factory 注册 BeanDefintion中 到容器中
beanFactory.registerBeanDefinition("userService", beanDefinition);
//使用时 从BeanFactory中获取 Bean对象
UserService userService = (UserService) beanFactory.getBean("userService");  
userService.queryUserInfo();
```

# 阶段2. 对Bean得注册，定义，获取 进行解耦
## 1. 解藕 存放实例化Bean的方式
期望对Bean实现单态模式，这样不用重复实例化，
- 定义 SingletonBeanRegistry 接口
- DefaultSingletonBeanRegistry 实现 SingletonBeanRegistry
DefaultSingletonBeanRegistry 才是真正存放已实例化的Bean对象


## 2. 解耦获取过程
- 修改BeanFactory 为接口
- AbstractBeanFactory 定义获取过程(模版方法)
	1. 看是否能获取已经实例化的Bean
	2. 如果没有实例化，获取 BeanDefintion
	3. 实例化 Bean，并把实例化的Bean存放进 DefaultSingletonBeanRegistry

- AbstractAutowireCapableBeanFactory 定义实例化过程

- DefaultListableBeanFactory,BeanDefinitionRegistry 实现了对BeanDefinition的注册过程