## package-info.java详解

参考如下博客https://strong-life-126-com.iteye.com/blog/806246

在spring的包中随处可见的就是package-info.java这个类。但是点进去之后如下
```java
/**
 * Support package for annotation-driven bean configuration.
 */
@NonNullApi
@NonNullFields
package org.springframework.beans.factory.annotation;

import org.springframework.lang.NonNullApi;
import org.springframework.lang.NonNullFields;
```
这个类一般就这三个作用
## 1、为标注在包上Annotation提供便利；
>例如
@NonNullApi
@NonNullFields
这两个注解就是@Target(ElementType.PACKAGE)这个级别的

## 2、声明友好类和包常量；　
>在这个类中可以定义本包中使用的类。
```java
package cn.infisa.base;

/**
 * CreateTime: 2019-08-04 00:44
 * User: quinn
 * Desc: 以下得类只能在本包中使用
 */
class Test{
    public void test(){
    }
}
```
补充：关于类的访问标志详细的看JLS吧。[Class Modifiers](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.1)

## 3、提供包的整体注释说明。
>在JavaDoc中会显示包的注释
