# BeanPostProcessor详解

* BeanPostProcessor
> 提供在创建实例前后可自定义修改实例的hook-method。
```java
public interface BeanPostProcessor {
	@Nullable
    /**
    在初始化之前对实例进行操作。比如在(InitializingBean.afterPropertiesSet)之前。
    这里在说明以下InitializingBean.afterPropertiesSet(),在init-method方法之前执行。
    对应的DisposableBean.destroy在destroy-method方法之前执行。详细见getBean()。
    */
	default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}
    /**
    在初始化方法之后对实例进行操作。
    原注释提到的的短路方法在AbstractAutowireCapableBeanFactory.resolveBeforeInstantiation()方法之后。
    短路判断如下
    if (bean != null) {
				return bean;
			}
    */
	default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		return bean;
	}
}
```

* InstantiationAwareBeanPostProcessor
> 实例化Bean后处理器

* SmartInstantiationAwareBeanPostProcessor
* DestructionAwareBeanPostProcessor
* MergedBeanDefinitionPostProcessor

---
## BeanPostProcessor
* BeanValidationPostProcessor
### Aware相关的后处理器
* ApplicationContextAwareProcessor
* ServletContextAwareProcessor
* BootstrapContextAwareProcessor
* LoadTimeWeaverAwareProcessor
### Advisor相关
* AbstractAdvisingBeanPostProcessor
* AbstractBeanFactoryAwareAdvisingPostProcessor
* AsyncAnnotationBeanPostProcessor
* MethodValidationPostProcessor
* PersistenceExceptionTranslationPostProcessor
---

---
## SmartInstantiationAwareBeanPostProcessor 
* InstantiationAwareBeanPostProcessorAdapter
* ScriptFactoryPostProcessor
* ImportAwareBeanPostProcessor

### 以下为AOP涉及到的
* AbstractAutoProxyCreator
* AbstractAdvisorAutoProxyCreator
* AspectJAwareAdvisorAutoProxyCreator
* AnnotationAwareAspectJAutoProxyCreator
* DefaultAdvisorAutoProxyCreator
* InfrastructureAdvisorAutoProxyCreator
* BeanNameAutoProxyCreator
---

---
## MergedBeanDefinitionPostProcessor
* JmsListenerAnnotationBeanPostProcessor
--- 

---
## DestructionAwareBeanPostProcessor
* SimpleServletPostProcessor
---

--- 
## SmartInstantiationAwareBeanPostProcessor || MergedBeanDefinitionPostProcessor
* AutowiredAnnotationBeanPostProcessor
* RequiredAnnotationBeanPostProcessor
---

---
## DestructionAwareBeanPostProcessor || MergedBeanDefinitionPostProcessor
* ApplicationListenerDetector
* InitDestroyAnnotationBeanPostProcessor
* ScheduledAnnotationBeanPostProcessor
---

---
## InstantiationAwareBeanPostProcessor || DestructionAwareBeanPostProcessor || MergedBeanDefinitionPostProcessor
* CommonAnnotationBeanPostProcessor
* PersistenceAnnotationBeanPostProcessor
---