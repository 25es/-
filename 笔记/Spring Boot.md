[TOC]



# Spring Boot

# Spring Boot相关介绍

## 1. Spring

官网：https://spring.io/

spring能做什么，来自官网的解释

![image-20230828105713021](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230828105713021.png)

## 1.1 Spring生态

https://spring.io/projects/spring-boot 

覆盖了:

 web开发 

数据访问 

安全控制 

分布式 

消息服务 

移动开发 

批处理

...........

## 1.2 Spring 5

#### 1.2.1 响应式编程

![image-20230828105948567](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230828105948567.png)

#### 1.2.2 内部源码设计

基于Java 8的一些新特性，如：**接口默认实现**。重新设计源码架构。

## 2. 为什么要使用Spring Boot

>Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
>
>
>
>能快速创建出生产级别的Spring应用

## 3. Spring Boot优点

>
>
>## Features
>
>- Create stand-alone Spring applications  创建独立的Spring应用
>- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)  **内嵌Tomcat**，jetty 等web服务器
>- Provide opinionated 'starter' dependencies to simplify your build configuration   **提供自动tarter依赖**，**简化构建配置**
>- Automatically configure Spring and 3rd party libraries whenever possible  自动配置Spring以及第三方功能
>- Provide production-ready features such as metrics, health checks, and externalized configuration  提供生产级别的监控、健康检查及外部化配置
>- Absolutely no code generation and no requirement for XML configuration   无代码生成，**无需编写xm**l

## 4. Spring Boot缺点

- 人称版本帝，迭代快，需要时刻关注变化
-   封装太深，内部原理复杂，不容易精通

微服务：

- 微服务是一种架构风格 
-  一个应用拆分为一组小型服务每个服务运行在自己的进程内，也就是可独立部署和升级
- 服务之间使用轻量级HTTP交互 • 
- 服务围绕业务功能拆分 • 
- 可以由全自动部署机制独立部署 • 
- 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术

**通过阅读官方文档来学习Spring  Boot**

# 01.第一个Spring Boot应用入门

## 1.1 创建Maven项目添加相关依赖

```xml
 <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.14</version>
    </parent>
 <groupId>com.hbw.boot</groupId>
    <artifactId>springboot</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

## 1.2 创建主程序类

```java
/**
 * 主程序类
 * 这是springboot程序入口
 */
@SpringBootApplication//标记这是一个springboot应用
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
```

以后启动Spring Boot项目直接运行该主配置类即可

## 1.3 编写业务

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String Hello(){
        return "Hello Spring boot 2 !";
    }

}
```

## 1.4 简化配置

Spring Boot相关配置以后在 [application.properties](..\..\..\ideahbw\springboot\src\main\resources\application.properties) 或 [application.yml](..\..\..\ideahbw\springboot\src\main\resources\application.yml) 中配置即可。例如服务端口号。

```properties
server.port=8080
```

## 1.5 简化部署

创建一个可执行jar包，需要添加以下依赖

To create an executable jar, we need to add the spring-boot-maven-plugin to our pom.xml. To do so, insert the following lines just below the dependencies section:

```xml
  <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

把项目打成jar包，直接在目标服务器执行即可。 

注意点： • 取消掉cmd的快速编辑模式



# 02. 了解自动配置原理

### 1 Spring Boot特点

#### 1.1 依赖管理

```xml
所有Spring Boot 项目父依赖 
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.14</version>
    </parent>
他的父依赖
 <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.7.14</version>
  </parent>
这个依赖帮我们管理了几乎所有常见的依赖的版本号，我们无需关心版本号，自动版本仲裁机制
```

以后使用Spring Boot时，需要用到哪个场景，就导入该场景的starter依赖即可。例如web开发时，就要导入spring-boot-starter-web

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

1、见到很多 spring - boot - starter - * : *就某种场景

 2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入 

3、SpringBoot所有支持的场景 https://docs.spring.io/spring - boot/docs/current/ reference/html/using - spring - bo 

4、见到的 * - spring - boot - starter： 第三方为我们提供的简化开发的场景启动器。

 5、所有场景启动器最底层的依赖

>- 自动版本仲裁机制，我们自己导入依赖时可以不用写版本号，如果引入非版本仲裁的依赖jar，就需要写版本号
>
>- **可以修改默认版本号**，比如修改mysql驱动的版本号
>
>  ```xml
>   <dependency>
>              <groupId>mysql</groupId>
>              <artifactId>mysql-connector-java</artifactId>
>              <version>5.1.6</version>
>          </dependency>
>  或者
>  〈properties〉
>  <mysql.version>5.1.43</mysql.version>
>  〈/properties〉
>  ```

### 2.1 自动配置

导入web starter后会自动导入很多依赖

```xml
<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.7.14</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.7.14</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.7.14</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.3.29</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.29</version>
      <scope>compile</scope>
    </dependency>
```

- 内嵌Tomcat服务器 在导入web  starter依赖后因为依赖传递性会自动引入spring-boot-starter-tomcat依赖，并配置好Tomcat
- 自动配好SpringMVC
  - 引入SpringMVC全套组件
  - 自动配置好SpringMVC常用组件

- •自动配好Web常见功能，如：字符编码问题 。
  - SpringBoot帮我们配置好了所有web开发的常见场景

-  默认扫描包结构 
  - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来 ◦ 
  - 无需以前的包扫描配置 
  - 想要改变扫描路径，@SpringBootApplication(scanBasePackages= ,, com.atguigu") ■ 或者@ComponentScan 指定扫描路径

>
>```java
>@SpringBootApplication注解是一个复合注解，由@SpringBootConfiguration
>@EnableAutoConfiguration
>@ComponentScan这三个注解组成
>```
>
>

名字带有AutoConfiguration 的类都是自动配置类，这些类都在spring-boot-autoconfigure-2.7.14.jar下的org包内（未完）

## 2. 容器功能

### 2.1 添加组件

1.**使用@Configuration和@Bean**

>```java
>@Configuration(proxyBeanMethods = false)
>public class MyConfig {
>    @ConditionalOnBean (name = "user")
>    @Bean
>    public User user(){
>        User user = new User("贺博文", "23", "男");
>       // user.setPet(pet());
>        return user;
>    }
>    @ConditionalOnBean (name = "user")
>    @Bean
>    public Pet pet(){
>       return new Pet("小明","狗","2");
>    }
>}
>
>```
>
>```java
>@Configuration注解标识一个类为配置类，配置类本身也是一个组件。
>   1.@Configuration注解有proxyBeanMethods属性，默认值为true，设置该配置类为代理配置类。且类中@Bean标注方法在外部无论调用多少次，都是从容器拿。保证了单例。
>   2. 若把该属性值设置为false，则每次调用类中方法得到的都是不同的对象。但在容器内该组件还是单例不会变。
>    类中使用@Bean标识方法，往容器中添加组件，添加组件顺序从上到下。使用此种方法添加的组件也是默认单例的。此种方法添加的组件id即名字是方法名，类型为方法的返回类型
>    User user2 = run.getBean("user", User.class);
>        User user1 = run.getBean("user", User.class);
>        System.out.println(user2==user1);
>运行结果为true
>```

 Ful膜式与Lite模式

- Full模式：proxyBeanMethods 属性值为true，此种情况称为Full模式。通常在配置类中组件有依赖关系得时候使用。比如人和宠物。人每次获得的宠物应为同一个宠物。

- Lite模式：proxyBeanMethods 属性值为false，此种情况称为Lite模式。此种模式通常在没有依赖关系时使用。

  ```java
   @Bean
      public User user(){
          User user = new User("贺博文", "23", "男");
         user.setPet(pet());//User依赖了Pet
          return user;
      }
  //    @ConditionalOnBean (name = "user")
      @Bean
      public Pet pet(){
         return new Pet("小明","狗","2");
      }
  }
  ```

  

2**.使用@Component、@Controller、@Service、@Repository**

3.@lmport

使用·@lmport注解会引入组件并把组件添加到容器内，默认组件名为全类名

```java
* 4、@Impont({User.class^
DBHelper.class})
2 * 给容器中自动创建出这两个类型的组件、默认组件的名字就是全类名
氺 3
氺 4
氺 5
6 */
7
@Import({User.class, DBHelper.class})
@Configuration(proxyBeanMethods = false)//告诉SpringBoot这 是 一 个 配 置 类 == 配 置
public class MyConfig {}
8
9
10
11

```

4.@Conditional注解

@Conditional注解：按条件装配，。满足指定条件，才把标识的组件添加到容器内

@Conditional注解可以标识在配置类或者@Bean方法上

标识在类上标识按条件注入该配置类

表示在方法上表示按条件注入该组件

![image-20230828192228478](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230828192228478.png)

注意：组件注入是有顺序的，默认在配置类中从上到下。

```java
 @ConditionalOnMissingBean(name = "user")
    @Bean
    public User user(){
        User user = new User("贺博文", "23", "男");
//       user.setPet(pet());//User依赖了Pet
        return user;
    }
    @ConditionalOnMissingBean(name = "user")
    @Bean
    public Pet pet(){
       return new Pet("小明","狗","2");
    }
}
上述user组件注入成功，pet注入失败
```

```java

@Configuration(proxyBeanMethods = false)
@ConditionalOnMissingBean(name = "user")
public class MyConfig {

    @Bean
    public User user(){
        User user = new User("贺博文", "23", "男");
//       user.setPet(pet());//User依赖了Pet
        return user;
    }
//    @ConditionalOnMissingBean(name = "user")
    @Bean
    public Pet pet(){
       return new Pet("小明","狗","2");
    }
}
上述user和pet组件都注入成功
```

### 2.2 @ImportResource引入原生配置文件

```xml
<bean id="pet" class="com.hbw.boot.pojo.Pet">

</bean>
    <bean id="user" class="com.hbw.boot.pojo.User">
<property name="name" value="雄安"></property>
<property name="gender" value="公"></property>
<property name="age" value="12"></property>
    </bean>
```



```java
@Configuration
@ImportResource(locations = "classpath:haha.xml")
public class Config {
}
```

```java
测试：
      boolean user = run.containsBean("user");
        System.out.println(user);
        boolean pet2 = run.containsBean("pet");
        System.out.println(pet2);
        User user1 = run.getBean(User.class);
        System.out.println(user1);
输出结果：true
true
User{name='雄安', age='12', gender='公', pet=null}
```

### 2.3 配置绑定

如何使用java读取到properties文件中的内容，并且把它封装至」I javaBean中，以供随时使用;

1.使用@ConfigurationProperties和@Component注解

只有添加到容器内才能有Spring Boot的强大功能

@ConfigurationProperties有属性prefix，设置值后就可以在Spring Boot配置文件中修改属性

```java
@ConfigurationProperties(prefix = "pet")
@Component
public class Pet {
    private String name;

    private String kind;

    private String  age;

    public Pet(String name, String kind, String age) {
        this.name = name;
        this.kind = kind;
        this.age = age;
    }

    public Pet() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getKind() {
        return kind;
    }

    public void setKind(String kind) {
        this.kind = kind;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Pet{" +
                "name='" + name + '\'' +
                ", kind='" + kind + '\'' +
                ", age='" + age + '\'' +
                '}';
    }
```

2.使用@EnableConfigurationProperties + @ConfigurationProperties

@EnableConfigurationProperties有两个功能：

- 开启配置绑定功能
- 把类注入到容器中

开启后，用@EnableConfigurationProperties标识的类就可以使用@ConfigurationProperties标识类中的属性了

```java
@EnableConfigurationProperties (Car.class)
//1、开启Car配置绑定功能
//2、把这个Car这个组件自动注册到容器中
public class MyConfig {}
```

### 2.4 自动配置

1. @SpringBootApplication由以下三个注解组成

```

@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
       @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

   1.1  @SpringBootConfiguration

这个注解就是一个配置类

 ```java
 @Configuration
 @Indexed
 public @interface SpringBootConfiguration {
 
 	
 	@AliasFor(annotation = Configuration.class)
 	boolean proxyBeanMethods() default true;
 
 }
 
 ```

1.2  @ComponentScan

这个注解就是扫描一些组件

**1.3  @EnableAutoConfiguration**（重点）

这个注解又由下面这两个注解组成

```java
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
```

 1.3.1  **@AutoConfigurationPackage（注册主程序包下所有组件到容器内）**

```java
@Import(AutoConfigurationPackages.Registrar.class)
```

这个注解导入.Registrar，并把.Registrar注入到容器内

```java
static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {

		@Override
		public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
			register(registry, new PackageImports(metadata).getPackageNames().toArray(new String[0]));
		}

		@Override
		public Set<Object> determineImports(AnnotationMetadata metadata) {
			return Collections.singleton(new PackageImports(metadata));
		}

	}
```

这个Registrar类的registerBeanDefinitions方法就是注册组件，debug分析

首先metadata属性是注解的源信息，也就是标注位置的信息，也就是主程序信息

![image-20230903153327144](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903153327144.png)

在这个方法中又调用register为容器注册组件，要注册的组件是new PackageImports(metadata).getPackageNames().toArray(new String[0])

计算一下，**计算结果是主程序所在的包，也就是说这个方法会把主程序所在的包中的类都注册到容器内。**，这就解释了为什么默认扫包是在主程序所在的包下

![image-20230903153616122](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903153616122.png)

1.3.2 @Import(AutoConfigurationImportSelector.class)

引入AutoConfigurationImportSelector，并把AutoConfigurationImportSelector注册到容器内

AutoConfigurationImportSelector类中有 getAutoConfigurationEntry方法

     ```java
     protected AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
     		if (!isEnabled(annotationMetadata)) {
     			return EMPTY_ENTRY;
     		}
     		AnnotationAttributes attributes = getAttributes(annotationMetadata);
     		List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);
     		configurations = removeDuplicates(configurations);
     		Set<String> exclusions = getExclusions(annotationMetadata, attributes);
     		checkExcludedClasses(configurations, exclusions);
     		configurations.removeAll(exclusions);
     		configurations = getConfigurationClassFilter().filter(configurations);
     		fireAutoConfigurationImportEvents(configurations, exclusions);
     		return new AutoConfigurationEntry(configurations, exclusions);
     	}
     ```

在未引入其他jar包时debug分析一下该方法

1.该方法会执行getCandidateConfigurations方法

![image-20230903154704748](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903154704748.png)

在未引入其他jar包时，跳过该方法，会发现configurations中有144个自动配置类，说明这个方法是帮我们引入自动配置类的

![image-20230903195541130](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903195541130.png)

2.进入到getCandidateConfigurations方法内，该方法会执行ImportCandidates.load方法

![image-20230903200142661](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903200142661.png)

3.进入到ImportCandidates.load方法内

![image-20230903200515010](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903200515010.png)

该方法内会从location中取出配置类，就是从下面文件取出配置类

![image-20230903200634398](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903200634398.png)

4.对urls进行遍历并依次取出每个元素，（**这个urls中的元素就是包含location文件的jar包**）然后在通过readCandidateConfigurations方法读出每个元素中的配置类，再添加到候选配置类中

![image-20230903200924411](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903200924411.png)

```java
while(urls.hasMoreElements()) {
            URL url = (URL)urls.nextElement();
            importCandidates.addAll(readCandidateConfigurations(url));
        }
```

由于没有引入其他jar包，所以while只循环一次，因为只有spring-boot-autoconfigure-2.7.14.jar包含上面图片的文件

此时configurations中就有144个自动配置类

5.回到 getAutoConfigurationEntry方法，该方法会执行removeDuplicates方法去除一些自动配置类，再执行getExclusions获取自动义排除自动配置类的信息，然后再执行removeALL方法把取自动义排除自动配置类去除，由于我们没有配置排除项，所以Configurations还有144个

![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903202646523.png)

![image-20230903202852134](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903202852134.png)

按需加载

6.自动加载的自动配置类只有@Condictional中条件满足才会添加到容器内，其他不满足条件的则不添加，调用configurations = this.getConfigurationClassFilter().filter(configurations);方法即可过滤掉条件不成立的自动配置类，执行完后configurations 只剩25个自动配置类

![image-20230903203700325](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903203700325.png)

**![image-20230903203750737](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903203750737.png)**

在引入其他jar包是debug分析getAutoConfigurationEntry方法

大多和之前没引入其他jar包一样，只不过在遍历urls时不同。



# 03. 配置文件

## 1.properties

> 同以前的properties，使用a=b形式

## 2.yaml(yml)（建议使用这个）

**YAML** 是 "YAML Ain't Markup LanguageM (YAML 不是一种标记语言）的**递归缩写**。在开发 的这种语言时，YAML 的意思其实是："Yet Another Markup Language" (仍是一种**标记语 言**）。 非常适合用来做以数据为中心的配置文件

### 2.1使用说明：

- 配置使用键值对形式 key：value，**注：kv之间有空格**
- **使用缩进来表示层级关系**
- **大小写敏感**
- 使用"#"来写注释
- 不能使用tab，只能使用空格
- 空几格不重要，只要同层级的左对齐就行
-  **字符串无需加引号**，如果要加，"与" M表示字符串内容会被转义/不转义

### 2.2 数据类型使用：

1.字面量：单个的，不可再分的值  int，double，date、boolean、string、number、null等

直接使用k：v即可

2.对象： 键值对的集合  map、Object、 hash、set

```yaml
行内写法： k: {kl:vl,k2:v2,k3:v3}
或
k:
 kl: vl
 k2: v2
 k3: v3
```

3.数组：array、list、queue

```yaml
行内写法： k: [vl,v2,v3]
#或者
k:
 - vl
 - v2
 - v3
```

## 2.3 示例

```java
@Data
 public class Person {

private String userName;
private Boolean boss;
private Date birth;
private Integer age;
private Pet pet;
private String[] interests;
private List<String> animal;
private Map<String,Object〉score;
private Set<Double> salarys;
private Map<StringJ List<Pet>> allPets;
 }
@Data
public class Pet {
private String name;
private Double weight;

}
```

![image-20230828211754803](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230828211754803.png)

## 3. 配置提示

自定义的类与配置文件绑定一般没有提示

加入以下依赖即可

```xml
〈dependency〉
<groupId>org.springframework.boot</groupId >
<artifactId>spring-boot-configuration-processors/artifactId>
<optional>true</optional〉
〈/dependency〉
 < build>
<plugins>
< plugin>

<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
〈configuration〉
<excludes>

<exclude>
<groupId >org.springframework.boot</gnoupId>
<artifactId >spring- boot-configuration-processors/a
</exclude>

</excludes>
〈/configuration〉

</plugin>
</plugins>
</build
```

# 04 web开发

![image-20230829101830674](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230829101830674.png)

## 1. SpringMVC自动配置概览（未完）

Spring Boot provides auto-configuration for Spring MVC that **works** **well** **with** **most**

**applications**•

(大多场景我们都无需自定义配置）



## 2. 静态资源

### 2. 访问静态资源

静态资源可以放在类路径下static(推荐)、called (or /public or /resources or /META - INF/resource

直接可以使用工程路径/+静态资源即可访问

可以修改静态资源前缀（为避免控制器方法处理请求路径和访问静态资源名请求一样，导致请求被控制器方法处理访问不了静态资源），加上前缀后，访问静态资源也得加上前缀

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
  resources:
    static-locations: [classpath:/he/]  ：修改静态资源位置，不建议修改，直接放在类路径下的static即可
```

>注：
>
>请求首先会被DispatherServlet拦截，看有没有控制器方法能够处理请求，如果没有则会交给默认的Servlet处理访问静态资源，如果默认servlet也找不到会报404

### 3. 欢迎页 index.html（未完）

index.html 放在静态资源路径下即可

设置了静态资源访问前缀会导致欢迎页不可用(看源码),设置静态资源路径不会影响欢迎页

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**
#  web:
#    resources:
#      static-locations: [classpath:/he/]
```

### 4.  [favicon.ico](..\..\..\ideahbw\springboot3\src\main\resources\static\favicon.ico) 使用

 [favicon.ico](..\..\..\ideahbw\springboot3\src\main\resources\static\favicon.ico) 即浏览器标签页前面得得小图片， [favicon.ico](..\..\..\ideahbw\springboot3\src\main\resources\static\favicon.ico) 放在静态资源路径下即可

![image-20230829112631960](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230829112631960.png)

和欢迎页一样设置了静态资源访问前缀会导致不可用

### 5. 静态资源配置原理

1.关于静态资源的自动配置在WebMvcAutoConfiguration这个自动配置类中

2.WebMvcAutoConfiguration内部有**WebMvcAutoConfigurationAdapter**这个配置类，这个类是自动配置静态资源相关的类

3.WebMvcAutoConfigurationAdapter绑定了两个配置文件，可以直接使用这两个配置文件中的属性。

![image-20230919153535606](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919153535606.png)

WebMvcAutoConfigurationAdapter有一个有参构造器，有参构造器的参数都从容器中拿

```java
public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
				ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
				ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
				ObjectProvider<DispatcherServletPath> dispatcherServletPath,
				ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
			this.resourceProperties = webProperties.getResources();
			this.mvcProperties = mvcProperties;
			this.beanFactory = beanFactory;
			this.messageConvertersProvider = messageConvertersProvider;
			this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
			this.dispatcherServletPath = dispatcherServletPath;
			this.servletRegistrations = servletRegistrations;
			this.mvcProperties.checkConfiguration();
		}
```

4.WebMvcAutoConfigurationAdapter配置类中为容器添加了viewResolver这个组件

![image-20230919154130703](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919154130703.png)

5.viewResolver中有个addResourceHandlers方法，此方法内部定义了静态资源访问规则。

![image-20230919154310496](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919154310496.png)

6.addResourceHandlers方法其中调用了addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration)方法。

7.进入到addResourceHandler中，发现此方法有三个参数，分别是registry，pattern是浏览器访问路径，customizer是服务器静态资源。

![image-20230919154612628](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919154612628.png)

8.addResourceHandler内部又调用了registry.addResourceHandler(pattern)方法，registry.addResourceHandler(pattern)方法是为pattern创建一个控制器。

9.然后又执行了customizer.accept(registration);，**让服务器资源接受了这个控制器，也就是说，当被此控制器拦截的请求会访问服务器资源。**

10.![image-20230919155151257](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919155151257.png)

进入到this.mvcProperties.getStaticPathPattern()中，在进入到this.resourceProperties.getStaticLocations()中

![image-20230919155240151](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919155240151.png)

![image-20230919155348355](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919155348355.png)

也就是说这两行代码为/**请求创建了一个控制器并访问服务器路径为

```
classpath:/META-INF/resources/",
       "classpath:/resources/", "classpath:/static/", "classpath:/public/" 
```

的资源。**这就解释了为什么静态资源放在类路径下static(推荐)、called (or /public or /resources or /META - INF/resource**

**直接可以使用工程路径/+静态资源即可访问**



## 3. 请求参数处理

此部分内容大部分在ssm笔记SpringMVC部分

- 使用Rest风格时，发送put、delete请求需要的请求方式过滤器HiddenHttpMethodFilter在Spring Boot中需要自己手动开启

  ```yaml
  spring:
    mvc:
      hiddenmethod:
        filter:
          enabled: true 
  #        开启表单方法过滤器
  ```

  原因：![image-20230919165738645](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919165738645.png)

使用方法：表单方法必须为post，表单中必须包含一个隐藏域hidden，name属性必须为_method,value值为真正的请求方式。

SpringBoot提供的OrderedHiddenHttpMethodFilter如何把post请求变成put或delete请求呢。

下面是debug分析源码

1.OrderedHiddenHttpMethodFilter继承了HiddenHttpMethodFilter

![image-20230919195035033](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919195035033.png)

2.HiddenHttpMethodFilter中有doFilterInternal方法，当请求进来时会被doFilterInternal方法拦截

```java
public class HiddenHttpMethodFilter extends OncePerRequestFilter {

	private static final List<String> ALLOWED_METHODS =
			Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),
					HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));

	/** Default method parameter: {@code _method}. */
	public static final String DEFAULT_METHOD_PARAM = "_method";

	private String methodParam = DEFAULT_METHOD_PARAM;


	/**
	 * Set the parameter name to look for HTTP methods.
	 * @see #DEFAULT_METHOD_PARAM
	 */
	public void setMethodParam(String methodParam) {
		Assert.hasText(methodParam, "'methodParam' must not be empty");
		this.methodParam = methodParam;
	}

@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		HttpServletRequest requestToUse = request;

		if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {//先判断表单的请求方式是不是post，再判断是否有错误。
			String paramValue = request.getParameter(this.methodParam);//获取请求中名字为methodParam即_method的参数值
			if (StringUtils.hasLength(paramValue)) {
				String method = paramValue.toUpperCase(Locale.ENGLISH);//把_method值变成大写
				if (ALLOWED_METHODS.contains(method)) {//ALLOWED_METHODS为PUT,DELETE,PATCH，如果method值属于三者之一，则执行if语句内内容
					requestToUse = new HttpMethodRequestWrapper(request, method);//创建一个HttpMethodRequestWrapper对象，使对象中method属性等于当前的method值
				}
			}
		}

		filterChain.doFilter(requestToUse, response);//带着requestToUse放行，以后在DisPatcherServlet中执行String method = request.getMethod()时调用的是requestToUse中的getMethod()方法。
	}
```

请求映射(关于HandlerMapping)源码：

1.请求进来先执行![image-20230919201320810](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919201320810.png)方法来获取能够处理该请求的Handler

2.进入该方法内，该方法首先遍历handlerMappings，（所有的请求映射都在handlerMapping中。）

![image-20230919201013539](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919201013539.png)

3.然后挨个判断每个HandlerMapping中是否有包含该请求信息的Handler。

![image-20230919202100839](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230919202100839.png)

如果有，则return找到的Handler。如果没有返回null

我们需要一些自定义的映射处理，我们也可以自己给容器中放**HandlerMapping**。自定义

**HandlerMapping**

Rest使用客户端工具， • 如PostMan直接发送Put、 delete等方式请求，无需Filter。

### 请求参数和基本注解

此部分也在SSM笔记里，补充几个注解

@ModelAttribute:  1. 用在入参：把ModelAttribute注解标识在参数上，为请求参数和控制器方法形参建立映射，并且会自动将形参注入到ModelMap中，供view使用

2. 标注在方法上

被标注了@ModelAttribute:注解的方法，会在本controller中其他方法执行之前都会执行一遍，如果有返回值，则自动把返回值放到ModelMap中，因此如果有一个方法映射多个url时要谨慎使用。

使用@ModelAttribute注解的方法和被@RequestMapping注解的处理方法由很多相似之处:

都可以通过入参接收前台提交的数据，而且对入参绑定的设置都是一样的。
入参绑定的数据如果没有设置可为空，不能接收空数据,否则会报错。
都可以将数据放入[model](https://so.csdn.net/so/search?q=model&spm=1001.2101.3001.7020)中，而且对于一次请求,model是共享的,所以在处理方法中的model中存放了@ModelAttribute注解的方法中存放的数据。

示例一（重点）

```java
@ModelAttribute("user")
public User getUser(Model model){
 
    User user = new User("Tom","123456");
    return user;
}
 
@RequestMapping("/testModelAttribute")
public String testModelAttribute(@ModelAttribute("user")User user1,Model model){
    System.out.println("testModelAttribute User:"+user1);
    System.out.println("testModelAttribute model:"+model.toString());
    return "success";
}
```

```htm
<form action="testModelAttribute" method="post">
    用户名:<input type="text" name="userName" value="Jack">
    <button type="submit">submit</button>
</form>
```

在处理方法的入参中使用了@ModelAttribute(“user”)User user1
执行流程
1.springmvc 解析本次请求共享的model（BindingAwareModelMap），
由于先被getUser方法处理过，model里面保存了
user=User[userName=Tom, password=123456]的数据。
2.从model中取出key为user的User对象，并把表单的请求参数赋给这个User对象的对应属性。因为这里前台只传递了 一个参数userName = Jack,所以经过处理这时候的model数据为user=User[userName=Jack, password=123456]userName被覆盖。
3.SpringMVC 把上述对象传入目标的参数user1中。

注意:也可以使用@ModelAttribute修饰,这样会默认使用入参类型第一个字母小写去model中匹配

示例二 标注在方法上，并且方法也使用了@RequestMapping注解

```java
@RequestMapping(value = "/test1")
@ModelAttribute("name1")
public String test1(@RequestParam(required = false) String name) {
   return name;
}
```

这种情况下，返回值 String （或者其他对象），就不再是视图了。还是我们上面讲到的放入 Model 中的值，对应的key为注解指定的name1，即key(name1):value(name)。此时对应的页面就是 @RequestMapping 的值 test1，交给页面解析后就是test1.jsp

### 复杂参数

Map、Model (map、model里面的数据会被放在request的请求域 request.setAttribute) 、 Errors/BindingResultN **RedirectAttributes ( 重定向携带数据）**、ServletResponse (response) 、SessionStatus, UriComponentsBuilder、ServletUriComponentsBuilder

### pojo封装

ssm笔记

### 参数处理·原理

