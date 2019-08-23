# BeanPostProcessor详解

* BeanPostProcessor
* InstantiationAwareBeanPostProcessor
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