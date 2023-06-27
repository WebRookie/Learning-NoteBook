## Spring进阶 - SpringIOC 实现原理详解之 IOC体系结构设计



### 回顾一下知识

#### 什么是IOC？

IOC就是控制反转。

### 浅浅的设计一个IOC容器？

设计时，首先需要考虑的是 IOC容器的功能（输入和输出）

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-ioc-source-7.png)

在此基础上，去思考主体上应该包含哪几个部分；

* 加载Bean的配置（比如xml配置）
  * 比如不同类型资源的加载，解析成统一Bean的定义
* 根据Bean的定义加载成Bean的实例，并放置在Bean容器中
  * 比如Bean的依赖注入，Bean的嵌套，Bean的存放（缓存）等
* 除了基础Bean外，还有常规针对企业级业务等特殊Bean
  * 比如国际化Message，事件Event等生成特殊的类结构去支撑
* 对容器中的Bean提供统一的管理和调用
  * 比如用工厂模式管理，提供方法根据名字 / 类 的类型等从容器中获取Bean



### BeanFactory和BeanRegistry: IOC容器功能规范和Bean的注册

> Spring Bean的创建是典型的工厂模式，这一系列的bean工厂，也即IOC容器为开发者管理对象间的依赖关系提供了很多遍了和基础服务，在Spring中有许多IOC容器的实现供用户选择和使用，这是IOC容器的基础；在顶层的结构设计主要围绕着BeanFactory和XXXRegistry进行
>
> * BeanFactory: 工厂模式定义了IOC容器的基本规范功能
> * BeanRegistry： 向IOC容器手工注册BeanDefinition对象方法



![img](https://www.pdai.tech/images/spring/springframework/spring-framework-ioc-source-2.png)









## Spring进阶 - Spring IOC实现原理之IOC初始化流程



### 如何将Bean从XML配置中解析后放到IOC容器中？

#### 初始化入口

对于xml配置的Spring应用，在main方法中实例化ClasspathXml Applicationcontext即可创建一个IOC容器，









