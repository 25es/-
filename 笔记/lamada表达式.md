# Lamada表达式

Lambda表达式是Java 8引入的一种新特性，用于简化函数式编程的语法。它允许我们以更简洁的方式定义匿名函数。

作用：使用Lambda表达式可以对一个接口进行非常简洁的实现

注意：**，要求接口中定义的必须要实现的抽象方法只能是一个**

## 1. 基本用法

lamada lamada = ->(参数列表){代码具体实现}

接口

```java
package com.hbw.dao;

public interface lamada {
    /**
     * 测试lamada表达式
     * @param a
     * @param b
     * @return
     */
   void add(int a,int b);

}
```

测试：

```java
@Test
public void test() {
    lamada lamada = (a, b) -> {
        int result = a + b;
        System.out.println(result);

    };
    lamada.add(1,2);

}
```

## 2. 省略参数类型

参数类型可以省略

例如：

lamada lamada = ->(int a,int b){}

可以简化为

lamada lamada = ->(a,b){}

## 3. 省略参数括号

当接口方法中只有一个参数时，可以省略参数小括号

例如

```java
```

