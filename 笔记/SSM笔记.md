[TOC]

#  一. MyBatis

## 1.MyBatis 环境

```
MySQL5 版本驱动类使用com.mysql.jdbc.Driver
链接地址URL使用jdbc:mysql://localhost:3306/ssm
```

## 2.使用MyBatis

### 2.1. 创建Maven工程并导入依赖(pom.xml中添加)

```
<dependencies>
<!-- Mybatis核心 -->
<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis</artifactId>
<version>3.5.7</version>
</dependency>
<!-- junit测试 -->
<dependency>
<groupId>junit</groupId>

<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
<!-- MySQL驱动 -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.16</version>
</dependency>
</dependencies>
```

### 2.2. 创建MyBatis核心配置文件mybatis-config（可以在Idea中设置一个快捷模板在setting中File and Code Templates中Files下点击加号添加）

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--设置连接数据库的环境-->
<environments default="development">
<environment id="development">
<transactionManager type="JDBC"/>
<dataSource type="POOLED">
<property name="driver" value="com.mysql.cj.jdbc.Driver"/>
<property name="url" value="jdbc:mysql://localhost:3306/ssm?
serverTimezone=UTC"/>
<property name="username" value="root"/>
<property name="password" value="123456"/>
</dataSource>
</environment>
</environments>
<!--引入映射文件-->
<mappers>
<package name="mappers/UserMapper.xml"/> 创建模板时name中要空着
</mappers>
</configuration>
```

### 2.3 创建mapper接口（相当于dao层）里面只定义方法

```
public interface UserMapper {
/**
* 添加用户信息
*/
int insertUser();
}
```



### 2.4 在resources下创建mapper接口对应的mapper.xml文件

注意mapper.xml文件所在的包名要与mapper接口所在的包名要完全一致，因为maven工程中java包和resources包在编译后在同一个目录下，他们俩包名一致，核心配置文件中引入mapper时可以直接引入包名。引入mapper目的是为了SqlsessionFactory获取核心配置文件后创建sqlsession然后创建代理实现类，代理实现类重写方法时能找到对应的Sql

使用SqlSession操作数据库的步骤如下

![image-20230729093015718](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230729093015718.png)



可以把以上获取SqlSession对象代码封装成一个工具类

![image-20230729093209489](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230729093209489.png)

以上是获取代理实现类(在测试类中操作)

注意：

1、映射文件的命名规则：
表所对应的实体类的类名+Mapper.xml
例如：表t_user，映射的实体类为User，所对应的映射文件为UserMapper.xml
因此一个映射文件对应一个实体类，对应一张表的操作
MyBatis映射文件用于编写SQL，访问以及操作表中的数据
mapper.xml文件所在的包名要与mapper接口所在的包名要完全一致，
2、 MyBatis中可以面向接口操作数据，要保证两个一致：
**a>mapper接口的全类名和映射文件的命名空间（namespace）保持一致**
**b>mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致**

以下是mapper.xml的内容（也设置成模板）

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">

</mapper>
```

## 3.MyBatis核心配置文件详细（不重要）

```
1.<properties>标签：引入properties文件
2.<typeAliases>标签：设置某个类型的别名
属性：type：要设置别名的类型名
     alias：设置某个类型的别名名字（如果不设置，默认为类名，且不区分大小写
     可以嵌套<package>标签，用来为整个包的所有类型设置别名为类名，切不区分大小写
3.environments：配置多个连接数据库的环境
属性：
default：设置默认使用的环境的id
4.environment：配置某个具体的环境
属性：
id：表示连接数据库的环境的唯一标识，不能重复
  transactionManager：设置事务管理方式
属性：
type="JDBC|MANAGED"
JDBC：表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事
务的提交或回滚需要手动处理
MANAGED：被管理，例如Spring   
   dataSource：配置数据源
属性：
type：设置数据源的类型
type="POOLED|UNPOOLED|JNDI"
POOLED：表示使用数据库连接池缓存数据库连接
UNPOOLED：表示不使用数据库连接池
JNDI：表示使用上下文中的数据源
5.<mappers>标签:表示引入映射文件 嵌套<package>标签表示以包为单位引入映射文件
```

## 4.MyBatis增删改查

在使用查询时需要指定ResultType或ResultMap属性，用来接受查询到的结果。

ResultMap在处理一对多映射或多对一映射时或字段名属性不一致时使用

ResultType只能在字段名属性名一致时使用

## 5.MyBatis获取参数

获取参数的两种方式#{}和`${}`      `#{}`:占位符赋值方式，自带单引号。${}:字符串拼接方式，需要手动添加单引号



| 参数类型                                                     | #{}                                                          | ${}                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 单个字面量类型参数                                           | 直接在大括号内写参数名字（任何内容·都能获取，但最好使用参数名字） | 直接在大括号内写参数名字（任何内容·都能获取，但最好使用参数名字）注意添加单引号 |
| 多个字面量类型参数。MyBatis会把参数封装到Map集合中去，以param1...和arg0，arg1...为key，参数值为value。 | 访问param1...和arg0，arg1...获取参数                         | 访问param1...和arg0，arg1...获取参数，同样注意单引号         |
| 对于多个字面量类型参数，可以传入一个map集合，把参数放到map集合中 | 通过访问传入map集合的key来访问参数                           | 通过访问传入map集合的key来访问参数。注意单引号               |
| 实体类类型参数                                               | 直接访问实体类属性获取参数例如#{username}                    | 直接访问实体类属性获取参数例如${username}同样注意单引号      |
| **以上除了实体类类型参数，其他都建议使用@param注解为参数赋予访问名称** |                                                              |                                                              |

## 6. MyBatis的各种查询功能

### 6.1 查询一个实体类对象

注意在mapper映射文件中select标签内设置ResultType为实体类名（字段名和属性名一致时）

### 6.2 查询一个List集合

在mapper映射文件中select标签内设置ResultType为实体类名（字段名和属性名一致时）

注意方法返回类型不能为实体类类型，否则会报toomanyresult异常

### 6.3 查询单个数据（例如查总记录数、查单列字段时）

在MyBatis中，对于Java中常用的类型都设置了类型别名

\* 例如： java.lang.Integer-->int|integer

\* 例如： int-->_int|_integer

\* 例如： Map-->map,List-->list

*/

在resultType上使用常用类型别名即可例如Integer

### 6.4 查询一条数据为map集合时（有时查出的数据没有与之对应的实体类，则用map接收，因为实体类和map类似都是一个属性对应一个值）

此时resultType要用map

### 6.5 查出多条数据为map集合时（也适用于查出的数据没有对应的实体类，这种情况很常见）

此情况不能直接用map集合作为返回类型，因为map时键值对的集合，当每个值都是map时，无法确定其对应的key，用以下三种方法解决

1. 用List<Map<Object,Object>>来接收返回的数据（作为返回类型）**（常用）**

2. 用@MapKey注解，其value为每条数据的主键。这样，可以用一个大的map集合来放多个小map集合，其key为@MapKey的value值即每条数据的主键

   

   

## 7. 特殊类型sql（这种Sql只能用${}来获取参数）

### 7.1 模糊查询

```
模糊查询不能使用#{}因为#{}解析后会有单引号，此时占位符会拼接在单引号内，这样就不是占位符了,可以用以下几种方法来处理模糊查询
1.select * from t_emp where emp_name like '%${贺}%'
2.select * from t_emp where emp_name like "%"#{贺}"%"(常用)
```

### 7.2 批量删除



```
不能使用#{}因为#{}解析后会有单引号，此时占位符会拼接在括号内，这样就不是占位符了可以用${}
select * from t_emp where emp_id in (${ids})
```

### 7.3 动态设置表名（比如当一个用户表有普通用户和会员用户时，此时需要动态获取表名）

```
此时也不能用#{}原因同上面一样也是因为单引号，也要使用${}
select * from ${tableName}

```

### 7.4 获取自增的主键

场景模拟：

场景模拟：

t_clazz(clazz_id,clazz_name)

t_student(student_id,student_name,clazz_id)

1、添加班级信息

2、获取新添加的班级的id

3、为班级分配学生，即将某学的班级id修改为新添加的班级的id

可以在映射文件中insert语句中增加useGeneratedKeys和 keyProperty属性

**useGeneratedKeys：设置使用自增的主键**

**\* keyProperty：因为增删改有统一的返回值是受影响的行数，因此只能将获取的自增的主键放在传输的参**

**数user对象的某个属性中**

## 8. 自定义映射ResultMap

### 8.1 用ResultMap处理字段名和属性名不一致的问题

```
当字段名和属性名不一致时，此时不能直接用resultType设置接受sql查询出的数据的类型可以用以下两种方法
1.使用ResultMap
在mapper映射文件中添加<resultMap>标签 属性：id为唯一标识 Type为无法与表映射的实体类
<resultMap>标签的子标签<id>标签：用来设置主键的映射，他的属性property为映射关系中实体类的属性，column为映射关系中表的字段
<resultMap>标签的子标签<result>标签：用来设置非主键的映射，他的属性property为映射关系中实体类的属性，column为映射关系中表的字段。
例如：<resultMap id="userMap" type="User">
<id property="id" column="id"></id>
<result property="userName" column="user_name"></result>
<result property="password" column="password"></result>
<result property="age" column="age"></result>
<result property="sex" column="sex"></result>
</resultMap>
2.使用别名
3.在MyBatis核心配置文件中设置全局配置mapUnderscoreToCamelCase，可
以在查询表中数据时，自动将_类型的字段名转换为驼峰：在<settings>标签内添加<setting name="mapUnderscoreToCamelCase" value="true"/>
```

### 8.2 多对一映射

当表中存在多对一映射时，例如多个员工对应一个部门，此时如果要查员工信息和其对应的部门时，把查出的数据放在员工实体类中，会出现表中字段和实体类属性无法一 一对应时，即员工类中有部门类的属性，可以用以下三种方法处理这中多对一映射

#### 8.2.1 级联

```
<resultMap id="empDeptMap" type="Emp">
<id column="eid" property="eid"></id>
<result column="ename" property="ename"></result>
<result column="age" property="age"></result>
<result column="sex" property="sex"></result>
<result column="did" property="dept.did"></result>
<result column="dname" property="dept.dname"></result>
</resultMap>
<!--Emp getEmpAndDeptByEid(@Param("eid") int eid);-->
<select id="getEmpAndDeptByEid" resultMap="empDeptMap">
select emp.*,dept.* from t_emp emp left join t_dept dept on emp.did =
dept.did where emp.eid = #{eid}
</select>
```

#### 8.2.2 使用association

```
<resultMap id="empDeptMap" type="Emp">
<id column="eid" property="eid"></id>
<result column="ename" property="ename"></result>
<result column="age" property="age"></result>
<result column="sex" property="sex"></result>
<association property="dept" javaType="Dept">
<id column="did" property="did"></id>
<result column="dname" property="dname"></result>
</association>
</resultMap>
<!--Emp getEmpAndDeptByEid(@Param("eid") int eid);-->
<select id="getEmpAndDeptByEid" resultMap="empDeptMap">
select emp.*,dept.* from t_emp emp left join t_dept dept on emp.did =
dept.did where emp.eid = #{eid}
</select>
```

<association>标签用来处理属性为实体类的映射（多对一的映射）

属性：property 设置映射关系中实体类的和表字段不一致的属性

​           javaType设置property对应的数据类型

#### 8.2.3 使用分步查询(常用)

先查员工信息，在根据查出的部门id去查部门信息

```
<resultMap id="empDeptStepMap" type="Emp">
<id column="eid" property="eid"></id>
<result column="ename" property="ename"></result>
<result column="age" property="age"></result>
<result column="sex" property="sex"></result>
<!--
select：设置分步查询，查询某个属性的值的sql的标识（namespace.sqlId）
column：将sql以及查询结果中的某个字段设置为分步查询的条件
-->
<association property="dept"
select="com.atguigu.MyBatis.mapper.DeptMapper.getEmpDeptByStep" column="did">
</association>
</resultMap>
<!--Emp getEmpByStep(@Param("eid") int eid);-->
<select id="getEmpByStep" resultMap="empDeptStepMap">
select * from t_emp where eid = #{eid}
</select>
/**
* 分步查询的第二步： 根据员工所对应的did查询部门信息
* @param did
* @return
*/
Dept getEmpDeptByStep(@Param("did") int did);

<!--Dept getEmpDeptByStep(@Param("did") int did);-->
<select id="getEmpDeptByStep" resultType="Dept">
select * from t_dept where did = #{did}
</select>
```

### 8.3 一对多映射

当表中存在一对多映射时，例如一个部门对应多个员工，此时如果要查部门信息和对应的员工信息，把查出的数据放在部门实体类中，会出现表中字段和实体类属性无法一 一对应时，即部门类中有List<emp>类型的属性，可以用以下两种方法处理这中一对多映射

#### 8.3.1 使用collection

```
/**
* 根据部门id查新部门以及部门中的员工信息
* @param did
* @return
*/
Dept getDeptEmpByDid(@Param("did") int did);
<resultMap id="deptEmpMap" type="Dept">
<id property="did" column="did"></id>
<result property="dname" column="dname"></result>
<!--
ofType：设置collection标签所处理的集合属性中存储数据的类型
-->
<collection property="emps" ofType="Emp">
<id property="eid" column="eid"></id>
<result property="ename" column="ename"></result>
<result property="age" column="age"></result>
<result property="sex" column="sex"></result>
</collection>
</resultMap>
<!--Dept getDeptEmpByDid(@Param("did") int did);-->
<select id="getDeptEmpByDid" resultMap="deptEmpMap">
select dept.*,emp.* from t_dept dept left join t_emp emp on dept.did =
emp.did where dept.did = #{did}
</select>
```

#### 8.3.2 使用分步查询

1. 先查部门信息

   ```
   <resultMap id="deptEmpStep" type="Dept">
   <id property="did" column="did"></id>
   <result property="dname" column="dname"></result>
   <collection property="emps" fetchType="eager"
   select="com.atguigu.MyBatis.mapper.EmpMapper.getEmpListByDid" column="did">
   </collection>
   </resultMap>
   <!--Dept getDeptByStep(@Param("did") int did);-->
   <select id="getDeptByStep" resultMap="deptEmpStep">
   select * from t_dept where did = #{did}
   </select>
   ```

   2.查部门对应的员工信息

```
<!--List<Emp> getEmpListByDid(@Param("did") int did);-->
<select id="getEmpListByDid" resultType="Emp">
select * from t_emp where did = #{did}
</select>
```

## 9.动态SQL

### 9.1 if标签

当做查询需要拼接SQL时，可能会有很多判断，判断不为null时才会拼接，如果全部自己判断，会十分麻烦，此时可以在mapper映射文件中<select>标签内加上<if>标签。当if标签中test属性中表达式成立时才会把if标签中·的内容拼接到sql中

例

```
<!--List<Emp> getEmpListByCondition(Emp emp);-->
<select id="getEmpListByMoreTJ" resultType="Emp">
select * from t_emp where 1=1
<if test="ename != '' and ename != null">
and ename = #{ename}
</if>
<if test="age != '' and age != null">
and age = #{age}
</if>
<if test="sex != '' and sex != null">
and sex = #{sex}
</if>
</select>
```

### 9.2 where标签

一般和if标签配合使用

作用

1. 当where标签内至少有一条if成立时，会自动在SQL语句中添加where

2. 当where标签内所有if条件都不成立时，不会添加where

3. 会自动去掉where标签中if内容前有多余的and

   注意：不会去掉if内容后面多余的and

```
<select id="getEmpListByMoreTJ2" resultType="Emp">
select * from t_emp
<where>
<if test="ename != '' and ename != null">
ename = #{ename}
</if>
<if test="age != '' and age != null">
and age = #{age}
</if>
<if test="sex != '' and sex != null">
and sex = #{sex}
</if>
</where>
</select
```

### 9.3 trim标签

属性

suffix和preix作用分别是在标签后面和标签前面加内容

suffixOverrides和preixOverrides作用分别是在标签后面和前面删除内容

```
select id="getEmpListByMoreTJ" resultType="Emp">
select * from t_emp
<trim prefix="where" suffixOverrides="and">
<if test="ename != '' and ename != null">
</if>
<if test="age != '' and age != null">
age = #{age} and
</if>
<if test="sex != '' and sex != null">
sex = #{sex}
</if>
</trim>
</select>
```

### 9.4 choose when otherwise(不重要)

相当于if else-if else 结构

### 9.5 foreach标签

当需要批量添加和批量删除时会使用到

#### 9.5.1 批量添加

<foreach>标签中内容表示输出

<foreach>标签中属性：

1. collection:设置要遍历的集合
2. item：设置每次遍历获得的元素
3. separator：设置输出结果之间的间隔元素

```
<!--     void insertMoreEmp(@Param("emps") List<Emp> emps);-->
    <insert id="insertMoreEmp">
        insert into t_emp values
        <foreach collection="emps" item="emp" separator=",">
            (null,#{emp.empName},#{emp.empAge},#{emp.empGender},null)
        </foreach>
    </insert>

```

#### 9.5.2 批量删除

<foreach>标签中内容表示输出

<foreach>标签中属性：

1. collection:设置要遍历的集合
2. item：设置每次遍历获得的元素
3. separator：设置输出结果之间的间隔元素
4. open:设置foreach内容前面的元素（什么时候开始）
5. close：设置foreach内容后面的元素（什么时候结束）

```
<delete id="deleteMoreEmp">
        delete from t_emp where emp_id in
        <foreach collection="empIds" item="empId" separator="," open="(" close=")">
            #{empId}
        </foreach>
    </delete>
```

### 9.6 SQL片段（不重要，认识即可）

sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```
<sql id="empColumns">
eid,ename,age,sex,did
</sql>
select <include refid="empColumns"></include> from t_emp
```

## 10. MyBatis的缓存

### 10.1 一级缓存

sqlsession级别，默认存储、自动存储

失效原因：

1. 不是同一个sqlsession对象执行的查询
2. 同一个sqlsession查询时条件不同
3. 同一个SQL两次查询之间执行了任意的dml语句，因为dml语句会清空缓存
4. 同一个SQL两次查询之间手动清空缓存 `sqlSession.clearCache();`

### 10.2 二级缓存

SqlSessionFactory级别，需要手动开启

**手动开启条件：**

1. **在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置**

2. **在mapper映射文件中添加<cache>标签**

3. **关闭或提交SqlSession**

4. **查询的数据所转换的实体类类型必须实现序列化的接口**

   **使二级缓存失效的情况：**

   **两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效**

在mapper配置文件中添加的cache标签可以设置一些属性：

①eviction属性：缓存回收策略，默认的是 LRU。

LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。

FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。

SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。

WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。

②flushInterval属性：刷新间隔，单位毫秒

默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新

③size属性：引用数目，正整数

代表缓存最多可以存储多少个对象，太大容易导致内存溢出

④readOnly属性：只读， true/false

true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了 很重

要的性能优势。

false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是

false。

### 10.3 缓存查询顺序

先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。

如果二级缓存没有命中，再查询一级缓存

如果一级缓存也没有命中，则查询数据库

**SqlSession关闭之后，一级缓存中的数据会写入二级缓存**

### 10.4 整合第三方缓存

## 11 MyBatis逆向工程

所谓逆向工程就是根据数据库表生成实体类和mapper接口、映射文件的过程

（1） 添加依赖和插件

```
<!-- 依赖MyBatis核心包 -->

<dependencies>
<dependency>
<groupId>org.mybatis</groupId>
<artifactId>mybatis</artifactId>
<version>3.5.7</version>
</dependency>
<!-- junit测试 -->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
<!-- log4j日志 -->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.16</version>
</dependency>
</dependencies>
<!-- 控制Maven在构建过程中相关配置 -->
<build>
<!-- 构建过程中用到的插件 -->
<plugins>
<!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
<plugin>
<groupId>org.mybatis.generator</groupId>
<artifactId>mybatis-generator-maven-plugin</artifactId>
<version>1.3.0</version>
<!-- 插件的依赖 -->
<dependencies>
<!-- 逆向工程的核心依赖 -->
<dependency>
<groupId>org.mybatis.generator</groupId>
<artifactId>mybatis-generator-core</artifactId>
<version>1.3.2</version>
</dependency>
<!-- MySQL驱动 -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.16</version>
</dependency>
</dependencies>
</plugin>
</plugins>

</build>
```

(2) 创建MyBatis 核心配置文件

（3）创建逆向工程的配置文件

文件名必须是：generatorConfig.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
<!--
targetRuntime: 执行生成的逆向工程的版本
MyBatis3Simple: 生成基本的CRUD（清新简洁版）
MyBatis3: 生成带条件的CRUD（奢华尊享版）
-->
<context id="DB2Tables" targetRuntime="MyBatis3">
<!-- 数据库的连接信息 -->
<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
connectionURL="jdbc:mysql://localhost:3306/mybatis?
serverTimezone=UTC"
userId="root"
password="123456">
</jdbcConnection>
<!-- javaBean的生成策略-->
<javaModelGenerator targetPackage="com.atguigu.mybatis.pojo"
targetProject=".\src\main\java">
<property name="enableSubPackages" value="true" />
<property name="trimStrings" value="true" />
</javaModelGenerator>
<!-- SQL映射文件的生成策略 -->
<sqlMapGenerator targetPackage="com.atguigu.mybatis.mapper"
targetProject=".\src\main\resources">
<property name="enableSubPackages" value="true" />
</sqlMapGenerator>
<!-- Mapper接口的生成策略 -->
<javaClientGenerator type="XMLMAPPER"
targetPackage="com.atguigu.mybatis.mapper" targetProject=".\src\main\java">
<property name="enableSubPackages" value="true" />
</javaClientGenerator>
<!-- 逆向分析的表 -->
<!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
<!-- domainObjectName属性指定生成出来的实体类的类名 -->
<table tableName="t_emp" domainObjectName="Emp"/>
<table tableName="t_dept" domainObjectName="Dept"/>
</context>
</generatorConfiguration>
```

(4) 执行执行MBG插件的generate目标

![image-20230729184907856](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230729184907856.png)

使用：

@Test

public void testMBG(){

try {

InputStream is = Resources.getResourceAsStream("mybatis-config.xml");

SqlSessionFactory sqlSessionFactory = new

SqlSessionFactoryBuilder().build(is);

SqlSession sqlSession = sqlSessionFactory.openSession(true);

EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);

//查询所有数据

/*List<Emp> list = mapper.selectByExample(null);

list.forEach(emp -> System.out.println(emp));*/

//根据条件查询

/*EmpExample example = new EmpExample();

example.createCriteria().andEmpNameEqualTo("张

三").andAgeGreaterThanOrEqualTo(20);

example.or().andDidIsNotNull();

List<Emp> list = mapper.selectByExample(example);

list.forEach(emp -> System.out.println(emp));*/

mapper.updateByPrimaryKeySelective(new

Emp(1,"admin",22,null,"456@qq.com",3));

} catch (IOException e) {

e.printStackTrace();

}

}

## 12. 分页插件

使用步骤

（1）添加依赖

```
<dependency>
<groupId>com.github.pagehelper</groupId>
<artifactId>pagehelper</artifactId>
<version>5.2.0</version>
</dependency>
```

（2）配置分页插件

在MyBatis的核心配置文件中配置插件

```
<plugins>
<!--设置分页插件-->
<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

a>在mapper调用查询功能方法之前使用PageHelper.startPage(int pageNum, int pageSize)开启分页功能，一般在service层

`pageNum：当前页的页码`

`pageSize：每页显示的条数`

b>在查询获取list集合之后，使用PageInfo<T> pageInfo = new PageInfo<>(List<T> list, int

navigatePages)获取分页相关数据

list：分页之后的数据

navigatePages：导航分页的页码数

c>分页相关数据

PageInfo{

pageNum=8, pageSize=4, size=2, startRow=29, endRow=30, total=30, pages=8,

list=Page{count=true, pageNum=8, pageSize=4, startRow=28, endRow=32, total=30,

pages=8, reasonable=false, pageSizeZero=false},

prePage=7, nextPage=0, isFirstPage=false, isLastPage=true, hasPreviousPage=true,

hasNextPage=false, navigatePages=5, navigateFirstPage4, navigateLastPage8,

navigatepageNums=[4, 5, 6, 7, 8]

}

pageNum：当前页的页码

pageSize：每页显示的条数

size：当前页显示的真实条数

total：总记录数

pages：总页数

prePage：上一页的页码

nextPage：下一页的页码

isFirstPage/isLastPage：是否为第一页/最后一页

hasPreviousPage/hasNextPage：是否存在上一页/下一页

navigatePages：导航分页的页码数

navigatepageNums：导航分页的页码，[1,2,3,4,5]



# 二 Spring

## 1. IOC 

>
>
>常见概念
>
>IOC：控制翻转，把对象的控制权和对象生命周期交给IOC容器，获取资源直接从IOC 容器获取
>
>DI：依赖注入，即组件以一些预先定义好的方式（例如：setter 方法）接受来自于容器
>
>的资源注入。通俗的讲即给IOC容器管理的对象赋值，在获取对象时直接注入依赖。是IOC思想的一个具体实现
>
>aop：面向切面编程
>
>声明式：区别于编程式。比如我们需要某个资源如对象，可以直接告诉程序，由程序帮我们创建，不用手动编写代码创建
>
>组件：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用 XML
>
>和 Java 注解组合这些对象。这使得我们可以基于一个个功能明确、边界清晰的组件有条不紊的搭
>
>建超大型复杂应用系统。例如**对象**

## 2. IOC在spring具体实现

>
>
>1. BeanFactory:给Spring使用
>2. ApplicationContext:给开发者使用

ApplicationContext常见实现类

![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230731090610455.png)

```java
通常用这两个类来获取IOC容器
1.FileSystemXmlApplicationContext：通过文件系统路径读取 XML 格式的配置文件创建 IOC 容
器对象
 2.ClassPathXmlApplicationContext（最常用)：通过读取类路径下的 XML 格式的配置文件创建 IOC 容器
对象 因为java可能会打包成jar包在其他电脑使用   
3.ConfigurableApplicationContext：ApplicationContext 的子接口，包含一些扩展方法
refresh() 和 close() ，让 ApplicationContext 具有启动、
关闭和刷新上下文的能力。
4.WebApplicationContext：专门为 Web 应用准备，基于 Web 环境创建 IOC 容器对
象，并将对象引入存入 ServletContext 域中。
```

## 3. 创建Spring工程过程

1. 创建maven工程，在pom.xml文件导入相关依赖

```html
<dependencies>
        <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>

            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

2.创建Spring配置文件，取名随意，最好是ApplicationContext.因为后面ssm整合时，名字必须为这个

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>    
```

此文件用来配置bean，即把类（对象）控制权交给IOC容器

**利用IOC容器获取对象的思路：先通过配置Bean把类（对象）控制权交给IOC容器，然后通过ClassPathXmlApplicationContext加载配置文件，然后获得组件对象**

Spring 通过IOC创建对象底层使用反射机制获取对象，所以必须有无参构造器。没有无参构造器，则会抛出下面的异常：

>
>
>org.springframework.beans.factory.BeanCreationException: Error creating bean with name
>
>'helloworld' defined in class path resource [applicationContext.xml]: Instantiation of bean
>
>failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed
>
>to instantiate [com.atguigu.spring.bean.HelloWorld]: No default constructor found; nested
>
>exception is java.lang.NoSuchMethodException: com.atguigu.spring.bean.HelloWorld.<init>
>
>()

## 4.获取bean对象的几种方式

1.使用id获取

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        Student studentOne = (Student) applicationContext.getBean("studentOne");
```

注意：通过此种方法获得的对象为Object类型，需要强转为对象所对应的类型，这样才能调用方法。

2.使用类型获取（**常用）**

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
 Student bean = applicationContext.getBean(Student.class);
```

注意：此种方法不用类型强转，但要注意配置bean时，一个类型有且仅有一个bean与之对应。当一个类型有多个bean与之对应时，会报NoUniqueBeanDefinitionException异常，而当没有bean与之对应时再用类型方式获取对象时会报NoSuchBeanDefinitionException异常。**在实际开发中，每个类型配置一个bean就行**

3.使用id和类型获取

```java
 ApplicationContext applicationContext = new ClassPathXmlApplicationContext("ApplicationContext.xml");
Student studentOne = applicationContext.getBean("studentThree", Student.class);
```

注意：此种方法在当一个类型有多个bean与之对应时能更准确无误的获取对象。

### 组件类实现了接口，可以用接口类型获取bean，但要求bean唯一，当接口有多个实现类且都配置了bean时就不能用接口类型获取bean

###  总结：根据类型来获取bean时，在满足bean唯一性的前提下，其实只是看：『对象 **instanceof** 指定的类

型』的返回结果，只要返回的是true就可以认定为和类型匹配，能够获取到。

## 5. 依赖注入

### 5.1 使用setter注入

在配置文件bean标签内加上property标签，通过name属性指定要注入的属性名，注意属性名时和get，set方法有关的要和成员变量区分开。通过value属性设置要给属性赋的值(赋的值为字面量时使用value）。也可以在标签内部加上<value>标签进行赋值。

当获取对象时会自动为对象属性赋值（依赖注入）

```java
<bean id="studentTwo" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender" value="男"></property>
     </bean>
```

### 5.2 使用构造器注入

使用构造器注入时，可以省略name属性，**不过要确保constructor-arg标签value属性类型要与有参构造器中成员变量的类型兼容**

当有一个以上构造器参数数量相同时，且constructor-arg中value属性类型与这些构造器参数类型兼容即都能为其赋值，此时constructor-arg将使用其中一个构造器赋值，此时若要指定某个构造器，**必须使用name属性指定。**

```java
 <bean id="studentThree" class="com.hbw.spring.pojo.Student">
          <constructor-arg  value="12"></constructor-arg>
          <constructor-arg  value="王"></constructor-arg>
          <constructor-arg  value="女"></constructor-arg>
          <constructor-arg  value="23" name="age"></constructor-arg>
     </bean>
```

```java
<bean id="studentThree" class="com.hbw.spring.pojo.Student">
          <constructor-arg  value="12"></constructor-arg>
          <constructor-arg  value="王"></constructor-arg>
          <constructor-arg  value="女"></constructor-arg>
          <constructor-arg  value="23" name="age"></constructor-arg>
     </bean>
```

```java
public Student(Integer sId, String sName,  String gender,Integer age) {
        this.sId = sId;
        this.sName = sName;
        this.gender = gender;
        this.age = age;
    }
    public Student(Integer sId, String sName,  String gender,Double score) {
        this.sId = sId;
        this.sName = sName;
        this.gender = gender;
        this.score = score;
    }
```

### 5.3 特殊值处理

1.字面量（基本数据类型，字符串等）赋值

直接使用value属性或者<value>标签赋值

2.null类型

>
>
>当要给属性赋值一个null空对象时，不能用value为其赋值null
>
>要使用<null>标签

3.xml实体

>
>
>当要给属性赋的值中有">"<"这种特殊字符时，要使用对应的xml实体
>
>例如&lt代表小于号

4.CDATA节

>
>
>直接在IDEA敲CD回车就可以生成<![CDATA[<男>]]，在解析时会被当作纯文本处理
>
>不过要在value标签内使用

5.类类型

>
>
>当要给属性赋类类型的值时，可以使用两种方法
>
>1. 引入外部bean
>
>   直接在property属性中使用ref来引入外部bean，要给ref设置外部bean的id

```html
<bean id="studentSix" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender">
               <value><![CDATA[<男>]]></value>
          </property>
          <property name="score" value="100"></property>
          <property name="clazz" ref="clazz"></property>
     </bean>


<bean id="clazz" class="com.hbw.spring.pojo.Clazz">
          <property name="cId" value="100"></property>
          <property name="cName" value="哈哈"></property>
     </bean>
```

>
>
>2.级联（不使用·）
>
>3.使用内部bean，在property标签内使用<bean>标签，用法和在外部定义<bean>标签一致
>
>注意该bean和内部类类似，不能在外部使用，也就是在外部使用IOC容器拿不到
>
>

```html
<bean id="studentSeven" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender">
               <value><![CDATA[<男>]]></value>
          </property>
          <property name="score" value="100"></property>
          <property name="clazz" >
               <bean id="clazzInner" class="com.hbw.spring.pojo.Clazz">
                    <property name="cName" value="贺博文"></property>
                    <property name="cId" value="1020"></property>
               </bean>
          </property>
        
           </bean>
```

6.数组类型

>
>
>要给属性赋数组类型的值时，要在<property>标签内使用<Array>标签,当数组中的值为字面量类型时，则要在<Array>标签中使用<value>标签为其赋值

```html
 <bean id="studentSeven" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender">
               <value><![CDATA[<男>]]></value>
          </property>
          <property name="score" value="100"></property>
          
          <property name="hobby">
               <array>
                    <value>和</value>
                    <value>你</value>
                    <value>玩</value>

               </array>
          </property>

         
     </bean>
```

7.List集合类型

>
>
>当要给属性赋List集合类型的值时，和赋类类型值做法类似，有两种方法：引入外部List集合（注意在外部定义List集合时，要util:前缀)和使用内部List
>
>1.引入外部定义好的list
>
>外部定义List集合时要使用util:前缀，内部使用ref引入外部bean来给该list集合赋值，也可以使用内部bean来给list集合赋值

```html
 <bean id="clazzOne" class="com.hbw.spring.pojo.Clazz">
          <property name="cId" value="100"></property>
          <property name="cName" value="哈哈"></property>
          <property name="students" ref="clazzList"></property>

     </bean>
     

    <util:list id="clazzList">
          <ref bean="studentOne"></ref>
          <ref bean="studentSeven"></ref>
          <bean id="beanInner01" class="com.hbw.spring.pojo.Student">
               <property name="sId" value="10086"></property>
               <property name="sName" value="陆逊"></property>
          </bean>
     </util:list>
```

>
>
>2.使用内部list
>
>即在property标签内使用List标签，该标签使用方法与在外部配置list标签用法一致

```html
<bean id="clazzOne" class="com.hbw.spring.pojo.Clazz">
          <property name="cId" value="100"></property>
          <property name="cName" value="哈哈"></property>
          <property name="students" ref="clazzList"></property>
          <property name="students">
               <list>
                    <ref bean="studentOne"></ref>
                    <ref bean="studentThree"></ref>
                    <ref bean="studentTwo"></ref>
                    
               </list>
          </property>
     </bean>
```

8.map类型

>
>
>当要给属性赋map类型值时，和赋类类型值和赋List集合类型值做法类似，有两种方法：引入外部map集合（注意在外部定义map集合时，要util:前缀)和使用内部map
>
>1.引入外部定义好的map
>
>外部定义map集合时要使用util:前缀，内部使用entry标签来给map赋值，entry标签有两个常用的属性key和value-ref和value，key用来设置map中每个元素的键，value用来设置每个元素的值(每个元素值为字面量类型时），当map集合中元素值为类类型时，使用value-ref，它的值通常为外部bean的id，这种情况不能使用内部bean来给map中元素赋值，只能用value—ref‘来引入外部bean

```html
 <bean id="studentSeven" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender">
               <value><![CDATA[<男>]]></value>
          </property>
          <property name="score" value="100"></property>
          
          
          <property name="teacherMap" ref="map"></property>
     </bean>


<util:map id="map">
          <entry key="10086" value-ref="teacherOne"></entry>
          <entry key="10010" value-ref="teacherTwo"></entry>
     </util:map>
```

>
>
>2.使用内部map
>
>即在property标签内使用mapt标签，该标签使用方法与在外部配置map标签用法一致

```html
<bean id="studentSeven" class="com.hbw.spring.pojo.Student">
          <property name="sId" value="10"></property>
          <property name="sName" value="贺博文"></property>
          <property name="age" value="23"></property>
          <property name="gender">
               <value><![CDATA[<男>]]></value>
          </property>
          <property name="score" value="100"></property>
         
          <property name="teacherMap">
               <map>
                    <entry key="10086" value-ref="teacherOne"></entry>
                    <entry key="10010" value-ref="teacherTwo"></entry>
               </map>
          </property>
          
     </bean>
```

## 5.4 bean作用域

>
>
>用bean标签中scope属性指定。 scope标签有两个值 prototype（多例）和singleton（单例）默认值为单例。
>
>如果为单例，在获得容器时就会创建。多例在获得该对象时才会被创建

如果是在WebApplicationContext环境下还会有另外两个作用域（但不常用）：

request 

在一个请求范围内有效

session 

在一个会话范围内有效

 ## 5.5 bean生命周期

1.实例化（利用无参构造器创建对象）

2.依赖注入

3.初始化前置操作（额外对bean执行的操作）

4.初始化

5.初始化后置操作（使用bean后置处理器）

6.使用

7.销毁

8.IOC容器关闭

其中初始化和销毁方法要在bean标签内指定，初始化方法使用init-method来指定。销毁方法使用destroy-method来指定

### 5.5.1 bean后置处理器

bean的后置处理器会在生命周期的初始化前后添加额外的操作，需要实现BeanPostProcessor接口，

且配置到IOC容器中，需要注意的是，bean后置处理器不是单独针对某一个bean生效，而是针对IOC容

器中所有bean都会执行

```html
package com.atguigu.spring.process;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
public class MyBeanProcessor implements BeanPostProcessor {
@Override
public Object postProcessBeforeInitialization(Object bean, String beanName)
throws BeansException {
System.out.println("☆☆☆" + beanName + " = " + bean);
return bean;
}
@Override
public Object postProcessAfterInitialization(Object bean, String beanName)
throws BeansException {
System.out.println("★★★" + beanName + " = " + bean);
return bean;
}
}
<!-- bean的后置处理器要放入IOC容器才能生效 -->
<bean id="myBeanProcessor" class="com.atguigu.spring.process.MyBeanProcessor"/>
```

## 5.6 FactoryBean

是一个工厂类，用来创建bean对象。FactoryBean是Spring提供的一种整合第三方框架的常用机制。和普通的bean不同，**配置一个**

**FactoryBean类型的bean，在获取bean的时候得到的并不是class属性中配置的这个类的对象，而是**

**getObject()方法的返回值。**通过这种机制，Spring可以帮我们把复杂组件创建的详细过程和繁琐细节都

屏蔽起来，只把最简洁的使用界面展示给我们。

将来我们整合Mybatis时，Spring就是通过FactoryBean机制来帮我们创建SqlSessionFactory对象的。

## 5.7 基于xml的自动装配

自动装配：对类、接口类型的属性进行自动依赖注入

在bean标签可以使用autowire属性来实现对属性的自动装配，autowire有两个值byType和ByName

Bytype根据要自动装配的属性的类型来匹配bean。（常用）ByName使用要自动装配属性的名字来匹配id与名字相同的bean进行赋值

```html
<bean id="controller" class="com.hbw.controller.UserController" autowire="byName">
<!--    <property name="userService" ref="service"></property>-->
</bean>
    <bean id="service" class="com.hbw.service.impl.UserServiceImpl" autowire="byName"   >
<!--        <property name="userDao" ref="dao"></property>-->

    </bean>
    <bean id="userService" class="com.hbw.service.impl.UserServiceImpl" autowire="byName">
        <!--        <property name="userDao" ref="dao"></property>-->
    </bean>
    <bean id="userDao" class="com.hbw.dao.Impl.UserDaoImpl"></bean>
    <bean id="dao" class="com.hbw.dao.Impl.UserDaoImpl"></bean>
```

当byType无法使用时（一个类型有多个bean对应）才使用ByName

几种特殊情况：

- 使用ByType时一个类型有多个bean与之对应，此时编译就不会通过，会报  NoUniqueBeanDefinitionException异常
- 使用ByType时一个类型没有bean与之对应时，属性不装配会使用默认的值。
- 使用ByName时没有名字与属性相匹配的bean时，属性不装配使用默认的值。
- 使用ByName不会出现有多个名字与属性相匹配的bean，因为bean'的id是唯一的。

# 6.使用注解来管理bean

**注解没有实质性的作用只是作为有一个标记，在扫描时看到这个标记就会执行某些操作。**

在需要交给IOC容器管理的类上方加入以下四个注解

```html
@Controller：把类标记成Controller组件交给IOC容器管理
@Service：把类标记成Service组件交给IOC容器管理
@Repository：把类标记成dao组件交给IOC容器管理
@Component：把类标记成组件交给IOC容器管理

```

**这四种注解功能其实是一致的都是把类标记成组件交给IOC容器管理，之所以使用@Controller @Service @Repository是给开发人员看的，提高代码可读性**

>
>
>使用了注解，就不用在配置文件中配置bean了
>
>只需要在配置文件中使用<context:component-scan base-package="com.hbw.spring"><</context:component-scan>>标签来扫描。base-package属性用来设置要扫描的包

## 1.扫描时排除组件

>
>
><!-- context:exclude-filter标签：指定排除规则 -->
><!--
>type：设置排除或包含的依据
>type="annotation"，根据注解排除，expression中设置要**排除的注解的全类名**
>type="assignable"，根据类型排除，expression中设置要**排除的类型的全类名**

根据注解排除：

```xml
<context:component-scan base-package="com.hbw.spring" >
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

根据类型排除:

```xml
<context:component-scan base-package="com.hbw.spring" >
<!--    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>-->
    <context:exclude-filter type="assignable" expression="com.hbw.spring.controller.UserController"/>
</context:component-scan>
```

## 2.扫描时包含组件：‘

>type：设置排除或包含的依据
>type="annotation"，根据注解排除，expression中设置要**排除的注解的全类名**
>type="assignable"，根据类型排除，expression中设置要**排除的类型的全类名**
>
>```xml
>使用context:include-filter标签来设置扫描时包含哪些组件，使用这个标签时必须在<context:component-scan标签中指定use-default-filters属性值为false
>```

使用类型包含：

```xml
<context:component-scan base-package="com.hbw.spring" use-default-filters="false">


<context:include-filter type="assignable" expression="com.hbw.spring.controller.UserController"/>
    </context:component-scan>
```

使用注解包含：

```xml
<context:component-scan base-package="com.hbw.spring" use-default-filters="false">

<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
<!--<context:include-filter type="assignable" expression="com.hbw.spring.controller.UserController"/>-->
    </context:component-scan>
```

## 3.使用注解配置的bean的id

>
>
>默认时类名所对应的驼峰形式例如UserController对应userController
>
>可通过标识组件的注解的value属性设置自定义的bean的id

# 7.使用注解来自动装配

>
>
>使用@Autowired注解，在成员变量上直接标记@Autowired注解即可完成自动装配，不需要提供setXxx()方法。以后我们在项 目中的正式用法就是这样。
>
>@Autowired注解可以标记在构造器和set方法上

# 8.@Autowired工作流程

![image-20230818151419704](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230818151419704.png)

>
>
>首先根据所需要的组件类型到IOC容器中查找 能够找到唯一的bean：直接执行装配 如果完全找不到匹配这个类型的bean：装配失败 和所需类型匹配的bean不止一个 没有@Qualifier注解：根据@Autowired标记位置成员变量的变量名作为bean的id进行 匹配 能够找到：执行装配 找不到：装配失败 使用@Qualifier注解：根据@Qualifier注解中指定的名称作为bean的id进行匹配 能够找到：执行装配 找不到：装配失败

注意：

>
>
>在以往xml方式配置bean自动装配时，如果找不到，则不装配，不会报错。
>
>而@Autowired中有属性required，默认值为true，**因此在自动装配无法找到相应的bean时，会装 配失败**报异常**org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'employeeController': Unsatisfied dependency expressed through field 'employeeService'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.hbw.ssm.service.EmployeeService' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}** 可以将属性required的值设置为true，则表示能装就装，装不上就不装，此时自动装配的属性为 默认值 但是实际开发时，**基本上所有需要装配组件的地方都是必须装配的，用不上这个属性。**

# 9.AOP

>
>
>aop即面向切面编程，以往面向对象编程时封装只能全部封装，不能分段封装，例如封装事务、加日志就无法实现。使用aop则可以解决这个问题

![image-20230818152540750](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230818152540750.png)

>
>
>①现有代码缺陷 针对带日志功能的实现类，我们发现有如下缺陷： 对核心业务功能有干扰，导致程序员在开发核心业务功能时分散了精力 附加功能分散在各个业务功能方法中，不利于统一维护
>
>②解决思路 解决这两个问题，核心就是：解耦。我们需要把附加功能从业务功能代码中抽取出来。
>
>③困难 解决问题的困难：要抽取的代码在方法内部，靠以前把子类中的重复代码抽取到父类的方式没法解决。 所以需要引入新的技术。

# 10.代理模式

不直接调用方法，而是由代理实现类去调用方法，这样就可以添加额外的非核心业务代码，让不属于目标方法核心逻辑 的代码从目标方法中剥离出来——解耦。

### 1.静态代理

创建静态代理类

```java
public class CalculatorStaticProxy implements Calculator {
// 将被代理的目标对象声明为成员变量
private Calculator target;
public CalculatorStaticProxy(Calculator target) {
this.target = target;
}
@Override
public int add(int i, int j) {
// 附加功能由代理类中的代理方法来实现
System.out.println("[日志] add 方法开始了，参数是：" + i + "," + j);
// 通过目标对象来实现核心业务逻辑
int addResult = target.add(i, j);
System.out.println("[日志] add 方法结束了，结果是：" + addResult);

静态代理确实实现了解耦，但是由于代码都写死了，完全不具备任何的灵活性。就拿日志功能来
说，将来其他地方也需要附加日志，那还得再声明更多个静态代理类，那就产生了大量重复的代
码，日志功能还是分散的，没有统一管理。
提出进一步的需求：将日志功能集中到一个代理类中，将来有任何日志需求，都通过这一个代理
类来实现。这就需要使用动态代理技术了。


```

## 2.动态代理

生产代理对象的工厂类

```java
public class ProxyFactory {
private Object target;
public ProxyFactory(Object target) {
this.target = target;
}
public Object getProxy(){
/**
* newProxyInstance()：创建一个代理实例
* 其中有三个参数：
* 1、classLoader：加载动态生成的代理类的类加载器
* 2、interfaces：目标对象实现的所有接口的class对象所组成的数组
* 3、invocationHandler：设置代理对象实现目标对象方法的过程，即代理类中如何重写接
口中的抽象方法
*/
ClassLoader classLoader = target.getClass().getClassLoader();
Class<?>[] interfaces = target.getClass().getInterfaces();
InvocationHandler invocationHandler = new InvocationHandler() {
@Override
public Object invoke(Object proxy, Method method, Object[] args)
throws Throwable {
/**
* proxy：代理对象
* method：代理对象需要实现的方法，即其中需要重写的方法
* args：method所对应方法的参数

Object result = null;
try {
System.out.println("[动态代理][日志] "+method.getName()+"，参
数："+ Arrays.toString(args));
result = method.invoke(target, args);
System.out.println("[动态代理][日志] "+method.getName()+"，结
果："+ result);
} catch (Exception e) {
e.printStackTrace();
System.out.println("[动态代理][日志] "+method.getName()+"，异
常："+e.getMessage());
} finally {
System.out.println("[动态代理][日志] "+method.getName()+"，方法
执行完毕");
}
return result;
}
};
return Proxy.newProxyInstance(classLoader, interfaces,
invocationHandler);
}
}

```

# 11.AOP概念和相关术语

# 1.概念

AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面 向对象编程的一种补充和完善，它以通过预编译方式和运行期动态代理方式实现在不修改源代码的情况 下给程序动态统一添加额外功能的一种技术。

## 2.相关术语

### 1.横切关注点

>
>
>要附加的功能就叫一个横切关注点

### 2.通知

>在每一个横切关注点要做的事情要有一个方法实现，这样的方法就叫通知方法
>
>每一个横切关注点对应一个通知方法
>
>分为四种
>
>1.前置通知：在目标方法执行之前执行
>
>2.返回通知：在目标方法成功之后执行
>
>3.异常通知：在目标方法执行出现异常时执行
>
>4.后置通知：最后执行
>
>5.环绕通知：使用try...catch...finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所 有位置

### 3.切面

>
>
>封装通知方法的类

### 4.连接点

>即通知方法作用的位置
>
>是一个纯粹的概念，没有代码实现

![image-20230819155701306](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230819155701306.png)

### 5.目标

>
>
>即被代理的对象。是核心代码

### 6.切入点

**定位连接点的方式**。 每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物（从逻辑上来说）。 **如果把连接点看作数据库中的记录，那么切入点就是查询记录的 SQL 语句。** Spring 的 AOP 技术可以通过切入点定位到特定的连接点。 切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条 件。

### 7.代理

>
>
>向目标对象应用通知之后创建的代理对象。

## 3.优点

- 简化代码：把方法中固定位置的重复的代码抽取出来，让被抽取的方法更专注于自己的核心功能， 提高内聚性。 
- 代码增强：把特定的功能封装到切面类中，看哪里有需要，就往上套，被套用了切面逻辑的方法就 被切面给增强了。

## 4.基于注解的AOP

- 动态代理（InvocationHandler）：JDK原生的实现方式，需要被代理的目标类必须实现接口。因 为这个技术要求代理对象和目标对象实现同样的接口（兄弟两个拜把子模式）。
- cglib：通过继承被代理的目标类（认干爹模式）实现代理，所以不需要目标类实现接口。
- AspectJ：本质上是静态代理，将代理逻辑“织入”被代理的目标类编译得到的字节码文件，所以最 终效果是动态的。weaver就是织入器。Spring只是借用了AspectJ中的注解。

## 5.准备工作

### 1.添加依赖

```xml
 <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.3.1</version>
        </dependency>
```

###  2.准备测试用到的一系列资源

注意：

- 切面类要用@Aspect注解标识为切面类，而且要用 @Component把切面类放到IOC容器内

- 必须有接口

- @Before标识前置通知，@AfterReturning表示返回通知，@After表示后置通知，@AfterThrowing标识异常通知，@Around标识环绕通知。且这些注解都要使用value属性指定切入点表达式,value属性指定execution，execution中指定连接点的全名称（全类名+方法名加参数），**execution(* com.hbw.spring.aop.annotation.CalculatorPureImpl.*(..))**这个表示CalculatorPureImpl所有方法

- 可以用 @Pointcut来统一管理切入点表达式

  ```java
   @Pointcut("execution(* com.hbw.spring.aop.annotation.CalculatorPureImpl.*(..))")
      public void pointCut(){}
      @Before("pointCut()")
      public void calculatorAopBefore(){
          System.out.println("日志");
      }
      @After("pointCut()")
      public void calculatorAopAfter(JoinPoint joinPoint){
          Signature signature = joinPoint.getSignature();
          System.out.println("方法"+signature.getName()+"后置通知");
      }
      @AfterReturning(value = "pointCut()",returning = "result" )
      public void calculatorAopAfterReturn(JoinPoint joinPoint,int result){
          Signature signature = joinPoint.getSignature();
  
          System.out.println("方法"+signature.getName()+"返回值为"+result);
      }
      @AfterThrowing(value = "pointCut()",throwing = "e")
      public void calculatorAopAfterThrow(JoinPoint joinPoint,Exception e){
          Signature signature = joinPoint.getSignature();
  
          System.out.println("方法"+signature.getName()+"出错"+e);
      }
      @Around("pointCut()")
      public Object calculatorAopAround(ProceedingJoinPoint proceedingJoinPoint){
          Object result = null;
          try {
              System.out.println("环绕通知"+"前置通知");
              result = proceedingJoinPoint.proceed();
              System.out.println("环绕通知"+"返回通知");
          } catch (Throwable e) {
  
              System.out.println("环绕通知"+"异常通知");
          } finally {
              System.out.println("环绕通知"+"后置通知");
          }
          return result;
  
  
      }
  }
  ```

  

- **要在配置文件中配置<aop:aspectj-autoproxy />**

- 各种通知的执行顺序： Spring版本5.3.x以前： 前置通知 目标操作 后置通知 返回通知或异常通知。 Spring版本5.3.x以后： 前置通知 目标操作 返回通知或异常通知 后置通知

- 切入点表达式细节：

  ![image-20230820152555882](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820152555882.png)

![image-20230820152741495](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820152741495.png)

### 3 重用切入点表达式

在其他切面类中使用PointCut时，要使用全类名

```java
@Before("com.atguigu.aop.CommonPointCut.pointCut()")
public void beforeMethod(JoinPoint joinPoint){
String methodName = joinPoint.getSignature().getName();
String args = Arrays.toString(joinPoint.getArgs());
System.out.println("Logger-->前置通知，方法名："+methodName+"，参数："+args);
```

## 6.获取通知相关信息

### 6.1 获取连接点相关信息

在通知方法参数内使用joinPoint

```java
@Before("execution(public int com.atguigu.aop.annotation.CalculatorImpl.*(..))")
public void beforeMethod(JoinPoint joinPoint){
//获取连接点的签名信息
String methodName = joinPoint.getSignature().getName();
//获取目标方法到的实参信息
String args = Arrays.toString(joinPoint.getArgs());
System.out.println("Logger-->前置通知，方法名："+methodName+"，参数："+args);
}
```

### 6.2 获取目标方法的返回值

@AfterReturning中的属性returning，用来将通知方法的某个形参，接收目标方法的返回值

```java
@AfterReturning(value = "execution(* com.atguigu.aop.annotation.CalculatorImpl.*
(..))", returning = "result")
public void afterReturningMethod(JoinPoint joinPoint, Object result){
String methodName = joinPoint.getSignature().getName();
System.out.println("Logger-->返回通知，方法名："+methodName+"，结果："+result);
}

```

### 6.3 获取目标方法的异常

@AfterThrowing中的属性throwing，用来将通知方法的某个形参，接收目标方法的异常

```java
@AfterThrowing(value = "execution(* com.atguigu.aop.annotation.CalculatorImpl.*
(..))", throwing = "ex")
public void afterThrowingMethod(JoinPoint joinPoint, Throwable ex){
String methodName = joinPoint.getSignature().getName();
System.out.println("Logger-->异常通知，方法名："+methodName+"，异常："+ex);
}

```

## 7.环绕通知

```java
@Around("execution(* com.atguigu.aop.annotation.CalculatorImpl.*(..))")
public Object aroundMethod(ProceedingJoinPoint joinPoint){
String methodName = joinPoint.getSignature().getName();
String args = Arrays.toString(joinPoint.getArgs());
Object result = null;
try {
System.out.println("环绕通知-->目标对象方法执行之前");
//目标方法的执行，目标方法的返回值一定要返回给外界调用者
result = joinPoint.proceed();
System.out.println("环绕通知-->目标对象方法返回值之后");
} catch (Throwable throwable) {
throwable.printStackTrace();
System.out.println("环绕通知-->目标对象方法出现异常时");
} finally {
System.out.println("环绕通知-->目标对象方法执行完毕");
}
return result;
}
```

## 8.切面优先级

![image-20230820154625466](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820154625466.png)

## 9.声明式事务

 JdbcTemplate相关内容

编程式事务：所有的功能都要自己编写代码实现

![image-20230820155442657](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820155442657.png)

声明式事务：

![image-20230820155608179](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820155608179.png)

### 9.1 基于注解的声明式事务’

1.导入依赖

```xml
<dependencies>
<!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-context</artifactId>
<version>5.3.1</version>
</dependency>
<!-- Spring 持久化层支持jar包 -->
<!-- Spring 在执行持久化层操作、与持久化层技术进行整合过程中，需要使用orm、jdbc、tx三个
jar包 -->
<!-- 导入 orm 包就可以通过 Maven 的依赖传递性把其他两个也导入 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-orm</artifactId>
<version>5.3.1</version>
</dependency>
<!-- Spring 测试相关 -->
<dependency>
<groupId>org.springframework</groupId>
<artifactId>spring-test</artifactId>
<version>5.3.1</version>
更多Java –大数据 – 前端 – UI/UE - Android - 人工智能资料下载，可访问百度：尚硅谷官网(www.atguigu.com)
②创建jdbc.properties
③配置Spring的配置文件
④创建表
</dependency>
<!-- junit测试 -->
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
</dependency>
<!-- MySQL驱动 -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.16</version>
</dependency>
<!-- 数据源 -->
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>druid</artifactId>
<version>1.0.31</version>
</dependency>
</dependencies>

```

2.开启事务注解驱动,要配置事务管理器（切面）注意：导入的名称空间需要 tx 结尾的那个。

```xml
<bean id="transactionManager"
class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="dataSource"></property>
</bean>
<!--
开启事务的注解驱动
通过注解@Transactional所标识的方法或标识的类中所有的方法，都会被事务管理器管理事务
-->
<!-- transaction-manager属性的默认值是transactionManager，如果事务管理器bean的id正好就
是这个默认值，则可以省略这个属性 -->
<tx:annotation-driven transaction-manager="transactionManager" />
```

3.在需要被事务管理的类或方法加上@Transactional注解，一般在service层加

4.事务属性：

- 只读

  对一个**查询操作**来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。这 样数据库就能够针对查询操作来进行优化。

```java
readOnly 默认值为false
```

- 超时

  事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源。而长时间 占用资源，大概率是因为程序运行出现了问题（可能是Java程序或MySQL数据库或网络连接等等）。 此时这个很可能出问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正常 程序可以执行。  **概括来说就是一句话：超时回滚，释放资源。**

```java
timeout 默认为-1 表示永不超时
    如果设置为正数，当超时时抛出异常
    
org.springframework.transaction.TransactionTimedOutException: Transaction timed out:
deadline was Fri Jun 04 16:25:39 CST 2022
```

- 回滚策略

  ![image-20230820205204120](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820205204120.png)

- 隔离级别

  幻读：一个事务同一查询执行多次，由于其他事务的**插入**操作，导致每次查询都返回不同的结果集。

  数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事 务与其他事务隔离的程度称为隔离级别。SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同 的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱。

  - 读未提交：一个事务能读另一个事务未提交的修改会产生脏读
  - 读已提交：一个事务只能读另一个事务已提交的修改，避免了脏读。但可能出现事务1进行查询操作，由于隔离级别是读已提交，在事务2提交了**删除或修改**操作，事务一能读到事务2提交后·的数据。要求情况下，只有事务一提交了事务后再次查询才能查到事务二进行的操作。此时发生了不可重复读。也就是事务同一次查询多次进行返回了不同的结果集。
  - 可重复读：解决了不可重复读。和幻读。
  - 串行化：在可重复读基础上加了锁。

![image-20230820210401479](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230820210401479.png)

- 使用方式

  ```java
  @Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别
  @Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交
  @Transactional(isolation = Isolation.READ_COMMITTED)//读已提交
  @Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读
  @Transactional(isolation = Isolation.SERIALIZABLE)//串行化
  ```

  

4.3.9、事务属性：事务传播行为 @Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别 @Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交 @Transactional(isolation = Isolation.READ_COMMITTED)//读已提交 @Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读 @Transactional(isolation = Isolation.SERIALIZABLE)//串行化 ①介绍 当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中 运行，也可能开启一个新事务，并在自己的事务中运行。 ②测试 创建接口CheckoutService： 创建实现类CheckoutServiceImpl： 在BookController中添加方法： 在数据库中将用户的余额修改为100元 ③观察结果 可以通过@Transactional中的propagation属性设置事务传播行为 修改BookServiceImpl中buyBook()上，注解@Transactional的propagation属性 @Transactional(propagation = Propagation.REQUIRED)，默认情况，表示如果当前线程上有已经开 启的事务可用，那么就在这个事务中运行。经过观察，购买图书的方法buyBook()在checkout()中被调 用，checkout()上有事务注解，因此在此事务中执行。所购买的两本图书的价格为80和50，而用户的余 额为100，因此在购买第二本图书时余额不足失败，导致整个checkout()回滚，即只要有一本书买不 了，就都买不了 public interface CheckoutService { void checkout(Integer[] bookIds, Integer userId); } @Service public class CheckoutServiceImpl implements CheckoutService { @Autowired private BookService bookService; @Override @Transactional //一次购买多本图书 public void checkout(Integer[] bookIds, Integer userId) { for (Integer bookId : bookIds) { bookService.buyBook(bookId, userId); } } } @Autowired private CheckoutService checkoutService; public void checkout(Integer[] bookIds, Integer userId){ checkoutService.checkout(bookIds, userId); } @Transactional(propagation = Propagation.REQUIRES_NEW)，表示不管当前线程上是否有已经开启 的事务，都要开启新事务。同样的场景，每次购买图书都是在buyBook()的事务中执行，因此第一本图 书购买成功，事务结束，第二本图书购买失败，只在第二次的buyBook()中回滚，购买第一本图书不受 影响，即能买几本就买几本

# SpringMVC

springmvc‘是表示层框架，使用springmvc之后就不用像学web一样自己写servlet了，springmvc提供一个DispatcherServlet帮我们拦截和转发请求

## 1.使用·springmvc

1. 创建web工程

2. 打包方式为war

3. 导入依赖

   1. 

   ```java
   dependencies>
   <!-- SpringMVC -->
   <dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.3.1</version>
   </dependency>
   <!-- 日志 -->
   <dependency>
   <groupId>ch.qos.logback</groupId>
   <artifactId>logback-classic</artifactId>
   <version>1.2.3</version>
   </dependency>
   <!-- ServletAPI -->
   <dependency>
   <groupId>javax.servlet</groupId>
   <artifactId>javax.servlet-api</artifactId>
   <version>3.1.0</version>
   <scope>provided</scope>
   </dependency>
   <!-- Spring5和Thymeleaf整合包 -->
   <dependency>
   <groupId>org.thymeleaf</groupId>
   <artifactId>thymeleaf-spring5</artifactId>
   <version>3.0.12.RELEASE</version>
   </dependency>
   </dependencies>
   
   ```

   4.在web.xml中配置DispatcherServlet

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
            version="4.0">
       <servlet>
           <servlet-name>SpringMVC</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:springmvc.xml</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
       <servlet-mapping>
           <servlet-name>SpringMVC</servlet-name>
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   ```

   可以设置init-param属性contextConfigLocation来指定springmvc配置文件位置，如果不指定默认在 [WEB-INF](..\..\..\ideahbw\SSM1\spring_mvc\src\main\webapp\WEB-INF) 目录下

   通过load-on-startup设置值为1，可以把DispatcherServlet初始化放在服务器启动时。

>
>
>标签中使用/和/*的区别： /所匹配的请求可以是/login或.html或.js或.css方式的请求路径，但是/不能匹配.jsp请求路径的请 求 因此就可以避免在访问jsp页面时，该请求被DispatcherServlet处理，从而找不到相应的页面 /*则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用/*的写 法

5.创建springmvc配置文件

```xml
<!-- 自动扫描包 -->
<context:component-scan base-package="com.atguigu.mvc.controller"/>
<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver"
class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
<property name="order" value="1"/>
<property name="characterEncoding" value="UTF-8"/>
<property name="templateEngine">
<bean class="org.thymeleaf.spring5.SpringTemplateEngine">
<property name="templateResolver">
<bean
class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
<!-- 视图前缀 -->
<property name="prefix" value="/WEB-INF/templates/"/>
<!-- 视图后缀 -->
<property name="suffix" value=".html"/>
<property name="templateMode" value="HTML5"/>
<property name="characterEncoding" value="UTF-8" />
</bean>
</property>
</bean>
</property>
</bean>
<!--
处理静态资源，例如html、js、css、jpg
若只设置该标签，则只能访问静态资源，其他请求则无法访问

此时必须设置<mvc:annotation-driven/>解决问题
-->
<mvc:default-servlet-handler/>
<!-- 开启mvc注解驱动 -->
<mvc:annotation-driven>
<mvc:message-converters>
<!-- 处理响应中文内容乱码 -->
<bean
class="org.springframework.http.converter.StringHttpMessageConverter">
<property name="defaultCharset" value="UTF-8" />
<property name="supportedMediaTypes">
<list>
<value>text/html</value>
<value>application/json</value>
</list>
</property>
</bean>
</mvc:message-converters>
</mvc:annotation-driven
```

<mvc:default-servlet-handler/>用于访问静态资源，使用它必须开启mvc注解驱动，否则其他请求无法访问

6.创建controller

@RequestMapping处理请求和控制器方法之间的映射关系，用来标识一个处理请求方法，value属性表示处理请求的路径

总结

>
>
>浏览器发送请求，若请求地址符合前端控制器的url-pattern，请求会被DispatcherServlet拦截，然后读取springmvc配置文件，扫描组件来获得controller组件，然后根据请求路径匹配控制器方法处理请求。该请求方法会返回一个字符串类型的视图名称，该视图会被themyleaf解析加上前缀和后缀，然后若请求地址符合前端控制器的url-pattern对视图进行渲染，然后跳转到view页面

## 2.详解@RequestMapping注解

### 2.1 @RequestMapping作用

建立请求和控制器方法之间的映射关系

### 2.2 @RequestMapping标识的位置

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息

 @RequestMapping标识一个方法：设置映射请求请求路径的具体信息

### 2.3 @RequestMapping value属性

 @RequestMapping value属性表示使用请求路径来匹配该请求映射

value是一个字符串数组，表示该请求映射可以匹配多个请求路径

@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射

```java
<a th:href="@{/testRequestMapping}">测试@RequestMapping的value属性--
>/testRequestMapping</a><br>
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>

@RequestMapping(
value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
return "success";
}
```

### 2.4 @RequestMapping method属性

@RequestMapping method属性表示使用请求的方式来匹配该请求映射

method属性也是一个数组，表示该请求映射可以匹配多个请求方式

若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错 405：Request method 'POST' not supported

```java
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
<input type="submit">
</form>
    
    
@RequestMapping(
value = {"/testRequestMapping", "/test"},
method = {RequestMethod.GET, RequestMethod.POST}
)
public String testRequestMapping(){
return "success";
}

```

>
>
>注： 1、对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解 
>
>处理get请求的映射-->@GetMapping 
>
>处理post请求的映射-->@PostMapping 
>
>处理put请求的映射-->@PutMapping
>
> 处理delete请求的映射-->@DeleteMapping
>
> 2、常用的请求方式有get，post，put，delete 但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符 串（put或delete），则按照默认的请求方式get处理 若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter，在 RESTful部分会讲到

### 2.5 SpringMVC支持ant风格的路径

？：表示任意的单个字符 

*：表示任意的0个或多个字符

/** **：表示任意层数的任意目录 注意：在使用**时，只能使用/**/xxx的方式

### 2.6  SpringMVC支持接受请求路径中的占位符（重点）

原始方式：/deleteUser?id=1

 rest方式：/user/delete/1

占位符常用在restful风格中，可以在@RequestMapping value属性中使用{}来表示路径中的参数，并且使用@PathVarible注解将{}占位符中的值赋值给请求映射方法中的参数

```java
 @RequestMapping("/test/{id}")
    public String param( @PathVariable("id") Integer id){
        System.out.println("id="+id);
        return "success";
    }
```

## 3. SpringMVC获取请求参数的方法

### 3.1 用servlet API（不推荐）

>
>
>将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请 求的请求报文的对象

```java
@RequestMapping("/testParam")
public String testParam(HttpServletRequest request){
String username = request.getParameter("username");
String password = request.getParameter("password");
System.out.println("username:"+username+",password:"+password);
return "success";
}
```

### 3.2 使用控制器方法形参直接获取请求参数

>
>
>把控制器方法形参设置成和请求参数名称一致，就可以接收到请求参数。当浏览器发送请求，匹配到请求映射时，在 DispatcherServlet中就会将请求参数赋值给相应的形参

```java
 @RequestMapping("/test/param")
    public String params(String username,String password){
        System.out.println("用户名为"+username+"密码为"+password);
        return "success";
    }
```

>
>
>注： 
>
>若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串 数组或者字符串类型的形参接收此请求参数 
>
>若使用字符串数组类型的形参，此参数的数组中包含了每一个数据 若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果

### 3.3 使用@RequestParam注解

>@RequestParam注解为请求参数和控制器方法的形参创建映射关系
>
>@RequestParam注解有三个属性
>
>value属性用来设置和控制器方法映射的请求参数名
>
>required属性用来设置是否必须传输此请求参数，默认是true
>
>若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置 defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；若设置为 false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为 null
>
>defaultValue：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值 为""时，则使用默认值为形参赋值

```java
@Target({ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {
    @AliasFor("name")
    String value() default "";

    @AliasFor("value")
    String name() default "";

    boolean required() default true;

    String defaultValue() default "\n\t\t\n\t\t\n\ue000\ue001\ue002\n\t\t\t\t\n";

```

### 3.3 使用@RequestHeader注解获取请求头信息

>@RequestHeader注解为请求头信息和控制器方法形参创建映射
>
>@RequestHeader注解有四个属性，和@RequestParam注解用法一致
>
>```java
>@Target({ElementType.PARAMETER})
>@Retention(RetentionPolicy.RUNTIME)
>@Documented
>public @interface RequestHeader {
>    @AliasFor("name")
>    String value() default "";
>
>    @AliasFor("value")
>    String name() default "";
>
>    boolean required() default true;
>
>    String defaultValue() default "\n\t\t\n\t\t\n\ue000\ue001\ue002\n\t\t\t\t\n";
>}
>```

### 3.4 使用@CookieValue注解获取cookie信息

>@CookieValue注解为Cookie数据和控制器方法形参建立映射关系
>
>@CookieValue注解有四个属性，和@RequestParam注解用法一致
>
>```java
>@Target({ElementType.PARAMETER})
>@Retention(RetentionPolicy.RUNTIME)
>@Documented
>public @interface CookieValue {
>    @AliasFor("name")
>    String value() default "";
>
>    @AliasFor("value")
>    String name() default "";
>
>    boolean required() default true;
>
>    String defaultValue() default "\n\t\t\n\t\t\n\ue000\ue001\ue002\n\t\t\t\t\n";
>}
>```
>
>

### 3.5 使用实体类来获取请求参数

>如果请求参数名和实体类属性名一致，则可以在控制器方法形参用实体类接受请求参数，此种多用于请求参数多个的时候。比如表单提交数据。
>
>```
><a th:href="@{/jump}">跳转</a>
><form th:action="@{/test/pojo}" method="post">
>     用户名：<input type="text" name="username"></br>
>     密码：<input type="password" name="password"></br>
>     登录：<input type="submit" ></br>
></form>
>```
>
>```java
>}
>@RequestMapping("/test/pojo")
>public String testGetParamByPojo(User user){
>    System.out.println(user);
>    return "success";
>
>}
>```

### 3.6 解决请求参数乱码问题

解决获取请求参数的乱码问题，可以使用SpringMVC提供的编码过滤器CharacterEncodingFilter，但是 必须在web.xml中进行注册

```xml
 <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

**注意**：

>
>
>**SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效**

## 4. 域对象共享数据

往Request域中

### 4.1 使用servlrtAPI方法(没人用)

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request){
request.setAttribute("testScope", "hello,servletAPI");
return "success";
}
```

### 4.2 使用ModelAndView

```java
 @RequestMapping("/test/mav")
    public ModelAndView mav(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("name","贺博文");
        modelAndView.setViewName("success");
        return modelAndView;
    }
```

> 
>
> ModelAndView有Model和View的功能 * Model主要用于向请求域共享数据 * View主要用于设置视图，实现页面跳转 */
>
> 之前的写法虽然返回的是view名字的字符串对象，但是最后都会被封装成ModelAndView对象

### 4.3 使用Model（常用）

```java
 @RequestMapping("/test/model")
    public String teetModel(Model model){
        //org.springframework.validation.support.BindingAwareModelMap
        System.out.println(model.getClass().getName());
        model.addAttribute("name","model");
        return "success";
    }
```

### 4.4 使用map

```java
 @RequestMapping("/test/map")
    public String testMap(Map<String,Object> map){
       // org.springframework.validation.support.BindingAwareModelMap
        System.out.println(map.getClass().getName());
        map.put("name","map");
        return "success";
    }
```

### 4.5 使用modelMap

```java
@RequestMapping("/test/modelmap")
    public String testModelMap(ModelMap modelMap){
        //org.springframework.validation.support.BindingAwareModelMap
        System.out.println(modelMap.getClass().getName());
        modelMap.addAttribute("name","modelmap");
        return "success";
    }
```

### 4.6 model、modelMap、map关系

>
>
>Model、ModelMap、Map类型的参数其实本质上都是 BindingAwareModelMap 类型的

```java
public interface Model{}
public class ModelMap extends LinkedHashMap<String, Object> {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class BindingAwareModelMap extends ExtendedModelMap {}
```

### 4.7 往Session域中共享数据

```java
@RequestMapping("/test/session")
    public String testSession(HttpSession httpSession){
       httpSession.setAttribute("name","session");
        return "success";
    }
```

### 4.8 往application域（servletContext域）中共享数据

```java
 @RequestMapping("/test/application")
    public String testApplication(HttpSession httpSession){
        ServletContext servletContext = httpSession.getServletContext();
        servletContext.setAttribute("name","application");
        return "success";
    }
```

##  5.SpringMVC的视图

SpringMVC中的视图是View接口，**视图的作用渲染数据，将模型Model中的数据展示给用户** .SpringMVC视图的种类很多，默认有**转发视图和重定向视图** 当工程引入jstl的依赖，转发视图会自动转换为JstlView 若使用的视图技术为Thymeleaf，在SpringMVC的配置文件中配置了Thymeleaf的视图解析器，由此视 图解析器解析之后所得到的是ThymeleafView

### 5.1 ThymeleafView

当控制器方法中所设置的视图名称没有任何前缀和后缀时，此时的视图名称会被SpringMVC配置文件中所配置 的视图解析器解析，视图名称拼接视图前缀和视图

后缀所得到的最终路径，会通过**转发**的方式实现跳转

```java
 @RequestMapping("/test/view")
    public String testViewResolver(){
        return "success";
    }
```



debug时，调用关系是从下往上的，当源码很复杂时，可以多打几个断点

![image-20230901140054129](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901140054129.png)

控制器方法最终返回都会被封装到modelandview中

下面是验证源码

1.此方法间接调用了控制器方法，最终返回一个modelandview对象

![image-20230901141708049](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901141708049.png)

2、

![image-20230901141903757](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901141903757.png)

视图解析源码

1.执行DispatcherServlet中执行控制器方法

![image-20230901144215862](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901144215862.png)

2.执行控制器方法

![image-20230901144308064](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901144308064.png)

3.执行DispatcherServle中处理调度结果的方法

![image-20230901144614343](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901144614343.png)

4.进入到这个方法内，这个方法经过一系列判断最终又会执行渲染方法

![image-20230901144952007](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901144952007.png)

5.进入到渲染方法内，这个方法首先会得到view名称，所以说用什么视图解析器取决于视图名称

![image-20230901145208080](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901145208080.png)

5.接下来就会调用视图解析方法

![image-20230901145329087](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901145329087.png)

6.进入视图解析方法内最终会发现是thymeleaf视图

![image-20230901145712159](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901145712159.png)

### 5.2 重定向视图

而控制器方法返回的是重定向的方式的话最终创建视图是重定向视图，而是会将前缀"redirect:"去掉，剩余部分作为最 终路径通过重定向的方式实现跳转（(相当于再发一次请求，还会调用一次DispatcherServlet中执行控制器方法)

重定向形式

```java
@RequestMapping("/test/redirect")
    public String testRedirect(){
        return "redirect:/test/view";
    }
```



![image-20230901151049218](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901151049218.png)

### 5.3 转发视图



如果控制器返回的是转发请求，则最终创建的是**InternalResourceView**,此时的视图名称不 会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"forward:"去掉，剩余部 分作为最终路径通过转发的方式实现跳转(相当于再发一次请求，还会调用一次DispatcherServlet中执行控制器方法)

转发视图

```java

    @RequestMapping("/test/forward")
    public String testForward(){
        return "forward:/test/view";

    }
```



![image-20230901152946754](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901152946754.png)

### 5.4 视图控制器view-Controller

当一个控制器方法仅仅是跳转页面，我们就可以把他配置在springMVC核心配置文件中

```xml
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
<mvc:view-controller path="/to/add" view-name="addEmployee"></mvc:view-controller>

 <mvc:annotation-driven></mvc:annotation-driven>
```

注意使用了视图控制器一定要开启mvc注解驱动，否则其他请求映射方法将会不可用。

## 6 Restful风格(重点)

万物皆资源。

REST：**Re**presentational **S**tate **T**ransfer，表现层资源状态转移。

### 6.1 资源

**资源是一种看待服务器的方式**，即，将服务器看作是由很多离散的资源组成。每个资源是服务器上一个 可命名的抽象概念。因为资源是一个抽象的概念，所以它不仅仅能代表服务器文件系统中的一个文件、 数据库中的一张表等等具体的东西，可以将资源设计的要多抽象有多抽象，只要想象力允许而且客户端 应用开发者能够理解。**与面向对象设计类似**，资源是以名词为核心来组织的，首先关注的是名词。一个 资源可以由一个或多个URI来标识。URI既是资源的名称，也是资源在Web上的地址。对某个资源感兴 趣的客户端应用，可以通过资源的URI与其进行交互。

### 6.2 资源的表述

资源的表述是一段对于资源在某个特定时刻的状态的描述。可以在客户端-服务器端之间转移（交 换）。资源的表述可以有多种格式，**例如HTML/XML/JSON/纯文本/图片/视频/音频等等**。资源的表述格 式可以通过协商机制来确定。请求-响应方向的表述通常使用不同的格式。

### 6.3 状态转移

状态转移说的是：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资 源的表述，来间接实现操作资源的目的。

### 6.4 Restful实现

**REST 风格提倡 URL 地址使用统一的风格设计**，把所有都放在请求路径中，**对某个资源操作就把该资源名和参数都放在请求URL中以/分隔。用不同的请求方式来区分对资源的不同操作**操作方式在HTTP协议中有四种，分别是Post（代表对资源进行提交操作例如增加记录）、Get（代表对资源进行获取操作例如查询）、Delete（对资源进行删除操作）、Put（对资源进行修改）、从前到后各个单词使用斜杠分开，不使用问号键值对方 式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。

### 6.5 HiddenHttpMethodFilter

由于浏览器只支持发送get和put请求，所以要想发put和delete请求，则要使用SpringMVC提供的HiddenHttpMethodFilter过滤器帮助我们将 POST 请求转换为 DELETE 或 PUT 请求

```java
 <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

HiddenHttpMethodFilter过滤器将 POST 请求转换为 DELETE 或 PUT 请求的条件

- 表单请求方式必须为post
- **需要发送名字为_method的参数**需要设置一个隐藏输出框，type为Hidden ，name为_method,value为真正的请求方式

```html
<form th:action="@{/user}" method="post">
    <input type="hidden" name="_method" value="put">
    用户名:<input type="text" name="username" ><br>
    密码: <input type="password" name="password"><br>
    修改：<input type="submit"><br>
</form>
```

满足以上条件，HiddenHttpMethodFilter 过滤器就会将当前请求的请求方式转换为请求参数 __method的值，因此请求参数_method的值才是最终的请求方式

注：

>
>
>目前为止，SpringMVC中提供了两个过滤器：
>
>CharacterEncodingFilter和 HiddenHttpMethodFilter 在web.xml中注册时，必须先注册CharacterEncodingFilter，再注册HiddenHttpMethodFilter 原因：
>
>-  在 CharacterEncodingFilter 中通过 request.setCharacterEncoding(encoding) 方法设置字 符集的
>
>-  request.setCharacterEncoding(encoding) 方法要求前面不能有任何获取请求参数的操作 
>
>- 而 HiddenHttpMethodFilter 恰恰有一个获取请求参数的操作：
>
>- ```java
>  String paramValue = request.getParameter(this.methodParam);
>  
>  ```
>
>  

### 6.6 实战

此部分可以看尚硅谷笔记

# 7.SpringMVC处理ajax请求

# 8. 文件上传和下载

## 8.1 文件下载(从服务器下载到主机)

ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文

使用ResponseEntity实现下载文件的功能

```java
@RequestMapping("/test/load")
    public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws
            IOException {
//获取ServletContext对象
        ServletContext servletContext = session.getServletContext();
//获取服务器中文件的真实路径
        String realPath = servletContext.getRealPath("img");
        realPath = realPath + File.separator + "1.jpg";
//创建输入流
        InputStream is = new FileInputStream(realPath);
//创建字节数组
        byte[] bytes = new byte[is.available()];
//将流读到字节数组中
        is.read(bytes);
//创建HttpHeaders对象设置响应头信息
        MultiValueMap<String, String> headers = new HttpHeaders();
//设置要下载方式以及下载文件的名字
        headers.add("Content-Disposition", "attachment;filename=1.jpg");
//设置响应状态码
        HttpStatus statusCode = HttpStatus.OK;
//创建ResponseEntity对象
        ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers,
                statusCode);
//关闭输入流
        is.close();
        return responseEntity;
    }
```

## 8.2 文件上传（从主机浏览器上传到服务器）

文件上传要求form表单的请求方式必须为post，并且添加属性enctype="multipart/form-data" **SpringMVC中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息** 上传步骤： 

①添加依赖：

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload --
>
<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.3.1</version>
</dependency>
```

②在SpringMVC的配置文件中添加配置：

```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver"
class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```



```java
@RequestMapping(value = "/test/up", method = RequestMethod.POST)
    public String fileUpload(MultipartFile photo, HttpSession session) throws IOException {
        String fileName = photo.getOriginalFilename();
        //获取文件名后缀
        String substring = fileName.substring(fileName.lastIndexOf("."));
        //使用uuid
        UUID uuid = UUID.randomUUID();
        //拼接uuid和后缀获得新fileName
        fileName = uuid+substring;
        ServletContext servletContext = session.getServletContext();
        String photoPath = servletContext.getRealPath("photo");
        File file = new File(photoPath);
        if (!file.exists()) {
            file.mkdir();
        }
        String finalPath = photoPath + File.separator + fileName;
        photo.transferTo(new File(finalPath));
        return "success";

    }
```



**上传和下载作为模板，会用即可**





# 	10.拦截器

### 10.1 使用拦截器

自定义拦截器必须实现HandlerInterceptor接口

并且要在SpringMVC核心配置文件中配置<bean>把自定义拦截器注入到容器中

还要配置<mvc:interceptor>用来配置拦截器的拦截路径和排除路径等。

```xml
<!--    把自定义的拦截器添加到容器内-->
    <bean id="FirstInterceptor" class="com.hbw.interceptor.FirstInterceptor"></bean>
    <bean id="SecondInterceptor" class="com.hbw.interceptor.SecondInterceptor"></bean>
<!--    设置拦截器的拦截路径、排除路径等-->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/index"/>
            <mvc:exclude-mapping path="/"/>
            <ref bean="FirstInterceptor"></ref>
        </mvc:interceptor>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/index"/>
            <mvc:exclude-mapping path="/"/>
            <ref bean="SecondInterceptor"></ref>
        </mvc:interceptor>
    </mvc:interceptors>
```

配置完这些拦截器就能工作了

### 10.2 拦截器方法执行顺序

拦截器有preHandle、postHandle和 afterCompletion方法

>preHandle在控制器执行之前调用，用返回值来判定是否拦截，返回值true表示放行，返回值false表示拦截

```java
 @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("这是第二个拦截器preHandle方法");
        return true;
    }
```

postHandle在控制器方法执行之后执行

```java

@Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("这是第二个拦截器postHandle方法");
    }
```

afterCompletion处理完视图和模型数据，渲染视图完毕之后执行afterCompletion

```java
@Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("这是第二个拦截器afterCompletion方法");
    }
```

### 10.4 多个拦截器调用顺序

interCeptorList中有mvc自带的拦截器和自定义的拦截器

![image-20230902114322114](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902114322114.png)

> **如果preHandle全部放行**，那么执行顺序为：preHandle按照拦截器在核心配置文件中配置正向顺序执行，而postHandle和afterCompletion按照拦截器在核心配置文件中配置反向顺序执行

原因：源码解析、

注意：这个mapperHander 是HanderExecutionChain，它是带着控制器方法和三个拦截器的。



**而getHandler方法本质上就是调用HandlerMapping来获取Handler相关信息和拦截器封装到**HanderExecutionChain中

![image-20230903113632627](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903113632627.png)

1.拦截器工作，首先执行if。如果applyPreHandle  返回false，直接return，拦截住请求不在执行控制器方法。如果applyPreHandle  返回true直接放行

![image-20230901195145307](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901195145307.png)

2.进入到方法内，applyPreHandle  方法从0开始for循环遍历并取出interceptorlist，也就是正向取出interceptor，然后执行interceptor的preHandle方法，如果返回值为true，则不执行if中语句，并且执行下一个拦截器的preHandle方法，并且继续判断返回值，返回true依旧不执行if内方法，以此重复，直到遍历完成或者遍历取出的拦截器preHandle方法返回false（此时会执行if内语句，并且最后直接返回false退出方法），此时interCeptorIndex值是2.只有所有拦截器的preHandle方法都返回true，applyPreHandle才会返回一个true。然后拦截器放行。

![image-20230901195342690](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901195342690.png)

3.执行Handle方法，这个ha就是HandlerAdapter，用于执行控制器方法

![image-20230903105917356](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903105917356.png)

![image-20230901200613474](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901200613474.png)

4.执行applyPostHandle

![image-20230901200647157](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901200647157.png)

5.进入applyPostHandle方法内，applyPostHandle方法内依旧是对interceptorlist进行遍历，**只不过是从interceptorlist最后一个元素开始遍历**，遍历取出拦截器后，在执行它的postHandle方法

**这也就解释了为什么PostHandle方法是按拦截器配置顺序相反的顺序执行。**

![image-20230901200754376](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901200754376.png)

6.执行triggerAfterCompletion方法

![image-20230901201627593](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230901201627593.png)

7.进入方法内部，triggerAfterCompletion方法内部同样是for循环遍历，但是从interCeptorIndex开始遍历i。interCeptorIndex现在值为2，然后取出i对应的拦截器，执行拦截器的afterCompletion方法，这也解释了为什么afterCompletion方法是按照配置顺序反序执行。

![image-20230902111438583](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902111438583.png)







>**如果某个拦截器preHandle方法返回false**，那**么它和它之前的preHandle方法**都会执行，preHandle按照配置顺序执行。**它之前得afterCompletion方法**会执行afterCompletion按照配置反向执行

原理：观察源码

1..拦截器工作，首先执行if。如果applyPreHandle  返回false，直接return，拦截住请求不在执行控制器方法。如果applyPreHandle  返回true直接放行

![image-20230902112726959](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902112726959.png)

2.执行applyPreHandle 方法，从interCeptorList第一个元素开始遍历并取出interCeptorList中元素，然后执行拦截器的peHandle方法然后进行一个if判断，如果

peHandle方法返回true,把i赋值给interCeptorIndex,然后继续遍历取出下一个拦截器。

如果peHandle方法返回false,会执行if内语句，执行triggerAfterCompletion方法，并且返回false

第一次取出的是mvc自带的拦截器，该拦截器preHandle方法返回的是true，所以直接执行this.interceptorIndex = i语句，此时interceptorIndex 为0。

第二次取出的是自定义的拦截器返回false，进入到if语句内。

![image-20230902113434019](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902113434019.png)

3.进入到triggerAfterCompletion方法内，从interCeptorIndex开始遍历并取出interCeptorList中的元素，此时interCeptorIndex为0，然后执行拦截器（mvc得拦截器）的afterComletion方法，i--，此时i=-1，跳出循环。

![image-20230902113823654](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902113823654.png)

4.返回false，if语句条件成立，return，请求被拦截。

![image-20230902115507074](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902115507074.png)

## 12 注解方式配置springmvc



# 13 SpringMVC执行流程

## 13.1 SpringMVC常用组件

- DispatcherServlet：**前端控制器**，不需要自己开发，由框架提供。

  作用：统一处理请求和响应，整个流程控制的中心，由它调用其它组件处理用户的请求

- HandlerMapping：**处理器映射器**不需要自己开发，由框架提供

  作用：根据请求的url、method等信息**匹配**Handler，即控制器方法

- HandlerAdapter：**处理器适配器**，不需要自己开发，由框架提供。

  作用：在处理器映射器找到控制器方法后，由处理器适配器执行控制器方法，在源码中就是那个ha

- handler：**处理器**即控制器方法，需要自己开发

  作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理

- ViewResolver：**视图解析器**，不需要自己开发，由框架提供

  作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、 RedirectView

- View：**视图**，需要自己开发

  作用：将模型数据通过页面展示给用户

## 13.2 DisPatcherServlet初始化

DisPatcherServlet继承关系如下图

![image-20230902162031767](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902162031767.png) 

DisPatcherServlet本身也是个servlet，初始化也是init方法，但是并没有在DisPatcherServlet中发现init方法，从顶层父接口init方法开始分析

- 顶层servlet接口init方法是有参的

![image-20230902162526163](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902162526163.png)

- 在GenericServlet 中这个有参init方法进行了重写，调用了无参init方法

  ![image-20230902162706192](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902162706192.png)

![image-20230902162802841](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902162802841.png)

- 在HttpServletBean中重写了init方法，该方法会调用initServletBean方法
- ![image-20230902163006552](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163006552.png)

![image-20230902163055433](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163055433.png)

- 在FrameworkServlet中重写了initSevletBean方法，这个方法中又调用了initWebApplicationContext()方法，这个方法是初始化springmvc的IOC容器

  ![image-20230902163302385](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163302385.png)

- 在initWebApplicationContext()方法中最终又会执行onRefresh方法

  ![image-20230902163514185](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163514185.png)

- DisPatcherServlet重写了onRefresh方法，在这个方法中又会调用initStrategies方法

  ![image-20230902163700353](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163700353.png)



- 在这个方法中才是DisPatcherServlet真正初始化各个组件的地方

  ![image-20230902163727446](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902163727446.png)

### 13.3 DisPatcherServlet处理请求的过程

由于.DisPatcherServlet本身也是个servlet，那么他的处理请求也应该从service方法开始，发现DisPatcherServlet并没有service方法，那么就从他的顶级父接口Servlet开始分析

- servlet中有service方法

  ![image-20230902165043055](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902165043055.png)

- 在HttpServlet中重写了Service方法

  ```java
  protected void service(HttpServletRequest req, HttpServletResponse resp)
          throws ServletException, IOException
      {
          String method = req.getMethod();
  
          if (method.equals(METHOD_GET)) {
              long lastModified = getLastModified(req);
              if (lastModified == -1) {
                  // servlet doesn't support if-modified-since, no reason
                  // to go through further expensive logic
                  doGet(req, resp);
              } else {
                  long ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
                  if (ifModifiedSince < lastModified) {
                      // If the servlet mod time is later, call doGet()
                      // Round down to the nearest second for a proper compare
                      // A ifModifiedSince of -1 will always be less
                      maybeSetLastModified(resp, lastModified);
                      doGet(req, resp);
                  } else {
                      resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
                  }
              }
  
          } else if (method.equals(METHOD_HEAD)) {
              long lastModified = getLastModified(req);
              maybeSetLastModified(resp, lastModified);
              doHead(req, resp);
  
          } else if (method.equals(METHOD_POST)) {
              doPost(req, resp);
              
          } else if (method.equals(METHOD_PUT)) {
              doPut(req, resp);
              
          } else if (method.equals(METHOD_DELETE)) {
              doDelete(req, resp);
              
          } else if (method.equals(METHOD_OPTIONS)) {
              doOptions(req,resp);
              
          } else if (method.equals(METHOD_TRACE)) {
              doTrace(req,resp);
              
          } else {
              //
              // Note that this means NO servlet supports whatever
              // method was requested, anywhere on this server.
              //
  
              String errMsg = lStrings.getString("http.method_not_implemented");
              Object[] errArgs = new Object[1];
              errArgs[0] = method;
              errMsg = MessageFormat.format(errMsg, errArgs);
              
              resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
          }
      }
  ```

  在这个service方法中，根据不同的请求类型执行不同的·Doxxx方法

  

- 在FrameworkServlet重写了各个Doxxx方法，在这些方法中都执行了processRequest(request, response）方法

  

  ![image-20230902165520663](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902165520663.png)

- 而在processRequest方法内又执行了doService方法

  ![image-20230902165805768](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902165805768.png)

![image-20230902165826304](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902165826304.png)

- DisPatcherServlet重写了Doservice方法，在这个方法中又执行了doDisPatch方法

  ![image-20230902170005040](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230902170005040.png)

- doDisPatch方法是真正处理请求的方法

  ```java
  protected void doDispatch(HttpServletRequest request, HttpServletResponse
  response) throws Exception {
  HttpServletRequest processedRequest = request;
  HandlerExecutionChain mappedHandler = null;
  boolean multipartRequestParsed = false;
  WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
  try {
  ModelAndView mv = null;
  Exception dispatchException = null;
  try {
  processedRequest = checkMultipart(request);
  multipartRequestParsed = (processedRequest != request);
  // Determine handler for the current request.
  /*
  mappedHandler：调用链
  包含handler、interceptorList、interceptorIndex
  handler：浏览器发送的请求所匹配的控制器方法
  interceptorList：处理控制器方法的所有拦截器集合
  interceptorIndex：拦截器索引，控制拦截器afterCompletion()的执行
  */
  mappedHandler = getHandler(processedRequest);
  if (mappedHandler == null) {
  noHandlerFound(processedRequest, response);
  return;
  }
  // Determine handler adapter for the current request.
  // 通过控制器方法创建相应的处理器适配器，调用所对应的控制器方法
  HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
  // Process last-modified header, if supported by the handler.
  String method = request.getMethod();
  boolean isGet = "GET".equals(method);
  if (isGet || "HEAD".equals(method)) {
  long lastModified = ha.getLastModified(request,
  mappedHandler.getHandler());
  if (new ServletWebRequest(request,
  response).checkNotModified(lastModified) && isGet) {
  return;
  }
  }
  // 调用拦截器的preHandle()
  if (!mappedHandler.applyPreHandle(processedRequest, response)) {
  return;
  }
  更多Java –大数据 – 前端 – UI/UE - Android - 人工智能资料下载，可访问百度：尚硅谷官网(www.atguigu.com)
  ④processDispatchResult()
  // Actually invoke the handler.
  // 由处理器适配器调用具体的控制器方法，最终获得ModelAndView对象
  mv = ha.handle(processedRequest, response,
  mappedHandler.getHandler());
  if (asyncManager.isConcurrentHandlingStarted()) {
  return;
  }
  applyDefaultViewName(processedRequest, mv);
  // 调用拦截器的postHandle()
  mappedHandler.applyPostHandle(processedRequest, response, mv);
  }
  catch (Exception ex) {
  dispatchException = ex;
  }
  catch (Throwable err) {
  // As of 4.3, we're processing Errors thrown from handler methods as
  well,
  // making them available for @ExceptionHandler methods and other
  scenarios.
  dispatchException = new NestedServletException("Handler dispatch
  failed", err);
  }
  // 后续处理：处理模型数据和渲染视图
  processDispatchResult(processedRequest, response, mappedHandler, mv,
  dispatchException);
  }
  catch (Exception ex) {
  triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
  }
  catch (Throwable err) {
  triggerAfterCompletion(processedRequest, response, mappedHandler,
  new NestedServletException("Handler processing
  failed", err));
  }
  finally {
  if (asyncManager.isConcurrentHandlingStarted()) {
  // Instead of postHandle and afterCompletion
  if (mappedHandler != null) {
  mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
  }
  }
  else {
  // Clean up any resources used by a multipart request.
  if (multipartRequestParsed) {
  cleanupMultipart(processedRequest);
  }
  }
  }
  }
  ```

  ## 

## 13.4 SpringMVC工作流程

注意：请求映射应该是包括控制器方法和视图

1. 请求进来首先会被DisPatcherServlet捕获，DisPatcherServlet会对请求url进行解析，得到请求资源标识符URI，根据URI来判断是否有对应的请求映射

   如果没有

   1. 判断是否配置了默认的Servlet

   2. 如果没配置，控制台报找不到对应的请求映射错误，会响应给浏览器404提示

      ![image-20230903112202330](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903112202330.png)

      3.如果配置了默认的Servlet，则访问目标资源（一般为静态资源，如：JS,CSS,HTML），如果找不到，依然会响应浏览器404

​        ![image-20230903112807941](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903112807941.png)

​       如果有对应的请求映射，则会执行以下步骤

​    1.DisPatcherServlet调用HandleMapping来获得Handler相关信息和拦截器等等，通过HanderExecutionChain对象形式返回

![image-20230903104322780](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903104322780.png)

getHandler方法内部就是调用HandlerMapping来获取Handler相关信息和拦截器的，

![image-20230903113632627](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903113632627.png)

2.DispatcherServlet根据得到的Handler相关信息来匹配一个适合的HandlerAdapter

![image-20230903142916365](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903142916365.png)

3.执行拦截器的preHandle方法（正序）

![image-20230903143019260](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903143019260.png)

4.提取Request中的模型数据，填充Handler形参，开始执行Handler（Controller)方法，处理请求。

![image-20230903143355579](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903143355579.png)

5.Handler方法执行完毕会返回给DispatcherServlet一个ModelAndView对象

6.然后执行拦截器的postHandle方法

![image-20230903143558718](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903143558718.png)

7.根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行 HandlerExceptionResolver进行异常处理）选择一个适合的ViewResolver进行视图解析，根据Model 和View，来渲染视图。

![image-20230903143934035](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903143934035.png)

![image-20230903144020013](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903144020013.png)

![image-20230903144054393](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903144054393.png)



![image-20230903144120434](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903144120434.png)

8.渲染视图完毕执行拦截器的afterCompletion(…)方法【逆向】。

![image-20230903144306761](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230903144306761.png)

9. 将渲染结果返回给客户端。

# SSM整合

# 1. ContextLoaderListener

由于controller层需要用到service层，所以spring配置文件要比SpringMVC配置文件提前读取。所以要用到ContextLoaderListener监听器。ContextLoaderListener用于监听servletContext，servletContext一般在服务器启动时就初始化了，所以可以配置个ContextLoaderListener，这样就可以在服务器启动时就加载了spring的核心配置文件,创建了spring的IOC容器。使用 ContextLoaderListener必须在web.xml中配置

```xml
<!--    设置servletContext监听器，在服务器启动时获取spring配置文件-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

<!--    设置spring配置文件的位置。Spring配置文件默认位置和名称：/WEB-INF/applicationContext.xml
可通过上下文参数自定义Spring配置文件的位置和名称
-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring.xml</param-value>
    </context-param>
```

## 2.SSM整合

### 2.1 创建maven工程导入相关依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!--springmvc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- Mybatis核心 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
        <!--mybatis和spring的整合包-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.6</version>
        </dependency>
        <!-- 连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- MySQL驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <!-- log4j日志 -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->

        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.2.0</version>
        </dependency>
        <!-- 日志 -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- ServletAPI -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.1</version>
        </dependency>
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.12.RELEASE</version>
        </dependency>
```

### 2.2 配置web.xml

```xml
<!--    配置spring的编码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>UTF-8</param-value>
       </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
<!-- 配置设置请求方式的过滤器  -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
<!--    配置springmvc的前端控制器-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--        设置springmvc配置文件的位置-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
<!--        把DispatcherServlet的初始化提前到服务器启动时-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
<!--    设置servletContext监听器，在服务器启动时获取spring配置文件-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

<!--    设置spring配置文件的位置-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring.xml</param-value>
    </context-param>
</web-app>
```

### 2.3 创建SpringMVC配置文件并配置

```xml
<!--设置扫描器扫描controller层-->
    <context:component-scan base-package="com.hbw.ssm.controller"></context:component-scan>
<!--    配置视图解析器-->
    <bean id="viewResolver"
          class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean
                            class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
<!--  配置默认的servlet处理静态资源  -->
   <mvc:default-servlet-handler></mvc:default-servlet-handler>
<!--    开启mvc注解驱动-->
    <mvc:annotation-driven></mvc:annotation-driven>
<!--  配置访问主页的视图控制器  -->
    <mvc:view-controller path="/" view-name="index"></mvc:view-controller>
    <mvc:view-controller path="/to/add" view-name="employee_update"></mvc:view-controller>
<!--    设置文件上传解析器-->
    <bean id="CommonsMultipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
</beans>
```

#### 2.4 创建MyBatis核心配置文件(可以不创建)

```xml

    <settings>
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <plugins>
        <!--设置分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
```

### 2.5 创建mappe接口和mapper xml文件

```java
public interface EmployeeMapper {
    List<Employee> getAllEmployees();

    /**
     * 修改员工信息
     *
     * @param
     * @param employee
     */
    void updateEmployee( Employee employee);

    /**
     * 根据id查询员工信息
     * @param id
     * @return
     */
    Employee getEmployeeById(@Param("id") Integer id);
}
```

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.hbw.ssm.mapper.EmployeeMapper">
<!-- List<Employee> getAllEmployees();-->
    <select id="getAllEmployees" resultType="Employee">
        select * from t_emp
    </select>
<!--      void updateEmployee(Integer id);-->
    <update id="updateEmployee">
        update t_emp set emp_name=#{empName},age=#{age},sex=#{sex},email=#{email} where emp_id=#{empId}

    </update>
<!--     Employee getEmployeeById(Integer id);-->
    <select id="getEmployeeById" resultType="Employee">
        select * from t_emp where emp_id=#{id}
    </select>

```

### 2.6 创建jdbc.properties（管理数据库连接的）

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm1
jdbc.username=root
jdbc.password=hbw
```

### 2.7 创建log4.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}
%m (%F:%L) \n" />
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```



### 2.8 创建Spring核心配置文件并配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
https://www.springframework.org/schema/context/spring-context.xsd">
<!--扫描组件-->
<context:component-scan base-package="com.atguigu.ssm">
<context:exclude-filter type="annotation"
expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
<!-- 引入jdbc.properties -->
<context:property-placeholder location="classpath:jdbc.properties">
</context:property-placeholder>
<!-- 配置Druid数据源 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
<property name="driverClassName" value="${jdbc.driver}"></property>
<property name="url" value="${jdbc.url}"></property>
<property name="username" value="${jdbc.username}"></property>
<property name="password" value="${jdbc.password}"></property>
</bean>
<!-- 配置用于创建SqlSessionFactory的工厂bean -->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
<!-- 设置MyBatis配置文件的路径（可以不设置） -->
<property name="configLocation" value="classpath:mybatis-config.xml">
</property>
<!-- 设置数据源 -->
<property name="dataSource" ref="dataSource"></property>
<!-- 设置类型别名所对应的包 -->
<property name="typeAliasesPackage" value="com.atguigu.ssm.pojo">
</property>
<!--
设置映射文件的路径
若映射文件所在路径和mapper接口所在路径一致，则不需要设置
-->
<!--
<property name="mapperLocations" value="classpath:mapper/*.xml">
</property>
-->
</bean>
<!--
配置mapper接口的扫描配置
由mybatis-spring提供，可以将指定包下所有的mapper接口创建动态代理
并将这些动态代理作为IOC容器的bean管理
-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="com.atguigu.ssm.mapper"></property>
</bean>
</beans>
```

