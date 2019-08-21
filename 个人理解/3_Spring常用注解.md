## Spring常用注解整理

* @Target
> 定义了Annotation所修饰的范围，一般分为以下几类：  
ANNOTATION_TYPE ,  
CONSTRUCTOR ,  
FIELD ,  
LOCAL_VARIABLE ,  
METHOD , 
PACKAGE ,  
PARAMETER ,  
TYPE ,  
TYPE_PARAMETER 

* @Retention
> 定义了该Annotation被保留的时间长短：某些Annotation仅出现在源代码中，而被编译器丢弃；而另一些却被编译在class文件中；编译在class文件中的Annotation可能会被虚拟机忽略，而另一些在class被装载时将被读取。  
1.SOURCE:在源文件中有效（即源文件保留）  
2.CLASS:在class文件中有效（即class保留）    
3.RUNTIME:在运行时有效（即运行时保留）    

* @Documented
>用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解。

* @Inherited
> 是一个标记注解，
类继承关系中，子类会继承父类使用的注解中被@Inherited修饰的注解  
接口继承关系中，子接口不会继承父接口中的任何注解，不管父接口中使用的注解有没有被@Inherited修饰  
类实现接口时不会继承任何接口中定义的注解

* @Repeatable
> 1.8之后新引入的，注解表明标记的注解可以多次应用于相同的声明或类型

* @Deprecated
> 被注解@Deprecated标记的代码是被弃用，是不鼓励使用

* @FunctionalInterface
> 一个标识注解，用来标示这个接口为函数式接口。但是一旦被标记了这个注解则接口中只能有一个抽象的方法。  
如果接口声明了一个覆盖java.lang.Object的公共方法之一的抽象方法，那么它也不会计入接口的抽象方法计数，因为接口的任何实现都将具有java.lang.Object或其他地方的实现，所以下面的equals方法不会计入接口的抽象方法计数。
```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
    boolean equals(Object obj);
}
```

* @Autowired
> 先按类型去注入，如果找到多个则在按照名称去注入。required属性用来指定是否必须装配，你需要谨慎对待。如果在你的代码中没有进行null检查的话，这个处于未装配状态的属性有可能会出现NullPointerException 
@see @Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})  
在修饰构造器，方法的时候，参数的值全部从容器中获取。
 
* @Qualifier
> 按照指定名称来装配。也可以自定义限定符注解。如此一来在实体上与注入处的注解一致。则即可指定用来装配的实体。
```java
@Target({ElementType.CONSTRUCTOR, ElementType.FIELD,
         ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier
public @interface Cold { }

@Bean
@Cold
public Dessert iceCream() {
    return new IceCream();
}
@Autowired
@Cold
public void setDessert(Dessert dessert) {
    this.dessert = dessert;
}
```

* @Primary
> 按照类型装配时。如果找到多个相同类型的Bean。则优先使用@Primary修饰的。前提是没有@Qualifier明确指定。

* @Resource
> 作用和@Autowired一样,JSR250提供的注解。但是不能配合@Primary来使用。

* @Inject
> JSR330的注解。作用和@Autowired一样。但是使用前需要导入javax.inject这个包。

* @Order
> @Order(1)也可以用来指定加载顺序。

* @Configurable
> 实体类的实例使用new创建，也即它们不是被Spring管理的。现在我们想要在这个类型中注入对应的Service或DAO类型，被注入类型是被Spring管理的，以上这些操作可以在@Configurable的帮助下完成。https://blog.csdn.net/qq_42786993/article/details/88045834这个博文讲的很清楚。

* @Configuration
> 声明这个类是一个spring的配置类

* @Bean
> 向IOC容器中注册一个Bean。ID为方法名称。也可在name属性中指定ID,initMethod用来指定init-method,destroyMethod用来指定destroy-method。  
 在Lite Mode(类的修饰不是Configuration比如说类被Component修饰)中，@Bean方法中的参数将不会从IOC容器中拿取。   

* @DependsOn
> 该注解用于声明当前bean依赖于另外一个bean。所依赖的bean会被容器确保在当前bean实例化之前被实例化。

* @ComponentScan
> 用来自动扫描组件，  
includeFilters = Filter[]属性,指定包含哪些组件 但是要将useDefaultFilters设置为false
excludeFilters = Filter[]属性,指定排除哪些组件
basePackages属性中可以跟多个String类型的包名。也可以指定为类。如果指定类的话。该类所在的包则会作为基础包
* ComponentScans

* @PostConstruct
> 作用于方法上，用来标明执行的初始化方法。JSR250中提出

* @PreDestroy
> 作用于方法上，用来标明容器销毁对象之前执行的方法。JSR250中提出

* @Controller
* @Service
* @Repository
* @Component
> 标示这类为容器需要装配的组件

* @Fliter 
> 在ComponentScan的includeFilters属性或者excludeFilters属性来使用。他的过滤策略在FilterType这个类中。按照给定的类型，按照注解，AspectJ表达式，按照正则表达式，自定义类型。自定义类型可以实现TypeFilter接口，用来自定义过滤规则。

* @Scope
> 用来指定实例的作用域，默认为SINGLETON
	 * @see ConfigurableBeanFactory#SCOPE_PROTOTYPE
	 * @see ConfigurableBeanFactory#SCOPE_SINGLETON
	 * @see org.springframework.web.context.WebApplicationContext#SCOPE_REQUEST
	 * @see org.springframework.web.context.WebApplicationContext#SCOPE_SESSION

* @Lazy
> 是否懒加载，只作用于SINGLETON的实例

* @Conditional 
> 这个注解可以标注在类上，也可以在方法上。实现Condition接口。用来自定义过滤条件

* @Profile
> 根据不同的profile来注册不同的组件。其实现原理如@Conditional接口，spring实现来ProfileCondition类。通过获得正在使用的Profiles来做对比。  
spring.profiles.active和spring.profiles.default。如果设置了spring.profiles.active属性的话，那么它的值就会用来确定哪个profile是激活的。但如果没有设置spring.profiles.active属性的话，那Spring将会查找spring.profiles.default的值。如果spring.profiles.active和spring.profiles.default均没有设置的话，那就没有激活的profile，因此只会创建那些没有定义在profile中的bean。
有多种方式来设置这两个属性：
作为DispatcherServlet的初始化参数；
作为Web应用的上下文参数；
作为JNDI条目；
作为环境变量；
作为JVM的系统属性；
在集成测试类上，使用@ActiveProfiles注解设置。

* @Value
> 对属性进行复制。可以使用基本类型。也可以使用SPEL#{}，或者使用占位符${}，但是在使用占位符时需要加载配置文件。

* @PropertySource
> 用来加载properties文件

* @PropertySources
> 同PropertySource一样，可跟多个PropertySource。

* @Import
> 1.第一种方式 @Import(Test.class) 直接向IOC容器注册一个组件,也可以注册一个被Configuration修饰的配置类。  
  2.第二种方式 实现ImportSelector接口，返回需要导入的组件的全类名数组。  
  3.第三种方式 实现ImportBeanDefinitionRegistrar接口。自定义注册Bean到容器中

* @ImportResource
> 用来导入xml配置文件。

* @Aspect
> 定义一个切面

* @Pointcut
> 定义一个切点

* @Before
> 通知方法会在目标方法调用之前执行

* @After
> 通知方法会在目标方法返回或抛出异常后调用

* @AfterReturning
> 通知方法会在目标方法返回后调用

* @AfterThrowing
> 通知方法会在目标方法抛出异常后调用

* @Around
> 通知方法会将目标方法封装起来，环绕通知。在方法的前后执行

* @DeclareParents
> value属性指定了哪种类型的bean要引入该接口。defaultImpl属性指定了为引入功能提供实现的类
 ```java
@Aspect
public class AspectConfig {
    //"+"表示person的所有子类；defaultImpl 表示默认需要添加的新的类
    @DeclareParents(value = "com.lzj.spring.annotation.Person+", defaultImpl = FemaleAnimal.class)
    public Animal animal;
}
```

* @ResponseStatus
> 将异常映射为状态码。或者改变当前异常返回的状态码。 具体看SpringInAction 7.3节

* @ExceptionHandler
> 能在同一个控制器中处理所有方法所抛出的异常。但是无法处理当前控制器以外的异常。具体看SpringInAction 7.4节

* @ControllerAdvice
> 在这个控制器里声明的@ExceptionHandler可以处理所有控制器所抛出的异常。具体看SpringInAction 7.4节