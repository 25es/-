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
public interface lamada2 {
    /**
     * 测试lamada表达式
     * @param a
     * @param b
     * @return
     */
    int reduce(int a, int b); 
}
```

test

```java
@Test
public void test1(){
    lamada2 lamada2=(a) -> {
        System.out.println(a);
    };


}
```

可简化成

```java
@Test
public void test1(){
    lamada2 lamada2=a -> {
        System.out.println(a);
    };


}
```

## 4. 省略方法体大括号

当方法体**内部只有一个表达式**或者一个输出语句时，大括号可以省略，而且这个表达式会**自动解析为return语句**

例：

```java
@Test
public void test() {
    lamada lamada = (a, b) -> {
        return a+b;

    };
    lamada.add(1,2);

}
```

可以简化为

```java
@Test
public void test() {
    lamada lamada = (a, b) -> a + b;
    lamada.add(1, 2);

}
```

# lamada语法进阶

## 1.方法引用(普通方法与静态方法)

在实际应用过程中，一个接口在很多地方都会调用同一个实现，例如：

LambdaSingleReturnMutipleParmeter lambda1=(a,b)->a+b;
LambdaSingleReturnMutipleParmeter lambda2=(a,b)->a+b;
这样一来每次都要写上具体的实现方法 a+b，如果需求变更，则每一处实现都需要更改，基于这种情况，可以将后续的是实现更改为已定义的 方法，需要时直接调用就行
语法：

方法引用可以在方法体中调用另一个方法

用法：

方法的隶属者如果是静态方法，隶属的就是一个类，其他的话就是隶属对象

语法：方法的隶属者::方法名

注意：

1. 引用的方法中，参数数量和类型一定要和接口中定义的方法一致
2. 返回值的类型也一定要和接口中的方法一致

public class Syntax3 {

```java
public static void main(String[] args) {
    
    LambdaSingleReturnSingleParmeter lambda1=a->a*2;
    LambdaSingleReturnSingleParmeter lambda2=a->a*2;
    LambdaSingleReturnSingleParmeter lambda3=a->a*2;

    //简化
    LambdaSingleReturnSingleParmeter lambda4=a->change(a);

    //方法引用
    LambdaSingleReturnSingleParmeter lambda5=Syntax3::change;
}
```

 

```java
//自定义的实现方法
private static int change(int a){
    return a*2;
}
```
2.方法引用(构造方法)
目前有一个实体类



```java
public class Person {
    public String name;
    public int age;
public Person() {
    System.out.println("Person的无参构造方法执行");
}

public Person(String name, int age) {
    this.name = name;
    this.age = age;
    System.out.println("Person的有参构造方法执行");
}
```
}
两个接口，各有一个方法，一个接口的方法需要引用Person的无参构造，一个接口的方法需要引用Person的有参构造 用于返回两个Person对象，例：

```java
interface PersonCreater{
    //通过Person的无参构造实现
    Person getPerson();
}

interface PersonCreater2{
    //通过Person的有参构造实现
    Person getPerson(String name,int age);
}
```


那么可以写作：



```java
    public class Syntax4 {
    public static void main(String[] args) {
    PersonCreater creater=()->new Person();

    //引用的是Person的无参构造
     //PersonCreater接口的方法指向的是Person的方法
    PersonCreater creater1=Person::new; //等价于上面的()->new Person()
    //实际调用的是Person的无参构造 相当于把接口里的getPerson()重写成new Person()。
    Person a=creater1.getPerson(); 

    //引用的是Person的有参构造
    PersonCreater2 creater2=Person::new;
    Person b=creater2.getPerson("张三",18);
}
```
}
注意：是引用无参构造还是引用有参构造 在于接口定义的方法参数

