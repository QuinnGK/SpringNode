#  getBean()方法解析

**AbstractBeanFactory.getBean()方法,所有的重载的方法都在其中调用了doGetBean()方法**   

**在Spring中真正做事的方法前缀都会有doXXX**   
*  **注为了方便说明调用方法在哪个类中统一都用className.methodName()的方式声明，这里不做成员方法与静态方法的区分**

### doGetBean又做了以下几件事

1. 调用了SimpleAliasRegistry.canonicalName()用来返回一个真实的BeanName。即Bean的ID；**但是值得注意的是AbstractBeanFactory在得到真实名称之前，会进行去除&符号的操作。例如getBean(&&&test),BeanName=test，并且当别名test->test1->test2->test3时，取得是test3。**

> ?? 疑问如果在<Bean>标签定义ID为“&&test”，但是在解析BeanDefinition的时候，是直接拉取的ID属性。然后直接将ID当key，BeanDefinition当value存入DefaultListableBeanFactory.beanDefinitionMap。如果这样的话getBean会不会有问题

2. 从单例缓存中拿取实，调用DefaultSingletonBeanRegistry.getSingleton();   
2.1  先从singletonObjects单例缓存中取
2.2  如果为null，并且"当前创建bean池"中存在(singletonsCurrentlyInCreation属性)。就是说这个beanName正在创建中   
2.3  加锁，为了防止其他线程往同时将ObjectFactory