# JAVA注解

注解APT(注解处理器)，可使用如下：

```dart
//注册一个APT功能
annotationProcessor 'com.google.auto.service:auto-service:1.0-rc4'
compileOnly 'com.google.auto.service:auto-service:1.0-rc4'
```

## 一、元注解

元注解是指能注解到注解上的注解，元注解有：@Retention、@Documented、@Target、@Inherited、@Repeatable。

### @Retention

Retention是保留期的意思，当@Retention注解使用在注解上时，可说明该注解的存活时间，它的取值可以有：

- RetentionPolicy.SOURCE：表示注解只在源码阶段保留，在编译器编译时会将其丢弃忽略。

- RetentionPolicy.CLASS：表示注解会被保留至编译时，不会被加载到JVM中。
- RetentionPolicy.RUNTIME：注解可以被保留至程序运行时，它会被加载至JVM中，所以在程序运行时可以获取到它们。

### @Documented

使用该注解可以将注解中的元素包含到Javadoc中。

### @Target

target是目标的意思，@Target指定了注解运用的地方，即限定注解的使用范围。该注解的取值有：

- ElementType.ANNOTATION_TYPE：表示可以使用该注解去注解注解
- ElementType.CONSTRUTOR：表示该注解可以使用在构造函数上
- ElementType.FIELD：表示该注解可使用在字段（属性）上
- ElementType.LOCAL_VARIABLE：可以对局部变量进行注解
- ElementType.METHOD：可以对方法进行注解
- ElementType.POCKAGE：对包进行注解
- ElementType.PARAMETER：对方法内的参数进行注解
- ElementType.TYPE：可以给一个类型进行注解，比如类、方法、枚举等。

### @Inherited

Inherited是继承的意思，并不是说注解本身可以继承，而是指被该注解进行注解的超类，其子类也将继承这个注解，尽管没有显示的写出。

例如：

```java
@Inherited
@Retention(RetentionPolicy.SOURCE)
@interface Test{}

@Test
class A{

}

class B extends A{
    
}
```

在上面的例子中，注解@Test被@Inherited注解，类A被@Test注解，则其子类B也被@Test注解。

### @Repeatable

Repeatable是可重复的意思，该注解的意思是可以使注解的值同时取多个。但@Repeatable是java1.8才引入的

例如：

```JAVA
@interface Persons {
    Person[] value();
}

@Repeatable(Persons.class)
@interface Person {
    String value() default "";
}

@Person(value = "aa")
@Person(value = "vv")
class SuperMan {

}
```



