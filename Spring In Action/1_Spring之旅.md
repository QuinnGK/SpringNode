
* 依赖注入(dependency injection DI)
* 面向切面编程(aspect-oriented programming AOP)

# Spring的目的是为了简化开发
* 基于POJO的轻量级和最小侵入性编程
* 通过依赖注入和面向接口实现松耦合
* 基于切面和惯例进行声明式编程
* 通过切面和模版减少样板式代码

# Spring容器
* BeanFactory
    * XmlBeanFactory
    * DefaultListableBeanFactory
    * AbstractAutowireCapableBeanFactory  

* ApplicationContext
    * AnnotationConfigApplicationContext：从一个或多个基于Java的配置类中加载Spring应用上下文。
    * AnnotationConfigWebApplicationContext：从一个或多个基于Java的配置类中加载Spring Web应用上下文。
    * ClassPathXmlApplicationContext：从类路径下的一个或多个XML配置文件中加载上下文定义，把应用上下文的定义文件作为类资源。
    * FileSystemXmlapplicationcontext：从文件系统下的一个或多个XML配置文件中加载上下文定义。
    * XmlWebApplicationContext：从Web应用下的一个或多个XML配置文件中加载上下文定义。

```java
//Xml配置
ApplicationContext context = new FileSystemXmlApplicationContext("c:/knight.xml");

//JavaConfig
ApplicationContext context = new AnnotationConfigApplicationContext(com.springinaction.knights.config.KnightConfig.class)
```
* SpringBean的生命周期
![SpringBean的生命周期](https://raw.githubusercontent.com/QuinnGK/SpringNode/master/Spring%20In%20Action/image/SpringBean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.jpeg)

BeanFactory的Doc中这样说

>Bean factory implementations should support the standard bean lifecycle interfaces as far as possible. The full set of initialization methods and their standard order is:
1. BeanNameAware's setBeanName
2. BeanClassLoaderAware's setBeanClassLoader
3. BeanFactoryAware's setBeanFactory
4. EnvironmentAware's setEnvironment
5. EmbeddedValueResolverAware's setEmbeddedValueResolver
6. ResourceLoaderAware's setResourceLoader (only applicable when running in an application context)
7. ApplicationEventPublisherAware's setApplicationEventPublisher (only applicable when running in an application context)
8. MessageSourceAware's setMessageSource (only applicable when running in an application context)
9. ApplicationContextAware's setApplicationContext (only applicable when running in an application context)
10. ServletContextAware's setServletContext (only applicable when running in a web application context)
11. postProcessBeforeInitialization methods of BeanPostProcessors
12. InitializingBean's afterPropertiesSet
13. a custom init-method definition
14. postProcessAfterInitialization methods of BeanPostProcessors
On shutdown of a bean factory, the following lifecycle methods apply:
15. postProcessBeforeDestruction methods of DestructionAwareBeanPostProcessors
16. DisposableBean's destroy
17. a custom destroy-method definition

实际上getBean原比上图中的步骤复杂的多，上图也只是说了一个大概流程，详细请看[Srping.getBean()方法解析](https://github.com/QuinnGK/SpringNode/blob/master/%E4%B8%AA%E4%BA%BA%E7%90%86%E8%A7%A3/getBean()%E6%96%B9%E6%B3%95%E8%A7%A3%E6%9E%90.md)

