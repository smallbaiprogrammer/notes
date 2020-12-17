spring 是干什么的

spring 基础要求 精通 Spring 的  XML 结构化 语言  Java  Web 项目 

spring boot 是建立在 spring FrameWork  基础上的

spring Framework 

IoC DI AOP  MVC JDBC 

（1） 引入各种jar 包  maven gradle 手动配置

（2） applicationContext。xml   Spring 核心配置文件

（3） 配置web。xml 

（4） 配置外部文件

这些操作都是作用不大的，重复的操作，所有有了spring boot 框架将这些操作全部封装起来，这些事情 是 Spring Framework 的开发操作

没有spring 之前采用 EJB 技术 ，将类打报放在服务器上运行 

spring起源 

《Expert One -on One J2EE Design and Development 》 -- > author r  **Rod johnson**

Spring 框架目前由pivotal公司团队进行维护 和开发 ，和pivotal 团队 是 从2012 年VMware差分出去，然后2019年VMware 以27亿美元收购了 这个团队 ，这些公司群是dell 的子公司 

所以springboot 是建立在 springFramework 之上的，简化了 开发，从而形成的一种新的框架

```
Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
```

我们没有进行配置，那么为什么能正常运行呢！

也就是框架已经自动帮助配置好了

那么他是通过什么来配置好的，通过maven的父子依赖关系引入的

pom 文件是Spring 项目的 必须要有的依赖，解决了版本冲突的问题，解决了环境问题

关于  Spring 的 XML 文件格式的学习，要完全掌握spring 的 XML 文件的格式 

Spring 的主类 SpringbootDemoApplcation主类，自己创建类也没有问题，框架运行入口  起始    main 方法 还有 类注解 @springBootApplication 来规定 运行入口

XML 文件是 Spring 的 环境配置信息 

 yml 文件 是 详细服务的创建 

对于spring boot 技术 

1. 创建工程比较方便，一键化创建
2. 通过starter 的思想简化了Spring 集成 其他组件的操作，也同时减少了出错的可能
3. 大大减少了开发成本，在微服务其他行业

spring boot 开发的 三板斧 

（1） 添加依赖  导入 jar包或者 war 包，在XML 配置文件添加依赖

（2） 配置文件

（3） 写注解    程序开发

广度  : Spring Cloud  、 微服务 、 容器化 、云原生等

深度： 实现原理[ 自动装配 、 starter 等] -- > Spring Framework 实现原理 或源码分析

（1） maven 引入父子依赖关系

（2） 配置 

```xml
<bean
</bean>
```

采用@configuration 注解来代替配置信息  // 或者 @ bean 

注解 

@Annotation 注解： 理解为Java代码提供的元数据、标签

标签可以作用域：类上 、 方法上 、 数据上  这些元注解是由JDK 提供的 ，

元注解表示当前注解可以作用的作用范围

可以通过类文件来配置文件



1.Spring boot 和 Spring  Framework 的关系 ？

2.springboot 的 features  特点

（1）Spring Framework 是spring 早期的框架 ，但是因为配置起来比较麻烦，spring Framework 与 外部工具一起用的时候，需要配置的信息非常多，比如tomcat 一起用的，时候，需要配置tomcat 版本号，端口号等信息，总体配置起来非常麻烦，如果对spring 不是特别熟悉，很容易出错。所以就出现了新的spring 框架，也就是spring boot，也就是说 ，spring boot 是在 spring Frame 的 框架基础上 演练而来的，是springFrame 的进化版，spring boot 很好地改善了spring Framework 的缺点，从配置上来说，spring boot 极大的简化了这些问题，可以采用注解，或者新建说明类的形式来配置信息，这样的好处很明显，容易使用，不易出错。你可以采用老的配置方式，也是可以的。 （2） 1.简单，容易使用，因为springboot 的很大程度的避免了配置文件，这里springboot 采用starter 依赖和自动配置。 2.灵活的环境熟悉配置，可以通过配置文件进行配置，可以通过注解进行配置，可以通过说明类进行配置。 3. 封装，我们看到表面的内容，框架内部的实现并不需要我们来考虑。