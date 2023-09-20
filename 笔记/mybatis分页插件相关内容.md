# mybatis分页插件相关内容

>
>
>limit index,pageSize
>
>pageSize：每页显示的条数
>
>pageNum：当前页的页码
>
>index：当前页的起始索引，index=(pageNum-1)*pageSize
>
>count：总记录数
>
>totalPage：总页数
>
>totalPage = count / pageSize;
>
>if(count % pageSize != 0){
>
>totalPage += 1;
>
>}
>
>pageSize=4，pageNum=1，index=0 limit 0,4
>
>pageSize=4，pageNum=3，index=8 limit 8,4
>
>pageSize=4，pageNum=6，index=20 limit 8,4
>
>首页 上一页 2 3 4 5 6 下一页 末页

**12.1****、分页插件的使用步骤**

**①添加依赖**

<dependency>

<groupId>com.github.pagehelper</groupId>

<artifactId>pagehelper</artifactId>

<version>5.2.0</version>

</dependency>

**②配置分页插件**

在MyBatis的核心配置文件中配置插件

<plugins>

<!--设置分页插件-->

<plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>

</plugins>





**a>在查询功能之前使用PageHelper.startPage(int pageNum, int pageSize)开启分页功能**

pageNum：当前页的页码

pageSize：每页显示的条数

**b>在查询获取list集合之后，使用PageInfo<T> pageInfo = new PageInfo<>(List<T> list, int**

**navigatePages)获取分页相关数据**

list：分页之后的数据

navigatePages：导航分页的页码数

**c>分页相关数据**

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