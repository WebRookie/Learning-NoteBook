## SpringBoot入门 - 开发中常用的注解



* S pring 开发中常用的注解
  * Spring Boot 常用注解
    * @SpringBootApplication
    * @EnableAutoConfiguration
    * @ImportResource
    * @Value
    * @ConfigurationProperties(prefix = "persion")
    * @EnableConfigurationProperties
    * @RestController
    * @RequestMapping("/api2/copper")
    * @RequestParam
    * @ResponseBody
    * @Bean
    * @Service
    * @Controller
    * @Repository
    * @Component
    * @PostConstruct
    * @PathVariable
    * @ComponentScan
    * @EnableZuulProxy
    * @Autowired
    * @Configuration
    * @Import(Config1.class)
    * @Order
    * @ConditionalOnExpression
    * @ConditionalOnProperty
    * @ConditionalOnClass
    * @ConditionalOnMissingClass({ApplicationManager.class})
    * @ConditionOnMissingBean(name = "example")



### Spring Boot 常用注解

#### @SpringBootApplication

定义在main方法入口类处，用于启动spring boot应用项目



#### @EnableAutoConfiguration

让Spring boot根据类路径中的jar包依赖当前项目进行自动配置

在src/main/resources的META - INF / spring.factories

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration

```



#### @ImportResource

加载xml配置，一般是放在启动main类上

```
@ImportResource("classpath*:/spring/*.xml")单个

@ImportResource({"classpath*:/spring/1.xml", "classpath*:/spring/2.xml"}) 多个
```



#### @Value

application.properties定义属性，直接使用@Value注入即可

```
public class A {
	@Value("${push.start:0}")  如果缺失，默认值为0
	private Long id;
}
```





#### @ConfigurationProperties(prefix = "person")

可以新建一个properties文件， ConfigurationProperties的属性prefix指定properties的配置前缀，通过location指定properties文件的位置

```
@ConfigurationProperties(prefix ="person")
public class PersonProperties {
	private String name;
	priavte int age;
}
```





#### @EnableConfigurationProperites

用@EnableConfiguraionPropertires注解使 @ConfigurationProperties生效，并从IOC容器中获取bean



#### @RequestrParam

获取request请求的参数值

```
public List<CopperVO> getOpList(HttpServletReuqest request,
	@RequestParam(value = "pageIndex", required = false) Integer pageIndex,
	@ReuqestParam(value = "pageSize", required = false) Integer pageSize) {
	
	}
)
```





#### @ResponseBody

支持将返回值放在response体内，而不是返回一个页面，比如Ajax接口，可以使用此注解返回数据而不是页面，此注解可以放在返回值前或方法前

```
另一种写法，不用@ResponseBody，
继承FastJsonHttpMessageConverter类并对writeInternal方法扩展，在Spring响应结果时，再次拦截、加工结果

//stringResult: json返回结果
// HttpOutputMessage outputMessage




```





#### @Bean

@Bean（name = "bean的名字", initMethod = "初始化时调用方法名字", destoryMethod = "close"）

定义在方法上，在容器内初始化一个bean实例类

```
@Bean(destoryMethod ="close")
@ConditionalOnMissingBean
public PersonService registryService() {
	return new PersonService();
}
```





#### @PostConstruct

spring容器初始化时，要执行的方法

```java
@PostContruct
public void init() {

}
```



#### @PathVariable

用来获取请求url中的动态参数

```java
	

@Controller
public class TestController {
  @RequestMapping(value="/user/{userId}/roles/{roleId}", method= RequestMethod.GET)
  public String getLogin(@PathVariable("userId") String userId, @PathVariable("roleId") String roleId ) {
    return "hello";
  }
}
```

 

#### @ComponentScan

注解会告知Spring扫描指定的包来舒适化Spring

```
@Componnent(basePackages = "com.bbs.xxx")
```



#### @EnableZuulProxy

路由网关的主要目的是为了让所有的微服务队外只有一个接口，我们只需要访问一个网关地址，即可由网关将所有的请求代理到不同的服务中，Spring Cloud是通过Zuul来实现的，支持自动路由映射到在Enreka Server上注册的服务。Spring Cloud提供了注解@EnableZuulProxy来启用路由代理





#### @Configuration

表示一个配置信息类，可以给这个配置类也起一个名称





#### @Import（Config.class）

导入Config配置类里实例化的bean



#### @Order

@order（1），值越小优先级超高，越先运行



#### @ConditionalOnExpression

```
@ConditionalonExpression("${enabled: fasle}")
```

开关为true的时候才会实例化bean、



#### @ConditionalOnProperty

这个注解能够控制某个@Configuraion是否生效，具体操作是通过器两个属性name和havingValue来实现，其中name用来从application.properties中读取某个属性值，如果改值为空，则返回false；如果不为空，则将该值域havingVlaue指定的值进行比较。如果一样则返回true否则返回false。如果返回false，则该configuration不生效，为true则生效。



#### @ConditionalOnClass

该注解的参数对应的类必须存在，否则不解析该注解修饰的配置类

```
@ConditionalOnClass({Gson.class})
```



#### @ConditionalOnMissingClass({ApplicationManager.class})

如果存在它修饰的类的bean，则不需要再创建这个bean





#### @ConditionalOnMissin gBean(name = "example")

表示如果name为“example”的bean存在，该注解修饰的代码块不执行。























