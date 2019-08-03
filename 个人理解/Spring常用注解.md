## Spring常用注解整理

---
#### java.lang.annotation
* @Target
> 定义了Annotation所修饰的范围，一般分为以下几类：  
ANNOTATION_TYPE ,  
CONSTRUCTOR ,  
FIELD ,  
LOCAL_VARIABLE ,  
METHOD , PACKAGE ,  
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
---
#### org.springframework.core

* @AliasFor
>

* @Order
>

* @NonNull
>

* @NonNullApi
>

* @NonNullFields
>

* @Nullable
>

---
#### org.springframework.beans

* @Autowired
>

* @Configurable
>

* @Lookup
>

* @Qualifier
>

* @Value
>
