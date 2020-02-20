# 40分钟搭建Java服务器（Linux）

某一天你突然有一个紧急需求，这个需求需要一个后台服务器作为支撑

时间紧迫，周围又没有对Java服务器特别熟悉的小伙伴

怎么办？

这篇文章能救到你！

## 这篇文章有什么？需要什么基础？

+ 这篇文章能够带你在短时间内（不包括编码，不一定在40分钟内但一定在一下午内）从0（甚至没有开发环境）上线一台处于公网的可访问的、可debug的服务器。
+ 这篇文章上线的服务器可以是无数据库的，也可以是内嵌H2Database的，如果有MySQL配置经验的话也可以有独立数据库的
+ 这篇文章需要你掌握一定的Linux基础（主要是常用指令），Java编程基础（主要是面向对象编程），数据库基础和一定的算法、组原、计网基础。

事不宜迟现在就开始吧！

## 0 min

接到了需求，明确了需求，决定了！就是你了！Java服务器！

开始之前当然是要做好开发环境的安装啦

那么我们需要什么开发环境呢？

+ Java Development Kit & Java Runtime Environment

  JDK和JRE是一定要有的。这里给出Java8的下载地址，需要登录。其他版本也是可以的，但是建议选择大于等于8的Java版本

  [Java8官方下载地址](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)

  顺着程序一路点下一步安装即可。

  如果使用IDEA开发的话，其实可以在开发的时候再选择JDK目录。当然你也可以事先配置好环境变量。

  [如何配置环境变量](https://jingyan.baidu.com/article/6dad5075d1dc40a123e36ea3.html)

  在win10里设置环境变量可以采取直接选择地址的方法，大同小异。

+ JetBrains IDEA

  在这里选择IDEA是再快速不过的事情了。不过你可能得花点时间去验证你的学生身份或者去寻找一下注册机，这样才能正常使用IDEA。

  一路点下一步安装完毕即可。
  
  [IDEA官方下载地址](https://www.jetbrains.com/idea/download/#section=windows)

到此所有开发环境安装完毕



## 10 min

该开始开发了对吧

首先要做的应该是创建一个项目。

本文章的主题是快速开发，自然是需要选择一个能够支持快速开发的框架

在这里，约定大于配置的SpringBoot就再好不过了。同时IDEA也提供了Spring initializer。

由于本文章的主要内容不在于编程，所以与编程相关的内容一并掠过。下面给出一些在讲解过程中给出的小提示或者教程。



国内Gradle镜像，可用于解决创建完项目后下载速度偏慢的问题：

```java
repositories {
    maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
    maven { url 'http://maven.aliyun.com/nexus/content/repositories/jcenter' }
    // mavenCentral()
}
```



项目的结构：[啊葱的个人风格](http://cong-onion.cn/archives/personal-style)

Spring自带的json解析器的用法：[Jackson注解用法](https://blog.csdn.net/zhouyecheng/article/details/79562425)

演示中所用的内嵌数据库的介绍：[H2Database](https://www.cnblogs.com/cnjavahome/p/8995650.html)

演示中所用的getter和setter自动生成插件[LomBok用法](https://www.jianshu.com/p/2543c71a8e45)

[数据库-catalog与schema简介](https://blog.csdn.net/zhangyong329/article/details/53392690)

[SpringData用法](https://blog.csdn.net/w_h_s_233/article/details/89887580)



## 30 min

假设刚刚的20分钟已经足够你开发出一套具有几个API的服务器，并且打算上线测试了

这里我们需要一个云服务器和一个ssh连接软件。

云服务器可以选择阿里云或腾讯云的，注册+购买十分方便，在5分钟左右

这里推荐一个比较傻瓜的ssh连接软件：[FinalShell](http://www.hostbuf.com/t/988.html)

通过Gradle的bootJar创建部署jar包

然后上传至服务器，运行即可。

```shell
nohup java -Xdebug -Xrunjdwp:transport=dt_socket,address=5005,server=y,suspend=n -jar xxx.jar >/dev/null 2>&1&
```
