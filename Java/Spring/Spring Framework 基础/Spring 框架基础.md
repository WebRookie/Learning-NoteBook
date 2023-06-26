## Spring Framework 5 基础

Spring组成

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-introduce-8.png)

spring 核心

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-helloworld-2.png)

### Spring基础

#### Spring 的特性和优势

* 非侵入式：基于spring开发的应用中的对象可以不依赖于Spring的API
* 控制反转： IOC ---- Inversion of Control 指的是将对象的创建权交给Spring去创建，在使用Spring之前，对象的创建都是由我们在代码中new创建。而使用Spring之后，对象的创建都是给了Spring 的对象。
* 依赖注入： DI ------ Dependecy Injection，是指依赖的对象不需要手动调用 setXX方法去设置，而是通过配置赋值。
* 面向切面编程： Aspect Oriented Programming ---- AOP
* 容器： Spring是一个容器，因为它包含并且管理应用对象的生命周期
* 组件化：Spring实现了使用简单的组件配置组合成了一个复杂的应用，在Spring中可以使用XML和Java注解组合这些对象。
* 一站式： 在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring自身也提供了表现层的SpringMVC和持久层的Spring JDBC）



### Spring组成

#### Core Container （Spring的核心容器）

Spring的核心容器上其他模块建立的基础，由Beans模块、Core核心模块、Context上下文模块和SpEL表达式语言模块组成，没有这些核心容器，也不能有AOP、Web等上层的功能具体如下：

* **Beans模块**：提供了框架的基础部分，包括控制反转和依赖注入。
* **Core核心模块：** 封装了Spring框架的底层部分，包括资源访问、类型转换及一些常用工具类。
* **Context上下文模块：**建立在Core和Beans模块的基础上，集成Beans模块功能并添加资源绑定、数据验证、国际化、JavaEE支持、容器生命周期、事件传播等。ApplicationContext接口是上下文模块等焦点。
* SpEL模块：提供了强大的表达式语言支持，支持访问和修改属性值，方法调用，支持访问及修改数组、容器和索引器，命名变量，支持算数和逻辑运算，支持从Spring容器获取bean，它也支持列表投影、选择和一般的列表聚合等。



#### Data Access/Integration （数据访问 / 集成）

数据访问 / 集成层包括JDBC、ORM、OXM、JMS和Transcations模块，具体介绍如下：

* **JDBC模块：**提供了一个JDBC的样例模版，使用这些模板能消除传统的冗长的JDBC编码还有必须的事务控制，而且能享受Spring管理事务的好处。
* **ORM模块：** 提供与流行的“对象-关系”映射框架无缝集成的API，包括JPA、JDO、Hibernate和Mybatis等，而且还可以使用Spring事务管理，无需额外控制事务。
* **OXM模块：**提供了一个支持Object / XML映射等抽象层实现，如JAXB、Castor、XMLBeans、JiBX和XStream。 将Java对象映射成XML数据，或者将XML数据映射成Java对象。
* **JMS模块：** 指Java消息服务，提供一套“消费生产者、消费消费者”模版用于更加简单的使用JMS，JMS用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。
* **Transcations事务模块：** 支持编程和声明式事务管理。



#### Web模块

Spring的Web层包括Web、Servlet、WebSocket和Webflux组件，介绍如下。

* **Web模块**：提供了基本的Web开发集成特性，例如多文件的上传功能、使用的Servlet监听器的IOC容器初始化以及Web应用上下文。
* **Servlet模块：**提供了一个Spring MVC Web框架实现。Spring MVC 框架提供了机遇注解的请求资源注入、更简单的数据绑定、数据验证等及一套非常易用的JSP标签，完全无缝与Spring其他技术协作。
* **WebSocket模块：**提供了简单的接口，用户只要实现响应的接口就可以快速的搭建WebSocket Server，从而实现双向通讯。
* **Webflux模块：** Spring WebFlux 是Spring Framework 5.x中引入的新的响应式web框架。与Spring MVC不同，它不需要Servlet API，是完全异步且非阻塞的，并且通过Reactor项目实现了Reactive Streams规范。 Spring WebFlex用于创建机遇事件循环执行模型的完全异步且非阻塞的应用程序。



#### AOP、Aspects、Instrumentation和Messaging

在Core Container之上是AOP、Aspects等模块，具体介绍如下：

* **AOP模块**： 提供了面向切面编程实现，提供比如日志记录、权限控制、性能统计等通用功能和业务逻辑分离等技术，并且能动态的把这些功能添加到需要的代码中，这样各司其职，降低业务逻辑和通用功能的耦合。
* **Aspects模块：** 提供了与AspectJ的集成，是一个功能强大且成熟的面向切面编程（AOP）框架
* **Instrumentation模块：**提供了类工具的支持和类加载器的实现，可以在特定的应用服务器中使用。
* **Messaging模块：** Spring 4.0 以后了消息（Spring - messaging）模块，该模块提供了对消息传递体系结构和协议的支持。
* **jcl模块：**Spring 5.x中新增了日志框架集成的模块



#### Test模块

Test模块：Spring 支持 Junit和 TestNG测试框架，而且还提供了一些基于Spring的测试功能，比如在测试Web框架是，模拟Http请求的功能。



### Spring 核心要点

#### 控制反转

* 如果没有Spring框架，就需要自己手动的创建User/Dao/Service等

```java
UserDaoImpl userDao = new UserDaoImpl();
UserSericeImpl userService = new UserServiceImpl();
userService.setUserDao(userDao);
List<User> userList = userService.findUserList();

```

* 有了Spring框架，可以将原有的Bean的创建工作转给框架，需要用时从Bean的容器中获取即可

```java
// create and configure beans
ApplicationContext context =
        new ClassPathXmlApplicationContext("aspects.xml", "daos.xml", "services.xml");

// retrieve configured instance
UserServiceImpl service = context.getBean("userService", UserServiceImpl.class);

// use configured instance
List<User> userList = service.findUserList();

```

Spring 优化的点

1. Spring框架管理这些Bean的创建工作，即由用户管理Bean转变为框架管理Bean，这个就叫**控制反转 - Inversion of Control（IoC）**
2. Spring框架托管创建的Bean放在哪里？ **IoC Container；**
3. Spring 框架为了更好让用户配置Bean， 必然会引入**不同方式配置Bean**？就有了 **xml配置**，**Java配置**，**注解配置**等支持。
4. Spring框架既然接管了Bean的生成，必然需要**管理整个Bean的生命周期**等。
5. 应用程序代码从IoC Container 中获取依赖Bean，注入到应用程序中，这个过程叫做 **依赖注入（Dependency Injection ， DI）**；所以说控制反转时通过依赖注入实现的，其实他们是同一个概念的不同角度描述。通俗来说就是**IoC是设计思想，DI是实现方式**
6. 在依赖注入时，有哪些方式呢？这是构造器方式， @Autowired，@Resource, @Qualifier...同时Bean之间存在依赖（可能是先后顺序问题，以及循环依赖的问题）



#### 面向切面 - AOP

> 给Service 所有方法调用添加日志（调用方法时），本质上是解耦问题；

* 如果没有Spring框架，需要在每个Service的方法中添加记录日志的方法

```java
/**
* find user list.
*
* @return user list
*/
public List<User> findUserList() {
    System.out.println("execute method findUserList");
    return this.userDao.findUserList();
}

```

* 有了Spring框架，通过@Aspect注解 定义了切面，这个切面中定义了拦截了所有的service方法，并记录日志，明显看到，框架将日志记录和业务需求的代码解耦了。不再是侵入式

```java
/**
* aspect for every methods under service package.
*/
@Around("execution(* tech.pdai.springframework.service.*.*(..))")
public Object businessService(ProceedingJoinPoint pjp) throws Throwable {
    // get attribute through annotation
    Method method = ((MethodSignature) pjp.getSignature()).getMethod();
    System.out.println("execute method: " + method.getName());

    // continue to process
    return pjp.proceed();
}
```

知识点：

1. Spring框架通过定义切面，通过拦截切点实现了不同业务模块的解耦。这个就叫做**面向切面编程 - Aspect Oriented Programming（AOP**）
2. 为什么@Aspect注解使用的是asectJ的jar包？ 这就引入出了**Aspect4J和Spring AOP**的历史渊源。
3. 如何支持**更多的拦截方式**来实现解耦，以满足更多的场景需求？ 这就是@Around， @Pointcut...等的设计
4. Spring框架又是如何实现AOP的呢？ 这就引入了**代理技术**，**分静态代理和动态代理**，动态代理又包含JDK代理和CGLIB代理等



```java
 public static void main(String[] args) {
        // create configure beans
//        ApplicationContext context = new ClassPathXmlApplicationContext("aspects.xml"," daos.xml", "services.xml");
//        通过JavaConfig方式配置的bean的方式与xml配置方式引入的类是不一样的  ClassPathXmlApplicationContext
//        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(BeansConfig.class);
//        通过注解方式配置就需要JavaConfig配置了启动方式也有所不一样(在扫描com.rookie.springframework包)
           AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext("com.rookie.springframework");



        // retrieve configured instance
        UserServiceImpl service = context.getBean("userService", UserServiceImpl.class);

        // use configured instance
        List<User> userList = service.findUserList();

        // print info from beans
        userList.forEach(a -> System.out.println(a.getName() + "," + a.getAge()));
    }
```

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-helloworld-8.png)



### 控制反转（Inversion of Control）

#### Spring Bean是什么

> IoC Container管理的是Spring Bean,那么Spring Bean是什么？

Spring里面的bean就类似是定义的一个组件，而这个组件的作用就是实现某个功能的，这里所定义的bean就相当于给你更简便的方法来调用这个组件去实现要完成的功能。



#### Ioc是什么

> IoC - Inversion of Control， 即 “控制反转”， 不是什么技术，而是一种设计思想。在Java开发中，IoC意味着将你设计好的对象交给容器控制，而不是传统的在你的对象内部直接控制。



* 谁控制谁，控制什么？

传统Java SE程序设计，我们直接在对象内部通过new进行创建对象，是程序主动去创建依赖对象；而IoC是有专门一个容器来创建这些对象，即由IoC容器来控制对象的创建，IoC容器控制了对象；控制了外部资源的获取（不只是对象包括比如文件等）

* 为何是反转，哪些方面反转了？

有反转就有正转，传统应用程序是由我们自己在对象中主动控制去直接获取依赖对象，也就是正转；而反转则是由容器来帮忙创建及注入依赖对象；为何是反转？因为有容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象，所以是反转； 哪些方面反转了？依赖对象的获取被反转了。



**传统程序设计**

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-ioc-1.png)

有IoC/ DI 的容器后，在客户端类中不在主动创建这些对象

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-ioc-2.png)

#### IoC和DI是什么关系

> 控制反转是通过依赖注入实现的，其实他们是同一个概念的不同角度描述，通俗来说就是**IoC是设计思想，DI是实现方式。**

* 谁依赖谁

应用程序依赖于IoC容器

* 为什么需要依赖？

应用程序需要IoC容器来提供对象需要的外部资源

* 谁注入谁？

IoC容器注入应用程序的某个对象，应用程序依赖的对象。

* 注入了什么？

就是注入某个对象所需要的外部资源（包括对象、资源、常量数据）



#### IoC配置的三种方式

##### XMl配置

* 优点：可以使用于任何场景，结构清晰，通俗易懂
* 缺点：配置繁琐，不易维护，枯燥无味，扩展性差



#### Java配置

将类的创建交给配置的JavaConfig类完成，Spring只负责维护和管理，采用纯Java创建方式。其实本质上就是把XML上配置声明转移到Java配置类中

* 优点： 适用于任何场景，配置方便，因为是纯Java代码，扩展性高，十分灵活
* 缺点：由于是采用Java类的方式，声明不明显，如果大量配置可读性较差。



#### 注解配置

通过在类上加注解的方式，来声明一个类交给Spring管理，Spring会自动扫描带有@Component，@Controller，@Service，@Respository这个四个注解的类。然后帮我们创建并管理，前提是需要先配置Spring的注解扫描器。

* 优点：开发便捷，通俗易懂，方便维护
* 缺点：具有局限性，对于一些第三方资源，无法添加注解，只能采用XML和JavaConfig的方式配置。



## Spring基础 - Spring核心之面向切面编程（AOP）

### AOP是什么？

> AOP为Aspect Oriented Programming的缩写，意为：面向切面编程

AOP最早是AOP联盟的组织提出的，指定的一套规范，spring将AOP的思想引入框架后之中，通过预编译方式和运行期间动态代理实现程序的统一维护的一种技术。

#### AOP术语

* **连接点（Jointpoint）**：表示需要在程序中插入横切关注点的拓展点，连接点可能是**类初始化，方法执行，方法调用，字段调用或处理异常等等**，spring只支持方法执行连接点，在AOP中表示**在哪里干**；
* **切入点（Pointcut）**：选择一组相关连接点的模式，既可以认为连接点的集合，Spring支持perl5正则表达式和AspectJ切入点模式，Spring默认使用AspectJ语法，在AOP中表示为**在哪里干的集合；**
* **通知（Advice）**：在连接点上执行的行为，通知提供了在AOP中需要在切入点所选择的连接点出进行扩展现有行为的手段；包括前置通知（before advice）、后置通知（after advice）、环绕通知（around advice），在Spring中通过代理模式实现AOP，并通过拦截器模式以环绕连接点的拦截器链织入通知；在AOP中表示**干什么**；
* **方面 / 切面 （Aspect ）**： 横切关注点的模块化，比如提到的日志组件，可以认为是通知、引入和切入点的组合；在Spring中可以使用Schema和@AspectJ方式进行组织实现；在AOP中表示为**在哪干和干什么集合;**
* **引入（inter-type declaration**）：也称为内部类型声明，为已有的类添加额外新的字段或方法，Spring允许引入新的接口（必须对应一个实现）到所有被代理对象（目标对象），在AOP中表示**干什么（引入什么）**
* **目标对象（Target Object）**： 需要被织入横切关注点到对象，即该对象是切入点选择的对象，需要被通知的对象，从而可以称为被通知的对象下；由于Spring AOP通过代理模式实现，从而这个对象永远是被代理对象，在AOP中表示**对谁干**；
* **织入（Waving）**： 把切面连接到其他的应用程序类型或者对象上，并创建一个被通知的对象，这些可以在编译时（例如使用Aspect J编译器），类加载时和运行时完成。Spring和其他春Java AOP框架一样，在运行时完成织入。在AOP中表示为**怎么实现**；
* AOP代理（AOP Proxy）： AOP框架使用代理模式创建的对象，从而实现在连接点出插入通知（即应用切面），就是通过代理来对目标对象引用切面。在Spring中，AOP代理可以用JDK动态代理或CGLIB代理实现，而通过拦截器模型应用切面，在AOP中表示**怎么实现的一种典型方式**



> 通知类型：

* 前置通知（Before advice): 在某链接点之前执行的通知，但这个通知不能阻止连接点之前的执行流程（除非它排出一个异常）
* 后置通知（After returning advice）：在某连接点正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。
* 异常通知（After throwing advce）： 在方法抛出异常退出时执行的通知
* 最终通知（After （finally）advice）： 当连接点退出的时候执行的通知（不论是正常返回还是异常退出）
* 环绕通知（Around Adivce）： 包围一个连接点的通知，如方法调用，这是最强大的一种通知类型，环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它自己的返回值或抛出异常来结束执行





> 串联这些话语

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-aop-3.png)



#### Spring AOP和AspectJ是什么关系

*  首先AspectJ是什么？

AspectJ是一个Java实现的AOP框架，它能够对java代码进行AOP编译（一般在编译期进行），让java代码具有AspectJ的AOP功能（当然需要特殊的编译器）



#### AOP的配置方式

> Spring AOP支持XML模式和基于AspectJ注解的两种配置方式



#### XML Scheme配置方式

Spring提供了使用“aop”命名空间来定义一个切面。示例如下：

* 定义目标类

```java

public class AopDemoServiceImpl {

    public void doMethod1() {
        System.out.println("AopDemoServiceImpl.doMethod1()");
    }

    public String doMethod2() {
        System.out.println("AopDemoServiceImpl.doMethod2()");
        return "hello world";
    }

    public String doMethod3() throws Exception {
        System.out.println("AopDemoServiceImpl.doMethod3()");
        throw new Exception("some exception");
    }
}
```

* 定义切面类

```java
public class LogAspect {
  
  /**
   * 环绕通知
   * 
   * @param pjp 
   * @retrun obj
   **/
  public Object doAround(ProceedingJoinPoint pjp)  throws Throwable {
    System.out.println("----");
    System.out.println("环绕通知：进入方法");
    Object o = pjp.proceed();
    System.out.println("环绕通知：退出方法");
    return o;
  }
  
  /**
   * 前置通知
   */
  public void deBefore() {
    System.out.println("前置通知")
  }
  
  /**
   * 后置通知
   */
  public void doAfterReturning(String result) {
    System.out.println("后置通知， 返回值：" + result);
  }
  
  /**
   * 异常通知
   */
  public void doAfterThrowing(Exception e) {
    System.out.println("异常通知，异常：" + e.getMessage());
  }
  
  /**
   * 最终通知
   */
  public void doAfter() {
    	System.out.println("最终通知");
  }
}
```

* XML配置AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
 http://www.springframework.org/schema/beans/spring-beans.xsd
 http://www.springframework.org/schema/aop
 http://www.springframework.org/schema/aop/spring-aop.xsd
 http://www.springframework.org/schema/context
 http://www.springframework.org/schema/context/spring-context.xsd
">

    <context:component-scan base-package="tech.pdai.springframework" />

    <aop:aspectj-autoproxy/>

    <!-- 目标类 -->
    <bean id="demoService" class="tech.pdai.springframework.service.AopDemoServiceImpl">
        <!-- configure properties of bean here as normal -->
    </bean>

    <!-- 切面 -->
    <bean id="logAspect" class="tech.pdai.springframework.aspect.LogAspect">
        <!-- configure properties of aspect here as normal -->
    </bean>

    <aop:config>
        <!-- 配置切面 -->
        <aop:aspect ref="logAspect">
            <!-- 配置切入点 -->
            <aop:pointcut id="pointCutMethod" expression="execution(* tech.pdai.springframework.service.*.*(..))"/>
            <!-- 环绕通知 -->
            <aop:around method="doAround" pointcut-ref="pointCutMethod"/>
            <!-- 前置通知 -->
            <aop:before method="doBefore" pointcut-ref="pointCutMethod"/>
            <!-- 后置通知；returning属性：用于设置后置通知的第二个参数的名称，类型是Object -->
            <aop:after-returning method="doAfterReturning" pointcut-ref="pointCutMethod" returning="result"/>
            <!-- 异常通知：如果没有异常，将不会执行增强；throwing属性：用于设置通知第二个参数的的名称、类型-->
            <aop:after-throwing method="doAfterThrowing" pointcut-ref="pointCutMethod" throwing="e"/>
            <!-- 最终通知 -->
            <aop:after method="doAfter" pointcut-ref="pointCutMethod"/>
        </aop:aspect>
    </aop:config>

    <!-- more bean definitions for data access objects go here -->
</beans>

```



* 测试类

```java
public static void main(String[] args) {
  // create and configure beans
  ApplicationContext context = new ClassPathXmlApplicationContext("apsect.xml");
  
  // retrieve configured instance
  AopDemoServiceImpl service = context.getBean("demoService", AopDemoServiceImpl.class);
  
  // use configured instance
  service.doMethod1();
  service.doMethod2();
  try {
    service.doMethod3();
  }catch (Exception e) {
    // e.printStackTrace();
  }
}
```

* 输出结果

```
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod1()
环绕通知: 退出方法
最终通知
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod2()
环绕通知: 退出方法
最终通知
后置通知, 返回值: hello world
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod3()
最终通知
异常通知, 异常: some exception
------

```



#### AspectJ注解方式

基于XML的声明式ApectJ存在一些不足，需要在Spring配置文件配置大量的代码信息，为了解决这个问题，spring使用了@AspectJ框架为AOP的实现提供了一套注解

| - 注解名称 -    | - 解释 -                                                     |
| --------------- | ------------------------------------------------------------ |
| @Aspcet         | 用来定义一个切面                                             |
| @pointcut       | 用于定义切入点表达式，在使用时还需要定义一个包含名字和人任意参数的方法签名来表示切入点名称，这个方法签名就是一个返回值为void，且方法体为空的普通方法 |
| @Before         | 用于定义前置通知，相当于@BeforeAdvice，在使用时，通常需要一个value属性值，该属性值用于指定一个切入点表达式（可以是已有的的切入点，也可以直接定义切入点表达式） |
| @AfterReturning | 用于定义后置通知，相当于AfterReturningAdivce,在使用时可以定义pointcut / value和returning属性，其中 pointcut / value 这两个属性的作用一样，都是用于指定切入点表达式。 |
| @Around         | 用于定义环绕通知，相当于MethodInterceptor。在使用时需要指定一个value属性，该属性用于指定该通知被植入的切入点 |
| @After-Throwing | 用于定义异常通知来处理程序中为处理的异常，相当于ThrowAdvice。在使用时可指定pointcut / value 和throwing属性。其中pointcut / value用指定切入点表达式而throwing属性值用于指定一个形参名来表示Advice方法中可定义与此同名的形参，该形参可用于访问目标方法抛出的异常。 |
| @After          | 用于定义最终final通知，不管是否异常，该通知都会执行，使用时需要指定一个value属性，该属性用于指定该通知被植入的切入点。 |
| @DeclareParents | 用于定义引介通知，相当于IntroductionInterceptor              |
|                 |                                                              |

>Spring AOP 的实现是动态织入，动态织入的方式是在运行时动态的要将增强的代码织入到目标类中，这样往往是通过动态代理技术完成的；**如Java JDK的动态代理（Proxy，底层通过反射实现）或者CGLB的动态代理（底层通过继承实现）**，Spring AOP采用的就是基于运行时增强的代理技术。
>
>* 基于JDK代理的例子
>* 基于Cglib代理的例子



##### 接口使用JDK代理

* 定义接口

```java
	public interface IJdkProxyService {
    void deMethod1();
    
    String doMethod2();
    
    String doMethod3() throws Exception;
  }
```

* 实现类

```java
@service
public class JdkProxyDemoServiceImpl implements IJdkProxyService {
  @Override
  public void doMethod1() {
    System.out.println("JdkProxyServiceImpl.doMetho1()");
  }
  
  @Override
  public String doMethod2() {
    System.out.println("JdkProxyServiceImpl.doMethod2()");
  }
  
  @Override
  public String doMethod3() {
    System.out.println("JdkProxyServiceImpl.doMethod3()");
    throw new Exception("mock some exception");
  }
}
```

* 定义切面

```java
@EnableAspectJAutoProxy
@Component
@Aspect
public class LogAspect {
  
  // define point cut
  @Pointcut("execution(*com.rookie.springframework.service.*.*(..))")
  private void pointCutMethod() {
    
  }
  
  // 环绕通知
  @Around("pointCutMethod()")
  public Object doAround(ProceedingJoinPoint pjp) throws Throwable {
    System.out.println("-------");
    System.out.println("环绕通知： 进入方法");
    Object o = pjp.proceed();
    System.out.println("环绕通知： 退出方法");
    return o;
  }
  
  // 前置通知
  @Before("pointCutMethod()")
  public void doBefore() {
    System.out.println("前置通知");
  }
  
  
  // 后置通知
  @AfterReturning(pointcut = "pointCutMethod()")
  public void doAfterReturning(String result) {
    System.out.println("后置通知，返回值：" + result);
  }
  
  // 异常通知
  @AfterThrowing(pointcut = "pointCutMethod()", throwing = "e")
  public void doAfterThrowing(Exception e) {
    System.out.println("异常通知，异常：" + e.getMessage());
  }
  
  // 最终通知
  @After("pointCutMethod()")
  public void doAfter() {
    System.out.println("最终通知");
  }
  
  
  
}
```

* 输出

```
-----------------------
环绕通知: 进入方法
前置通知
JdkProxyServiceImpl.doMethod1()
最终通知
环绕通知: 退出方法
-----------------------
环绕通知: 进入方法
前置通知
JdkProxyServiceImpl.doMethod2()
后置通知, 返回值: hello world
最终通知
环绕通知: 退出方法
-----------------------
环绕通知: 进入方法
前置通知
JdkProxyServiceImpl.doMethod3()
异常通知, 异常: some exception
最终通知
------
```





#### 非接口使用Cglib代理

* 类定义

```java
@Service
public class CglibProxyDemoServiceImpl {
  public void doMethod1() {
    System.out.println("CglibProxyDemoServiceImpl.doMethod1()");
  }
  
  public String doMethod2() {
    System.out.println("CglibProxyDemoServiceImpl.doMethod2()");
  }
  

  public String doMethod3() {
    System.out.println("CglibProxyDemoServiceImpl.doMethod3()");
    throw new Exception("mock some exception");
  }
}
```

* 切面定义

和上面相同

* 输出

```
-----------------------
环绕通知: 进入方法
前置通知
CglibProxyDemoServiceImpl.doMethod1()
最终通知
环绕通知: 退出方法
-----------------------
环绕通知: 进入方法
前置通知
CglibProxyDemoServiceImpl.doMethod2()
后置通知, 返回值: hello world
最终通知
环绕通知: 退出方法
-----------------------
环绕通知: 进入方法
前置通知
CglibProxyDemoServiceImpl.doMethod3()
异常通知, 异常: some exception
最终通知
------

```



### AOP 使用问题小结

#### 切入点（pointcut）的申明规则？

SpringAOP用户可能回家经常使用execution切入点指示符。执行表达式如下

```java
execution（modifiers-pattern? ret-type-pattern declaring-type-pattern? name-pattern（param-pattern） throws-pattern?）

```

* ret-type-pattern 返回类型模式。name-pattern名字模式和param-pattern参数模式是必选的。其他部分都是可选的。返回类型模式决定了方法的返回类型必须依次匹配一个连接点。你会使用的最频繁的返回类型模式是`*` ,**它代表了匹配任意的返回类型**。
* declaring-type-pattern, 一个全限定的类型名将只会匹配返回给定类型的方法。
* name-pattern 名字模式匹配的是方法名，你可以使用`*`通配符作为所有或者部分命名模式
* param-pattern 参数模式有些复杂：（）匹配了一个不接受任何参数的方法，而（..）匹配了一个接受任意数量参数的方法（零或者更多）模式（）匹配了一个接受一个任何类型参数的方法。模式（，String）匹配了一个接受两个参数的方法。第一可以是任意类型的，第二个则必须是String类型。

对应以上的例子

![img](https://www.pdai.tech/images/spring/springframework/spring-framework-aop-7.png)



下面是一些通用切入点表达式的例子

```java
// 任意公共方法的执行
execution (public * * (..))
  
// 任何一个名字以“set”开始的方法的执行
  execution(* set* (..))

  
// AccountService接口定义的任意方法的执行；
execution(* com.xyz.service.AccountService.*(..))
  
  
// 在service包中定义的任意方法的执行
execution (* com.xtz.service.*.* (..))
  
// 在service包或其子包中定义的任意方法的执行；
execution( * com.xyz.service..*.*(..))
  
  
// 在service包中的任意连接点（在Spring AOP中只是方法执行）
within (com.xyz.service.*)
  
// 在service包或其子包中的任意连接点（在Spring AOP中只是方法的执行）
 within (com.xyz.service..*)

// 实现了AccountService接口的代理对象的任意连接点（在Spring AOP中只是方法执行）
this (com.xyz.service.AccountService) // ‘this’在绑定表单中更加常用

// 实现AccountService接口的目标对象的任意连接点（在Spring AOP中只是方法的执行）
target(com.xyz.service.AccountService) // 'target'在绑定表单中更加常用

// 任何一个只接受一个参数，并且运行时所穿入的参数是Seriaizable接口的连接点（在Spring AOP中只是方法的执行）
arg(java.io.Serializable) // ‘args'在绑定表单中，更加常用
// 请注意在例子中给出的切入点不同于 execution(* *(java.io.Serializable))： args版本只有在动态运行时候传入参数是Serializable时才匹配，而execution版本在方法签名中声明只有一个 Serializable类型的参数时候匹配。

// 目标对象中有一个@Transactional注解的任意连接点（在Spring AOP中只是方法的执行）
@target(org.springframework.transcaction.annotation.Transactional) 
  
// 任何一个目标对象声明类型有一个@Transactional注解的连接点（在Spring AOP中只是方法执行）
@within(org.springframework.transaction.annotation.Transactional)
  
  
// 任何一个执行方法有一个@Transactional注解的连接点
@annotation(org.springframework.transaction.annotation.Transactional)
  
  
// 任何有一个只接受一个参数，并且运行时所穿入的参数类型具有@Classified注解的连接点（在Spring AOP中只是方法执行）
@args(com.xyz.security.Classified)
  
  
// 任何一个在名为'tradeService'的Spring bean之上的连接点（在Spring AOP中只是方法执行）
bean(tradeService)
  
  
// 任何一个在名字匹配通配符表达式‘*.Service'的Spring bean之上的连接点（在Spring AOP只是方法执行）
bean(*Service)

```

此外Spring支持如下三个逻辑运算符来组合切入表达式

```
&&：要求连接点同时匹配两个切入点表达式
||：要求连接点匹配任意个切入点表达式
!:：要求连接点不匹配指定的切入点表达式
```



Spring AOP和。AspectJ之间 的关键区别？

Aspect J可以 做Spring AOP干不了的事，它是AOP编程的完全解决方法，Spring AOP则致力于解决企业级开发中最普遍的AOP（方法织入）

| Spring AOP                                       | Aspect J                                                     |
| ------------------------------------------------ | ------------------------------------------------------------ |
| 在纯Java中实现                                   | 使用Java编程语言的扩展实现                                   |
| 不需要单独的编制过程                             | 除非设置LTW，否则需要Aspect J编译器（ajc）                   |
| 只能使用运行时织入                               | 运行时织入不可用。支持编译时、编译后和加载时织入             |
| 功能不强-仅支持方法级编织                        | 更强大 - 可以编织字段、方法、构造函数、静态初始值设定项、最终类 / 方法等... |
| 只能在Spring容器管理等bean上实现                 | 可以在所有域对象上实现                                       |
| 仅支持方法执行切入点                             | 支持所有切入点                                               |
| 代理是由目标对象创建的，并且切面应用在这些代理上 | 在执行应用程序之前（在运行时）前，各方面直接在代码中进行织入 |
| 比Aspect J 慢                                    | 更好的性能                                                   |
| 易于学习和应用                                   | 相对于Spring AOP来说更复杂                                   |





## Spring基础 - SpringMVC请求流程和案例



### 什么是MVC

> MVC英文是Model View Controller， 是模型（model） - 视图（view） - 控制器（controller）的缩写，一种软件设计规范，本质也是一种解耦

![img](https://www.pdai.tech/images/spring/springframework/spring-springframework-mvc-4.png)



* Model （模型） 是应用程序中用于处理应用程序数据逻辑的部分，通常模型对象负责在数据库中存储数据。
* View （视图） 是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的
* Controller （控制器）是应用程序中处理中用户交互的部分，通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。



### 什么是Spring MVC

> Spring Web MVC 是一种基于Java 的实现了Web MVC 设计模式的请求驱动类型的轻量级Web 框架，即使用了MVC 架 构模式的思想，将 web 层进行职责解耦，基于请求驱动指的就是使用请求-响应模型，框架的目的就是帮助我们简化开 发，Spring Web MVC 也是要简化我们日常Web 开发的。
>
> 

> 简单而言，Spring MVC 是Spring Container Core 和AOP等技术基础上，遵循Web MVC规范推出的web开发框架，目的是为了简化Java栈的web开发



### Spring MVC的请求流程

> Spring Web MVC 框架也是一个基于请求驱动的Web框架，并且也使用了前端控制器模式来进行设计，再根据请求映射 规则分发给相应的页面控制器（动作 / 处理器）进行处理。



#### 核心架构的具体流程步骤

![img](https://www.pdai.tech/images/spring/springframework/spring-springframework-mvc-5.png)

核心架构的具体流程步骤如下：

1. **用户发送请求 ----> DispatcherServlet**， 前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制；
2. **DispatcherServlet ----> HandlerMapping** ， HandlerMapping将会把请求映射为Handler ExceptionChain对象 （包含一个Handler处理器（页面控制器）对象、多个HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新的映射策略；
3. **DispatcherServlet ----> HandlerAdapter**, HandlerAdapter 将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器；
4. HandlerAdapter ----> 处理器功能处理方法的调用， HandlerAdaptor将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView对象（包含模型数据、逻辑视图名）
5. ModelAndView的逻辑视图名 ----> ViewResolver， ViewResolver将把逻辑视图名解析为具体的View，通过这种策略模式，很容易更换其他的视图技术；
6. View -----> 渲染, View会根据传进来的Model模型进行渲染，此处的Model实际是一个Map数据结构，因此很容易支持其他视图技术；
7. 返回控制权给DispatcherServlet，由DispatcherServlet， 由DispatcherServlet返回响应给用户，到此一个流程结束。



#### 补充

1. Filter（ServletFilter）

进入Servlet前可以有preFilter，Servlet处理后还可以有postFilter

![img](https://www.pdai.tech/images/spring/springframework/spring-springframework-mvc-8.png)



2. 对于文件的上传请求？

对于常规请求上述流程是合理的，但是如果是文件的上传请求，那么就不太一样；所以便有了MultipartResolver







