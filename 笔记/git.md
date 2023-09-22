[TOC]



# Git

详细笔记：[Git学习笔记【尚硅谷】_尚硅谷git笔记_m0_63077733的博客-CSDN博客](https://blog.csdn.net/m0_63077733/article/details/128773818?spm=1001.2014.3001.5501)

git使用方法：下载安装后，点击鼠标右键

![image-20230906153204690](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230906153204690.png)





# 1.git常用命令

| 基本命令                        | 作用                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| git init                        | 初始化本地库，只有初始化本地库后，才能进行其他操作，才会有.git文件夹 |
| git config --global  user.name  | 设置用户名   说明： 签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看 到，以此确认本次提交是谁做的。Git 首次安装必须设置一下用户签名，否则无法提交代码。 |
| git config --global  user.email | 设置用户邮箱                                                 |
| cat xxxxx                       | 查看某个文件                                                 |
| vim xxxxx                       | 进入到某个文件内                                             |
| i，：wq                         | 使用vim进入文件后，使用i进入插入模式。使用：wq保存并退出     |
| ctrl+ins                        | 复制                                                         |
| shift+ins                       | 粘贴                                                         |
| ll                              | 查看目录下所有文件                                           |
| tab                             | 自动补全代码                                                 |

注意：修改完文件后，还需要重新添加到暂存区和提交到本地库

git最常用最重要命令

| 基本命令                                                     | 作用                           |
| ------------------------------------------------------------ | ------------------------------ |
| **git status**                                               | **查看本地库状态**             |
| **git add xxx**                                              | **把xxx添加到暂存区内**        |
| **git commit -m" " xxx**                                     | **把xxx提交到本地库**          |
| **git reflog**                                               | **查看精简历史版本**           |
| **git log**                                                  | **查看详细历史版本**           |
| **git rm --cached 文件名**                                   | **删除暂存区的文件**           |
| **git reset --hard  精简 版本号**                            | **版本穿梭**                   |
| **git branch  -v**                                           | **查看分支**                   |
| **git branch 分支名**                                        | **创建分支**                   |
| **git checkout 分支名**                                      | **切换分支**                   |
| **git   merge xxxx分支名（无冲突）如果有冲突，先要解决冲突** | **把xxxx分支合并到当前分支上** |
|                                                              |                                |

版本和分支的切换底层就是移动head指针**，head指针通常指向我们看到的分支名，而分支名指向版本**

# 2. git常见概念

- 工作区：编写代码的地方，通常指磁盘。可以删除，且删除后不留记录。

- 暂存区：暂时储存的地方。可以删除，且删除后不留记录。

- 本地库：可以删除，但删除后也有历史版本记录。

- 远程库：代码托管中心。github，gitee，gitlab

- 分支：git的最主要特点。不同分支工作互不影响。

  - 概念：在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独 分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时 候，不会影响主线分支的运行。对于初学者而言，分支可以简单理解为副本，一个分支就是 一个单独的副本。（分支底层其实也是指针的引用）

  - 好处：**同时并行推进多个功能开发，提高开发效率。**

    各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败 的分支删除重新开始即可。

    使用git branch -v查看当前所有分支，使用git branch 分支名 指令来创建新分支。
    
    使用git  branch checkout 分支名 指令来切换分支。
    
    # 3. 合并分支（有冲突情况下）

使用git merge 分支名来合并分支，**如果没有冲突，合并成功后会直接提交本地库**

如果有冲突，此时要手动处理冲突在合并

产生冲突原因：**合并分支时，两个分支在\**同一个文件的同一个位置\**有两套完全不同的修改。Git 无法替 我们决定使用哪一个。必须\**人为决定\**新代码内容。**



>
>
>**<<<<<<< HEAD**
>
>**【当前分支的代码】**
>
>**======= ========**
>
> **合并过来的代码** 
>
> **>>>>>> hot-fix**

**如果没有冲突，合并成功后会直接提交本地库**

**有冲突解决冲突后要手动添加到暂存区并提交到本地库**

此时提交命令***\*git commit 命令时不能带文件名，并且修改后只有master被修改 hot-fix没有变）\****

# 4.团队内合作

![image-20230907115241453](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230907115241453.png)

**流程：从远程库clone代码到本地，然后进行开发。开发完毕后注意要先pull一下代码（确保远程库和本地库版本一致）然后再push代码到远程库（此步需要是团队成员才能push）**

## GitHub/Gitee 操作

1.首先要再github创建远程库 **注：此步在使用idea操作git时，直接使用分享到github就会自动创建远程仓库并push代码**

-    点击右上角+号

![image-20230907120208848](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230907120208848.png)

- 点击new仓库

  ![image-20230907120403581](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230907120403581.png)

- 创建仓库完毕后即可开发了

  ### 4.1 远程仓库相关操作命令

  | 常用命令                           | 作用                                                         |
  | ---------------------------------- | ------------------------------------------------------------ |
  | git remote -v                      | 查看所有远程地址别名                                         |
  | git remote add  别名 远程地址      | 给远程地址添加别名                                           |
  | **git clone 远程库地址**           | **克隆远程库代码到本地**                                     |
  | **git push 远程库别名 分支名**     | **提交分支到远程库**                                         |
  | **git pull 远程库别名 远程分支名** | **拉取远程库代码分支与本地库分支合并，用于更新本地库代码，使远程库代码和本地库代码一致** |
  
  

### 4.2 git remote -v 和git remote add  别名 远程地址

使用git remote -v命令查看所有远程仓库地址别名

使用git remote add 别名 远程地址 为远程地址添加别名

示例：![image-20230920113117796](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920113117796.png)

### 4.3 git clone 远程地址

使用git clone 指令。把远程库的代码克隆到了本地库，而且自动为远程库设置了一个别名叫origin



![image-20230920113607640](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920113607640.png)

### 4.4 git pull 远程库别名 远程库分支

使用git pull 远程库别名 远程库分支指令从远程库拉分支代码到本地库与本地库分支合并。用于保证远程库代码与本地库代码一致。

git clone 和 git pull用法：

- git clone 主要用于第一次从把远程库代码克隆到本地库。
- 而git pull主要用于远程库代码更新时，为保证本地库代码和远程库代码一致使用git pull

**小结：clone 会做如下操作。1、拉取代码。2、初始化本地仓库。3、创建别名**

***\*克隆：本地完全没有文件得到所有的文件\****

***\*拉取：本地已经有文件了，需要更新最新的\****

### 4.5 git push 远程库别名 分支名

**只有是团队成员才能使用git push 提交代码到远程库**

只有把代码提交到本地库后才能push到远程库。

# 5.跨团队协作

# 6. ssh免密登录

# 7.Idea集成git

## 7.1 配置忽略文件

![image-20230920203241786](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920203241786.png)

![image-20230920203306016](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920203306016.png)

![image-20230920203323495](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920203323495.png)

ignore内容

```txt

# Compiled class file
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (J2ME)
.mtj.tmp/
# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see 
http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml


```

![image-20230920203620474](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920203620474.png)

## 7.2 初始化本地库

选择被git管理的项目。选择setting中的git。

![image-20230920204631348](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920204631348.png)

设置git路径，选择git.exe

![image-20230920204749452](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920204749452.png)

点击test，出现git版本号即配置成功。

![image-20230920204921546](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920204921546.png)



创建git本地库

![image-20230920205316569](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920205316569.png)

创建成功后项目所在目录出现.git文件夹，初始化本地库成功。

![image-20230920205603741](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920205603741.png)

![image-20230920205910633](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920205910633.png)

此时全部红色，都在工作区需要添加到暂存区

![image-20230920210017158](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230920210017158.png)

点击add添加到暂存区’，添加成功后，颜色有变化变绿。

![image-20230921193837486](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921193837486.png)

然后提交到本地库

![image-20230921194010531](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921194010531.png)

提交成功后，文件都会变成正常颜色

![image-20230921194521863](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921194521863.png)

### 7.3 切换版本

对文件进行修改‘

![image-20230921194720436](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921194720436.png)

注意看，修改完文件变成蓝色，也就是说此时文件在暂存区，在我们修改后idea直接帮我们把文件添加到了暂存区，我们直接提交到本地库即可

![image-20230921200732652](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921200732652.png)

点击下方Git中的log查看版本信息

![image-20230921200924207](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921200924207.png)

右键版本切换版本

![image-20230921201039678](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201039678.png)



切换成功

![image-20230921201112384](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201112384.png)

### 7.4 分支

- 创建分支

![image-20230921201442406](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201442406.png)

创建分支成功后会自动切换到该分支

![image-20230921201545359](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201545359.png)

- 合并分支（无冲突情况）      对文件修改并提交到本地库

![image-20230921201644043](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201644043.png)

点击右下角分支切换回master分支

![image-20230921201853972](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921201853972.png)

点击右下角分支并选择合并分支’

![image-20230921202206052](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921202206052.png)

合并成功·，并自动提交本地库

![image-20230921202257870](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921202257870.png)

- 合并分支（有冲突情况）

  在master分支下修改文件并提交到本地库

  ![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921202855762.png)

切换到newBranch分支并在文件的同一位置进行修改

![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921203525414.png)

在切换为master分支来合并newBranch分支

此时发生了冲突，需要手动合并冲突‘

![image-20230921203727105](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921203727105.png)

![image-20230921203808800](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921203808800.png)

点击应用，合并冲突成功·，并提交本地库

![image-20230921203842561](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921203842561.png)

### 8.Idea集成github

点击setting中的github

点击加号

![image-20230921204419251](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921204419251.png)

可以选择第二个选项用token登录’

![image-20230921204524574](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921204524574.png)

去github中获取token步骤

- 在github右上角点击头像中的setting选项

  ![image-20230921204833715](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921204833715.png)

选择developer setting

![image-20230921204913506](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921204913506.png)

选择create new token

![image-20230921204952322](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921204952322.png)

将全部权限都打开并点击创建

![image-20230921205219645](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205219645.png)

生成的token要及时保存

![image-20230921205546037](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205546037.png)

，复制到idea

![image-20230921205632908](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205632908.png)

点击添加

![image-20230921205653348](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205653348.png)

关联github成功

## 8.1 push代码到github

点击git中的push选项

![image-20230921205825742](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205825742.png)

点击orgin中的自定义remote选项

![image-20230921205910167](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921205910167.png)

把github仓库的http或ssh链接粘贴过来

由于之前已经提交过代码，所以要先删除原来的别名，点击git中的管理别名选项

![image-20230921211340702](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921211340702.png)

把已有别名删除掉

![image-20230921211436026](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921211436026.png)

然后再点击orgin中的自定义remote选项把github仓库的http或ssh链接粘贴过来

![image-20230921211510716](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921211510716.png)

并点击push

![image-20230921211615260](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921211615260.png)

push成功，github已有代码

![image-20230921211707692](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921211707692.png)

## 8.2 clone远程库代码

当刚开始开发时，通常需要clone远程库代码到本地库

选择git中clone选项



![image-20230921212103041](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921212103041.png)

把远程库http或ssh地址粘贴过来，并点击clone

![image-20230921212252859](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921212252859.png)

![image-20230921212332063](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230921212332063.png)

选择新窗口

使用clone会自动帮我们为远程仓库起了别名默认叫origin，前文有说明，此处不再赘述。

## 8.3 pull代码

当远程库有更新，导致本地库代码与远程库代码不一致时，需要使用pull命令同步一下代码

在github中对文件进行修改

![image-20230922122627074](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922122627074.png)

此时在没有pull远程库情况下在对文件进行修改然后push会报错误，要合并冲突

**![image-20230922122858214](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922122858214.png)**

所以要先pull代码‘

点击git中的pull选项

![image-20230922122950825](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922122950825.png)

由于没pull之前修改过，所以此时pull代码需要合并冲突

![image-20230922123039970](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922123039970.png)

注意！！！！！：

**以后开发在push之前，最好在修改之前（不然可能需要解决冲突），要先pull，保证远程库代码和本地库代码一致**

![image-20230922123324893](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922123324893.png)

## 8.4 把项目分享到github

在idea中可以直接把项目分享到github

所以在**第一次**把项目交给github托管时可以使用分享

![image-20230922123835666](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922123835666.png)

分享成功后会自动创建github远程仓库。这样可以免去创建仓库，以及设置别名，push等操作

仅限第一次托管使用



# 9. gitee码云

码云基本操作和github一致，具体见详细笔记

## 9.1 Idea集成码云

首先在插件中下载码云插件

![image-20230922124235150](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922124235150.png)

然后再gitee中添加码云账号

![image-20230922124330288](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922124330288.png)

连接成功

![image-20230922125052926](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922125052926.png)

## 9.2 idea连接码云

Idea 连接码云和连接 GitHub 几乎一样，首先在 Idea 里面创建一个工程，初始化 git 工 程，然后将代码添加到暂存区，提交到本地库，这些步骤上面已经讲过，此处不再赘述。

也可以分享项目到gitee

![image-20230922125309886](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230922125309886.png)

其他操作基本和github一致’

# 10. gitlab

学写完linux在自学

