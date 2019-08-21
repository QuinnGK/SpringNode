# BeanDefinition体系详解
---

* 类图如下
[BeanDefinition类图]()

---
再进入真正的主题之前先了解两个接口**BeanMetadataElement**，**AttributeAccessor**

* BeanMetadataElement
> 实现这个接口，会使Bean携带元数据。
* AttributeAccessor
> 接口定义了设置和访问对象属性的一般方式。

## BeanDefinition
> Javadoc中这样描述，BeanDefinition是用来描述一个Bean的实例。主要目的是为了允许BeanFactoryPostProcessor来修改其属性值或者metadata。
这里贴出子类AbstractBeanDefinition的源码
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


