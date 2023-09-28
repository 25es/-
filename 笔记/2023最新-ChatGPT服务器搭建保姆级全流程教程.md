# 2023最新-ChatGPT服务器搭建保姆级全流程教程

- [演示：https://www.ai0001.online/ 密码：1234](https://zeejk007.online/#section0)
- [★必须](https://zeejk007.online/#section1)
- [第1步：购买服务器](https://zeejk007.online/#section2)
- [第2步：设置端口](https://zeejk007.online/#section3)
- [第3步：安装Docker环境](https://zeejk007.online/#section4)
- [下载软件：Xshell和Xftp](https://zeejk007.online/#section5)
- [打开Xshell软件](https://zeejk007.online/#section6)
- [①：下载docker.sh脚本](https://zeejk007.online/#section7)
- [②：列出下载的内容](https://zeejk007.online/#section8)
- [③：执行一下get-docker.sh文件](https://zeejk007.online/#section9)
- 
- [④：运行docker服务](https://zeejk007.online/#section11)
- [⑤：再次运行docker服务](https://zeejk007.online/#section12)
- [⑥：安装OpenAI](https://zeejk007.online/#section13)
- [⑦：检查下是否运行成功](https://zeejk007.online/#section14)
- 
- [&&如需域名访问，请接着往下看](https://zeejk007.online/#section16)
- 
- [购买域名&领取免费SSL证书：](https://zeejk007.online/#section18)
- [安装Nginx环境](https://zeejk007.online/#section19)
- [①：更新系统软件包](https://zeejk007.online/#section20)
- [②：安装EPEL存储库](https://zeejk007.online/#section21)
- [③：安装Nginx](https://zeejk007.online/#section22)
- [④：启动Nginx服务](https://zeejk007.online/#section23)
- [⑤：设置Nginx服务自启动：](https://zeejk007.online/#section24)
- [⑥：验证Nginx是否运行：](https://zeejk007.online/#section25)
- [⑦：安装vim编辑器：](https://zeejk007.online/#section26)
- [⑧：修改nginx.conf配置文件](https://zeejk007.online/#section27)
- [⑧：编辑nginx.conf配置文件](https://zeejk007.online/#section28)
- [重置OpenAPI 秘钥方法](https://zeejk007.online/#section29)

# 演示：https://www.ai0001.online/ 密码：1234

# ★必须

①：一台服务器

②：一个OpenAI的Key

方法1：最简单的获取方法，去某宝搜 “open账号ai” 购买一个key，几块钱，有3.5、4.0，买3.5就行了，4.0太贵了

方法2：+V 关注UP+收藏+点赞+投币+评论，赠送OpenAI 独立key

![图片](https://zeejk007.online/img/erweima.png)

# 第1步：购买服务器

[注意：近期很多B站的朋友联系我说国内的服务器搭建不成功，这个可能是OpenAI那边把国内的都屏蔽了，建议大家还是买香港的服务器最好，或者美国的服务器也行。](https://www.xiangcaoyun.com/?i5ad099)

推荐香草云服务器，性价比高，安全稳定，UP主经常使用他家的，大家可以放心使用，而且是可以24小时无理由退款

https://www.xiangcaoyun.com/?i5ad099

其他云服务器上安装方法都是一样的，此次全程以香草云服务器做演示

推荐买香港云/美国云服务器，1C2G就可以了

# 第2步：设置端口

TCP：1002

# 第3步：安装Docker环境

## 下载软件：Xshell和Xftp

Xshell：链接：https://pan.baidu.com/s/17nTapWa31vud3RasEg8aOA 提取码：b85r

Xftp：链接：https://pan.baidu.com/s/1XEDLfsotXFNyt1q2DpBzBg 提取码：3pge

傻瓜式安装，一直点下一步即可

## 打开Xshell软件

## ①：下载docker.sh脚本

```plain
curl -fsSL https://get.docker.com -o get-docker.sh
```

![图片](https://zeejk007.online/img/cuowu-01.png)如果出现这个，请通过下面链接里复制

https://docs.docker.com/engine/install/centos/

![图片](https://zeejk007.online/img/dockerimg-01.png)

## ②：列出下载的内容

```plain
ls
```

![图片](https://zeejk007.online/img/img-00.png)有这个说明下载成功

## ③：执行一下get-docker.sh文件

```plain
sh get-docker.sh
```

![图片](https://zeejk007.online/img/img-01.png)

## 

## ④：运行docker服务

```plain
systemctl start docker
```

## ⑤：再次运行docker服务

```plain
systemctl status docker
```

![图片](https://zeejk007.online/img/img-02.png)当出现active (running)… 即说明安装成功

## ⑥：安装OpenAI

```plain
docker run --name chatgpt-web -d -p 1002:3002 --env OPENAI_API_KEY=sk-秘钥 --env AUTH_SECRET_KEY=1234 --restart always chenzhaoyu94/chatgpt-web:latest
	
```

这一步安装时间较长 红色的是：OpenAI的key，将你自己的key复制进去

蓝色的是：访问密码，可以自己随意设置

![图片](https://zeejk007.online/img/img-03.png)

## ⑦：检查下是否运行成功

```plain
docker ps
```

将安全组添加端口1002

到这就可以访问了，不过只能IP访问（端口是1002）

如果想域名访问，请接着看

# 

# &&如需域名访问，请接着往下看

# 

# 购买域名&领取免费SSL证书：

https://www.bilibili.com/video/BV1oh4y1e7in/

# 安装Nginx环境

以下是在CentOS系统中安装Nginx的步骤：

### ①：更新系统软件包

```plain
sudo yum update
```

中间会让你输入1次y

### ②：安装EPEL存储库

```plain
sudo yum install epel-release
```

中间会让你输入1次y

### ③：安装Nginx

```plain
sudo yum install nginx
```

中间会让你输入2次y

### ④：启动Nginx服务

```plain
sudo systemctl start nginx
```

### ⑤：设置Nginx服务自启动：

```plain
sudo systemctl enable nginx
```

### ⑥：验证Nginx是否运行：

```plain
sudo systemctl status nginx
```

如果一切正常，输出应该是“Active: active (running)”或者类似的信息。

### ⑦：安装vim编辑器：

```plain
yum install vim
```

中间会让你输入1次y

### ⑧：修改nginx.conf配置文件

```plain
#进入nginx 
cd /etc/nginx/
	
```

查看里面有没有nginx.conf文件

```plain
ls
```

![图片](https://zeejk007.online/img/img-04.png)

### ⑧：编辑nginx.conf配置文件

```plain
vim nginx.conf
```

替换代码 按键盘上的a，进入编辑模式

```plain
a
```

用键盘上的↑↓← →键移动光标![图片](https://zeejk007.online/img/img-05.png)

将上图红框里的代码删掉，换成下面的代码

请将域名换成你们自己的域名，SSL证书也换成你们自己的

```plain
proxy_buffering off; 

upstream chatgpt-web {
	server 127.0.0.1:1002 weight=1;
}

server {
  listen 80;
  server_name www.替换的域名 替换的域名;
  location / {
	rewrite ^(.*)$ https://www.替换的域名; 
  }
}

server {
  listen 443 ssl;
  server_name www.替换的域名;
  ssl_certificate /etc/nginx/替换的SSL证书.pem;
  ssl_certificate_key /etc/nginx/替换的SSL证书秘钥.key;
  location / {
	proxy_pass http://chatgpt-web;
  }     
}
	
```

鼠标中间粘贴 修改完，按ESC键保存并退出编辑模式

然后输入命令，敲回车

```plain
:wq!
```

检查

```plain
nginx -t
```

![图片](https://zeejk007.online/img/img-06.png)

出现这个即成功

执行这个脚本

```plain
systemctl start nginx
systemctl status nginx
```

![图片](https://zeejk007.online/img/img-07.png)

```plain
systemctl restart nginx
```

即可

# 重置OpenAPI 秘钥方法

```plain
docker ps
docker stop 688a2580b3bf
docker rm -f 688a2580b3bf
docker ps -a
```

更新秘钥

```plain
官方：docker run --name chatgpt-web -d -p 1002:3002 --env OPENAI_API_KEY=sk-秘钥 --env AUTH_SECRET_KEY=1234 --restart always chenzhaoyu94/chatgpt-web:latest
香港：docker run --name chatgpt-web -d -p 1002:3002 \
--env OPENAI_API_KEY=hk-zy3oscrpgjb10r0pufv201ooraqlz4jwl5mnseul90xducg2 \
--env AUTH_SECRET_KEY=1913 \
--env OPENAI_API_BASE_URL=https://twapi.openai-hk.com  registry.cn-hangzhou.aliyuncs.com/generative/chatgpt-web	
```

红色的是：OpenAI的key，将你自己的key复制进去 蓝色的是：访问密码，可以自己随意设置





# 2023最新-ChatGPT搭建全流程【优化版】

 必须 

①：服务器（不要用国内的）

②：OpenAI key



 服务器 

不要用国内的，会被屏蔽，会出现各种问题，选择香港或者国外的

推荐香草云服务器，性价比高，安全稳定，UP主经常使用他家的，大家可以放心使用，而且是可以24小时无理由退款

https://www.xiangcaoyun.com/?i5ad099

其他云服务器上安装方法都是一样的，此次全程以香草云服务器做演示

推荐买香港云/美国云服务器，1C2G就可以了



 OpenAI 获取 

注意：直接调用OpenAI官方API会有一定几率被屏蔽，最好、最优、最稳定的方法是选择中转API，效果是跟官方的一模一样的

 中转OpenAI key获取方法 

网址：https://www.openai-hk.com/open/index

官网GPT公开接口与HKGPT企业接口优势对比

计费采用积分消耗制,GPT-4.0模型为OPENAL原价

官网120美刀GPT3.5APIKEY质保72小时

3.5模型:1美刀额度-1人民币(官网原价的0.14折扣)

OPENAI-HK

4.0模型:1美刀额度-7.1人民币(官网原价)

回答:0.426人民币/1KTOKEN

提问:0.03美刀/1KTOKENS

提问:0.213人民币/1KTOKENS

0.002人民币/1KTOKENS

回答:0.06美刀/1KTOKEN

GPT3.5/GPT-4.0

0.002美刀/1KTOKENS

官网GPT公开接口

PT3.5/GPT-4.0

风险高.风控掉号

T公开接口HKGPT企业接口

中转OPENAI接口

控制台

风险低.长期有效

连OPENAI接口

立即购买GPT4.0

账号池多账号轮训

购买GPT3.5

GPT4.0资费

GPT3.5资费

科学上网

密钥期限

至10元人民币

价格文栏

调用限速

调用方式

按月计算

账号风控

语言模型

消费门槛

月120美刀

不需要

无限制

需要

汇率:7.1

单账号

功能

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695111561776-9cb71f1c-4a3d-4b2c-983c-b01dacd32397.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



点击这里的控制台，扫码登录，点击这里的：获取key

由于0PENA风控等级日益提育,从北京时间2023年9月25日23京开始:

普通模型(不包括GPT4)的折扣将由原来官网的014倍调整为0.28倍.

通知时间:2023年9月17日

OPENAI-HK

也就是官方120美刀额度,由原来的120人民币调整为240人民币

09-1916:04:09

@获取KEY

PT3.5-TURBO

09-1915:46:41

09-1912:04:04

09-1915:47:50

09-1915:46:59

PT-3.5-TURBO

使用概况

凸购买充值

可用积分

7331

GPT-3.5-TURBO

COMPLETIOR

GPT-3.5-TURB0

调用次数

144

用TOKEN

用积分

0.09元

856

1439

PROMPT

2160

GPT-3.5-TURBO

721

TOKENS

682

时间

1419

模型

86

余额

665

84

138

581

17

52

737

59

41

18

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695111608175-b078ec3a-d444-403e-8d27-e95e6501bc2e.png)



网关由原来的HTTPS/LAPIOPENAICOM营换为HTTPS:/LAPLOPENAIHKCOM

OPENAI-HK

服QQ:1853719782

SDMXAOKV

更多接入说明:点这查看

接入说明

@获取KEY

使用概况

复制KEY

凸购买充值

重置KEY

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112179742-85417afe-18ef-40a6-8749-f70384e6c1c0.png)



这里会有一个免费的key，如果是空的，点击这里的重置key，因为是中转的key，这里的前缀是hk，官方的是sk

这里的key是没有时长限制的，是永久可以用的，不限制提问次数，但是有额度限制，用完了就要充值

刚注册默认会赠送1000积分，1000积分大概可以生成1W个汉字

由于0PENA1风控等级日益提高,从北京时间2023年9月25日宽23点开始:

当前KEY池有6个GPT-4.010个GPT-3.5可支持并发GPT-4为200X6,GPT-3.5为4000*10

普通模型(不包括9PL4)的折扣将由原来官网的0.14倍调整为0.28倍

也就是官方120美刀额度,由原来的120人民币调整为240人民

OPENAI-HK

通知时间:2023年9月17日

000元1千万积分

请输入卡密格式XOCX-XOOXOOX

50元50万积分

100元1百万积分

500元5百万积分

卡密充值

10元10万积分

下一步去支付

凸购买充值

使用锅况

1000元

100元

获取KEY

500元

10元

50元

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695111910961-5aa78e2f-7e85-42be-b117-7a66fa3c37a5.png)



积分用完了，可以在里面充值10块钱

10块钱有10W积分，10W积分大概可以生成100W汉字，可以够大家用很久了



 搭建流程 

 ①：现重置下服务器系统（如果是新开机器，可以跳过这步） 

显续表自动埃费开机关机清除代理

香草云

自云服务器

妻开级民置

内容分发CD

月购买云服务器

号服务器托管

息账号管理

[日服务器粗

CMYSERVER

支持与服

帮助文档

正常即将过期

购SSL证书

1/页共6条记录10.

云数捐库

域名管理

狂9云影务器

巷/港A

147包

虚拟主机

务器IP,

输入云主机IP精

国香港/港

订单状态

路(推荐)/2C4G

号/别名

管理面

动续费

0正常

已过期

目云服务器

?财务中心

账户

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112348926-8ce5ea07-c18e-4789-a487-c66e21b738e0.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



云服务器管理#1478

地域/线路/可用区:中国香港/香港CN2线路(推荐)/港B区

续费时间:29天后(2023-10-19)

编号/别名:#1478MYSERVE

续费价格(月):60.00元

创建时间:2023-09-1911:48

统:CENTOS-7.6.1810-X6

][升级记录]

门自动续费[手动续费]

[升级配置]

重置密码]

云服务器信息

YCPU:2VCPUS内存

远程连接

[重装系统]

操作日志

S内存:4CB硬盘:4

硬盘:40GB带宽:5MBPS

资源配置:

安全组

重启

付费信息

操作系统:

己概览

品网络

口关机

巴备份

监控

三任务

开机

日硬盘

系统

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112384948-f9c7a3a8-d80b-47cb-a06b-3f442c5bdfe6.png)



重装系统#1478

OCENTOS-7.8.2003-X64

CENTOS-7.6.1810-X6

COCENTOS-8-STREAM-X64

重装系统,系统盘数据将全部丢失,务必做好备份

OCENTOS-7.8.2003-X64-BT

COCENTOS-6.7.1504-X64

OCENTOS-9-STREAM-X64

请输入会员登录密码

安装方式:

公共镜像

随机分面

会员密码:

选择模板:

跟随系统(保特不变

点击装系统

WINDOWS

远程端口:

CENTOS

UBUNTU

DEBIAN

![image.png](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112410828-168fb9eb-8fe8-493e-ba0f-294181938171.png)





 ②：添加1002端口 

 ③：安装Xshell、Xftp软件 

下载软件：Xshell和Xftp

Xshell：链接：https://pan.baidu.com/s/17nTapWa31vud3RasEg8aOA  提取码：b85r Xftp：链接：https://pan.baidu.com/s/1XEDLfsotXFNyt1q2DpBzBg  提取码：3pge

傻瓜式安装，一直点下一步即可



 ④：安装docker环境 

打开Xshell软件，连接上服务器







![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112576967-4a63f0c7-fb54-4fb8-9605-26e5646d5d18.png?x-oss-process=image%2Fresize%2Cw_236%2Climit_0)



只要看到有这个，说明下载成功





![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112576984-be716365-6685-4de5-883c-73d92772a5d8.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)









![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695112576981-e1c28f79-16a2-4fad-be0a-9f3cf361cec9.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)

当出现active (running)… 即说明安装成功





 ⑤：安装ChatGPT镜像 









到这就可以访问了，不过只能IP访问（端口是1002）

如果想域名访问，请接着往下看



 &&如需域名访问，请接着往下看 

 购买域名&领取免费SSL证书： 

https://www.bilibili.com/video/BV1oh4y1e7in/

 安装Nginx环境 

以下是在CentOS系统中安装Nginx的步骤：

 ①：更新系统软件包 



中间会让你输入1次y

 ②：安装EPEL存储库 



中间会让你输入1次y

 ③：安装Nginx 



中间会让你输入2次y

 ④：启动Nginx服务 



 ⑤：设置Nginx服务自启动： 



 ⑥：验证Nginx是否运行： 



如果一切正常，输出应该是“Active: active (running)”或者类似的信息。

 ⑦：安装vim编辑器： 



中间会让你输入1次y

 ⑧：修改nginx.conf配置文件 



查看里面有没有nginx.conf文件



![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695139746138-8c5065a6-949c-4575-8f22-722015004730.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



 ⑧：编辑nginx.conf配置文件 



替换代码 按键盘上的a，进入编辑模式



用键盘上的↑↓← →键移动光标

![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695139747810-0a701328-ade4-4119-94b0-ac3a5c37b7cc.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)



将上图红框里的代码删掉，换成下面的代码

请将域名换成你们自己的域名，SSL证书也换成你们自己的



鼠标中间粘贴 修改完，按ESC键保存并退出编辑模式

然后输入命令，敲回车



检查



![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695139744013-a4eb461d-f176-4f94-baf9-adb971b2d113.png?x-oss-process=image%2Fresize%2Cw_480%2Climit_0)



出现这个即成功

执行这个脚本





Plain Text



复制代码

1

systemctl start nginx





Plain Text



复制代码

1

systemctl status nginx

![img](https://cdn.nlark.com/yuque/0/2023/png/29015062/1695139746159-b7a3f943-c4be-436d-8a36-e5d678824823.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)







Plain Text



复制代码

1

systemctl restart nginx

即可



 更换key/访问密码 





查看容器ID

Plain Text



复制代码

1

docker ps





停用容器ID

Plain Text



复制代码

1

docker stop 容器id





删除容器ID

Plain Text



复制代码

1

docker rm -f 容器id





Plain Text



复制代码

1

docker ps -a





更新key/访问密码

Plain Text



复制代码

1

2

3

4

docker run --name chatgpt-web -d -p 1002:3002 \

--env OPENAI_API_KEY=hk-你的key \

--env AUTH_SECRET_KEY=访问密码 \

--env OPENAI_API_BASE_URL=https://api.openai-hk.com  registry.cn-hangzhou.aliyuncs.com/generative/chatgpt-web

若有收获，就点个赞吧

![image-20230922135134833](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922135134833.png)