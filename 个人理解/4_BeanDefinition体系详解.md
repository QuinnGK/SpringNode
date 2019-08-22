# BeanDefinition体系详解
---

* 类图如下
![BeanDefinition类图](https://raw.githubusercontent.com/QuinnGK/SpringNode/master/%E4%B8%AA%E4%BA%BA%E7%90%86%E8%A7%A3/image/BeanDefinition_UML.png)

---
再进入真正的主题之前先了解两个接口**BeanMetadataElement**，**AttributeAccessor**

* BeanMetadataElement
> 接口定义了提供访问对象元数据的方法。
* AttributeAccessor
> 接口定义了设置和访问对象属性的方法。

## BeanDefinition
> Javadoc中这样描述，BeanDefinition是用来描述一个Bean的实例。主要目的是为了允许BeanFactoryPostProcessor来修改其属性值或者metadata。
这里贴出**AbstractBeanDefinition**的源码
```java
public abstract class AbstractBeanDefinition extends BeanMetadataAttributeAccessor
		implements BeanDefinition, Cloneable {
    //Bean的Class
	private volatile Object beanClass;
    //Bean的作用范围
	private String scope = SCOPE_DEFAULT;
    //是否是抽象
	private boolean abstractFlag = false;
    //是否延迟加载
	private Boolean lazyInit;
    //注入模式默认不定义，详细见AutowireCapableBeanFactory
	private int autowireMode = AUTOWIRE_NO;
	//用来表示一个Bean的实例化依靠另一个Bean优先实例化。
	private String[] dependsOn;
    //将此属性设置为false,则在容器自动装配时则不考虑这个Bean,就是说其他Bean在自动装配时，此Bean不会作为候选者
	private boolean autowireCandidate = true;
    //自动装配时，出现多个Bean候选者，则作为首选者。
	private boolean primary = false;
    //用于记录qualifiers
	private final Map<String, AutowireCandidateQualifier> qualifiers = new LinkedHashMap<>();
    //通过一个Supplier接口来实现工厂方法，默认优先级在工厂方法实例化之前。详细见AbstractAutowireCapableBeanFactory.createBeanInstance()。spring5新加的。
	private Supplier<?> instanceSupplier;
    //是否允许访问非公开的构造器和方法
	private boolean nonPublicAccessAllowed = true;
    //是否以宽松的模式解析构造函数
	private boolean lenientConstructorResolution = true;
    //对应的factory-bean
	private String factoryBeanName;
    //对应的factory-method
	private String factoryMethodName;
    //记录构造函数注入的属性
	private ConstructorArgumentValues constructorArgumentValues;
    //属性集合
	private MutablePropertyValues propertyValues;
    //方法重写，记录lookup-method与replaced-method
	private MethodOverrides methodOverrides;
    //初始化方法 init-method
	private String initMethodName;
    //销毁方法 destory-method
	private String destroyMethodName;
    //是否执行init-method
	private boolean enforceInitMethod = true;
    //是否执行destory-method
	private boolean enforceDestroyMethod = true;
    //是否是有程序生成的
	private boolean synthetic = false;
    //定义这个bean的应用
	private int role = BeanDefinition.ROLE_APPLICATION;
    //Bean的描述信息
	private String description;
    //定义这个Bean的资源
	private Resource resource;
}
```

* AttributeAccessorSupport
> 简单的实现了AttributeAccessor，提供了属性操作的最基本的方式。

* BeanMetadataAttributeAccessor
> 扩展AttributeAccessorSupport接口，用来保存BeanMetadataAttribute对象。以便跟踪定义源。

* AbstractBeanDefinition 
> 实现了BeanDefinition所定义的方法。并且继承了BeanMetadataAttributeAccessor。

* GenericBeanDefinition
> 标准的BeanDefinition的实现。可以通过parentName指定父级BeanDefinition，GenericBeanDefinition的优点是它允许动态定义父依赖关系，而不是将角色“硬编码”为根bean定义。**一般在BeanDefinitionReaderUtils.createBeanDefinition()方法创建一个GenericBeanDefinition。**

* RootBeanDefinition
> 同GenericBeanDefinition的区别在于，RootBeanDefinition将父子依赖关系通过硬编码的方式来定义。所以在Spring2.5开始，注册RootBeanDefinition首选用GenericBeanDefinition，但是在getBean时，还是会将GenericBeanDefinition转为RootBeanDefinition。在RootBeanDefinition又新添加了一些属性。如下
```java
	//用来装饰BeanDefinition
	private BeanDefinitionHolder decoratedDefinition;
	//在解析注解时用的到，暂时不晓得用途
	private AnnotatedElement qualifiedElement;
	//是否允许缓存
	boolean allowCaching = true;
	//工厂方法是否唯一
	boolean isFactoryMethodUnique = false;
	//BeanDefinition的类型用ResolvableType封装
	volatile ResolvableType targetType;
	//BeanDefinition的类型
	volatile Class<?> resolvedTargetType;
	//工厂方法返回的类型。用ResolvableType封装
	volatile ResolvableType factoryMethodReturnType;
	//以下四个变量的锁
	final Object constructorArgumentLock = new Object();
	//缓存已经解析的构造函数或是工厂方法
	Executable resolvedConstructorOrFactoryMethod;
	//表明构造函数参数是否解析完毕
	boolean constructorArgumentsResolved = false;
	//缓存完全解析的构造函数参数
	Object[] resolvedConstructorArguments;
	//缓存待解析的构造函数参数
	Object[] preparedConstructorArguments;
	//以下两个变量的锁
	final Object postProcessingLock = new Object();
	//表明是否被MergedBeanDefinitionPostProcessor处理过
	boolean postProcessed = false;
	//表面是否被BeanPostProcessors处理过
	volatile Boolean beforeInstantiationResolved;
	@Nullable
	//不明
	private Set<Member> externallyManagedConfigMembers;
	//InitializingBean中的init回调函数名——afterPropertiesSet会在这里记录，以便进行生命周期回调
	private Set<String> externallyManagedInitMethods;
	//DisposableBean的destroy回调函数名——destroy会在这里记录，以便进行生命周期回调
	private Set<String> externallyManagedDestroyMethods;
```

* ChildBeanDefinition
> 与RootBeanDefinition对应。子级BeanDefinition用这个表示。

---
* AnnotatedBeanDefinition
> 

* AnnotatedGenericBeanDefinition
> 扩展GenericBeanDefinition类，并提供对AnnotatedBeanDefinition的支持。

* ScannedGenericBeanDefinition
> 以上两者还需要去搞清楚他们的不同

* ConfigurationClassBeanDefinition
> 

## 以上用注解的方式来定义Bean的需要在研究一下
---

* BeanDefinitionHolder
> 拥有BeanName和别名和BeanDefinition的持有者，将BeanDefinition做了层封装

* BeanComponentDefinition
> BeanDefinitionHolder的子类，在ReaderEventListener中会注册这个，但没懂式是用来做什么的

* DefaultsDefinition
> 用于默认定义的标记接口

* DocumentDefaultsDefinition
> DefaultsDefinition的实现。在BeanDefinitionParserDelegate被使用，用来定义默认的一些Bean信息。
