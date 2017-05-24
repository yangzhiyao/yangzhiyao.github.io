---
title: 如何将Form表单数据安全、方便的赋值给Entity
---

前两天遇到了一个问题，在Web应用中从表单中接收一个对象数据，这个对象有几十个属性，
直接把Jpa的Entity映射到前端Form里面倒不是不可以，但是保存的时候会有一些问题，
包括一些安全相关的、状态相关的数据会被表单传过来的数据覆盖掉，一方面是不安全，
另一方面也不符合业务流程要求。

这时候，我想了一个方式就是先把实体对象从前台获取回来，之后用现在数据库中的对象属性
覆盖掉一部分需要保护的数据。但是这个时候，如果忘记了某些属性的话，就会产生非常严重的后果。

一番搜索之后，找到了一个非常好的方案，就是使用BeanUtils.copyProperties()，其使用方法如下：

```java
import org.springframework.beans.BeanUtils;
BeanUtils.copyProperties(Object src,Object dest,String... ignoreProperties);
```

这样就不用写一大长串的getter,setter，不管多少个属性，一句话就搞定，轻松方便。

不过，还有一些需求，比如：

1、如果目标对象属性!=null时，不覆盖。
2、如果源对象属性=null时不覆盖。

对于这两种需求，解决方式如下（jdk8）：

### 需求1
```java
public static String[] getNullPropertyNames(Object source) {
    final BeanWrapper wrappedSource = new BeanWrapperImpl(source);
    return Stream.of(wrappedSource.getPropertyDescriptors())
            .map(FeatureDescriptor::getName)
            .filter(propertyName -> wrappedSource.getPropertyValue(propertyName) == null)
            .toArray(String[]::new);
}
public static void copyPropertiesSrcNotNull(Object src, Object target) {
    BeanUtils.copyProperties(src, target, getNullPropertyNames(src))
}
```

### 需求2
```java
public static String[] getNotNullPropertyNames(Object source) {
    final BeanWrapper wrappedSource = new BeanWrapperImpl(source);
    return Stream.of(wrappedSource.getPropertyDescriptors())
            .map(FeatureDescriptor::getName)
            .filter(propertyName -> wrappedSource.getPropertyValue(propertyName) != null)
            .toArray(String[]::new);
}
public static void copyPropertiesTargetIsNull(Object src, Object target) {
    BeanUtils.copyProperties(src, target, getNotNullPropertyNames(target))
}
```

其他更多一些的需求，可以参考例子，自由发挥。