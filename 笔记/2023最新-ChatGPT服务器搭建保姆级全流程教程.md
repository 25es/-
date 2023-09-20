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
docker stop 99e7b15c5b91
docker rm -f 99e7b15c5b91
docker ps -a
```

更新秘钥

```plain
docker run --name chatgpt-web -d -p 1002:3002 --env OPENAI_API_KEY=sk-秘钥 --env AUTH_SECRET_KEY=1234 --restart always chenzhaoyu94/chatgpt-web:latest
	
```

红色的是：OpenAI的key，将你自己的key复制进去 蓝色的是：访问密码，可以自己随意设置