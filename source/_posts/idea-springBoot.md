---
title: idea SpringBoot 新特性
date: 2017-09-20 14:27:52
tags:
  - idea
  - spring-boot
---

前言
---

Intellij IDEA 2017.2.2版本针对Springboot设置了一些特性，本篇博客给搭建简单介绍一下如何使用这些特性。

Run Dashboard
-------------

针对Spring boot提供了Run Dashboard方式的来代替传统的run方法。下面看一下官网提供的面板结构图：  
![这里写图片描述](https://www.jetbrains.com/idea/whatsnew/img/2017.2/idea_2017_2_spring_run_dashboard.gif "")

是不是很炫，直接可以通过Dashboard看到Springboot的启动项目，并显示相应的端口等信息，同时还能在这里进行相应的操作。下面我们来看看如何调用出Dashboard。
<!-- more -->
首先，你的项目应该是一个springboot的项目。然后进入Edit configurations，点击+号，找到springboot选项，添加一个springboot的配置。  
![这里写图片描述](http://img.blog.csdn.net/20170823121157933?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd281NDEwNzU3NTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "")  
然后依次配置，name，Main class（包含main方法的启动类），working directory，Use classpath of module，jre等。  
最重要的是要合适一下下面的 Show in Run Dashboard是否勾选，如果未勾选，将其勾选。  
![这里写图片描述](http://img.blog.csdn.net/20170823121437461?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd281NDEwNzU3NTQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast "")

这样就完成这一新特性的配置。尝试一下吧。效果与官网提供的相同。

Actuator endpoints
------------------

经过上面的步骤启动springboot之后，你会发现在右侧出现了一个Endpoints的tab项。此项中又包含Health, Beans, 和Mappings。  
![这里写图片描述](https://www.jetbrains.com/idea/whatsnew/img/2017.2/idea_2017_2_spring_endpoints_2.png "")

比如Mappings可以显示出Springboot对外暴露的请求地址等信息。具体功能可自行尝试。不过，在使用此功能之前需要在pom文件中配置对应依赖。

```hljs xml has-numbering
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>1.2.3.RELEASE</version>
        </dependency>
```
