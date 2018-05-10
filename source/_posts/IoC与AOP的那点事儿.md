---
title: IoC与AOP的那点事儿
date: 2017-11-02 14:20:06
tags:
   - Spring
   - JAVA
---
## IoC
控制反转(Inversion of Control)是OOP中的一种设计原则,也是Spring框架的核心.大多数应用程序的业务逻辑代码都需要两个或多个类进行合作完成的,通过IoC则可以减少它们之间的耦合度.

## 实现方法

IoC的主要实现方法有两种,依赖注入与依赖查找.

依赖注入 : 应用程序被动的接收对象,IoC容器通过类型或名称等信息来判断将不同的对象注入到不同的属性中.
<!-- more -->
依赖注入主要有以下的方式:

- 基于set方法 : 实现特定属性的public set()方法,来让IoC容器调用注入所依赖类型的对象.
- 基于接口 : 实现特定接口以供IoC容器注入所依赖类型的对象.
- 基于构造函数 : 实现特定参数的构造函数,在创建对象时来让IoC容器注入所依赖类型的对象.
- 基于注解 : 通过Java的注解机制来让IoC容器注入所依赖类型的对象,例如Spring框架中的@Autowired.
- 依赖查找 : 它相对于依赖注入而言是一种更为主动的方法,它会在需要的时候通过调用框架提供的方法来获取对象,获取时需要提供相关的配置文件路径、key等信息来确定获取对象的状态.

## IoC的思想

在传统实现中,我们都是通过应用程序自己来管理依赖的创建,例如下代码.
```
public class Person {
	// 由Person自己管理Food类的创建
	public void eat() {
		Food food = new Chicken();
		System.out.println("I am eating " + food.getName() + "...");
	}
}
```
而IoC则是通过一个第三方容器来管理并维护这些被依赖对象,应用程序只需要接收并使用IoC容器注入的对象而不需要关注其他事情.
```
public class Person {
	private Food food;
	// 通过set注入
	public void setFood(Food food) {
		this.food = food;
	}
	// Person不需要关注Food,只管使用即可
	public void eat() {
		System.out.println("I am eating " + this.food.getName() + "...");
	}
}
```
通过以上的例子我们能够发现,控制反转其实就是对象控制权的转移,应用程序将对象的控制权转移给了第三方容器并通过它来管理这些被依赖对象,完成了应用程序与被依赖对象的解耦.

## AOP

AOP(Aspect-Oriented Programming)即面向方面编程.它是一种在运行时,动态地将代码切入到类的指定方法、指定位置上的编程思想.用于切入到指定类指定方法的代码片段叫做切面,而切入到哪些类中的哪些方法叫做切入点.

AOP是OOP的有益补充,OOP从横向上区分出了一个个类,AOP则从纵向上向指定类的指定方法中动态地切入代码.它使OOP变得更加立体.

Java中的动态代理或CGLib就是AOP的体现.

## 案例分析

在OOP中,我们使用封装的特性来将不同职责的代码抽象到不同的类中.但是在分散代码的同时,也增加了代码的重复性.

例如,我们需要在两个或多个类中的方法都记录日志或执行时间,可能这些代码是完全一致的,但因为类与类无法联系造成代码重复.
```
public class A {
	public void something () {
		// 业务逻辑...
		recordLog();
	}
	private void recordLog() {
		// 记录日志...
	}
}
public class B {
	public void something () {
		// 业务逻辑...
		recordLog();
	}
	private void recordLog() {
		// 记录日志...
	}
}
```
接下来,我们采取两种不同方案来改进这段代码.

将重复代码抽离到一个类中
```
public class A {
	public void something () {
		// 业务逻辑...
		Report.recordLog();
	}
}
public class B {
	public void something () {
		// 业务逻辑...
		Report.recordLog();
	}
}
public class Report {
	public static void recordLog (String ...messages) {
		// 记录日志...
	}
}
```
这样看似解决了问题,但类之间已经耦合了.并且当这些外围业务代码(日志,权限校验等)越来越多时,它们的侵入(与核心业务代码混在一起)会使代码的整洁度变得混乱不堪.

使用AOP分离外围业务代码

我们使用AspectJ,它是一个AOP框架,扩展了Java语言,并定义了AOP语法(通过它实现的编译器).

使用AspectJ需要先安装并将lib中aspectjrt.jar添加进入classpath,下载地址.
```
public class Something {
    public void say() {
        System.out.println("Say something...");
    }
    public static void main(String[] args) {
        Something something = new Something();
        something.say();
    }
}
public aspect SomethingAspect {
    /**
     * 切入点,切入到Something.say()
     */
    pointcut recordLog():call(* com.sun.sylvanas.application.hello_aop.Something.say(..));
    /**
     * 在方法执行后执行
     */
    after():recordLog() {
        System.out.println("[AFTER] Record log...");
    }
}
```
AOP解决了代码的重复并将这些外围业务代码抽离到一个切面中,我们可以动态地将切面切入到切入点.

本文作者：SylvanasSun
原文链接：https://sylvanassun.github.io/2017/06/07/2017-06-07-IoC&AOP/
版权归作者所有，转载请注明出处