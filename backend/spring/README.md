# TODO

* 了解servlet
    推荐阅读：
    * https://zhuanlan.zhihu.com/p/34518314 servlet究竟是啥?
    为java程序提供一个统一的web应用的规范。
    
    servlet本身是一个接口，它提供了web应用使用的一种规范。用户将tomcat servlet容器放入一个实现好的servlet，再调用serve接口来实现server的功能

    * https://www.zhihu.com/question/21416727 Servlet具体是怎么工作的
        * httpServlet帮我们实现了在servlet接口下快速的实现一些业务逻辑
        * servlet工作流程
        * servlet 在tomcat底下是如何被注入的

    * https://personal.ntu.edu.sg/ehchua/programming/howto/Tomcat_HowTo.html 自己动手写一个Tomcat servlet全栈app
        * servlet helloword 
        * 为tomcat webapp增加一个数据库
    * https://zhuanlan.zhihu.com/p/65658315 ServletContext
        * ServletContext是什么 
        * 如何获取ServletContext
        * Filter拦截方式之：REQUEST/FORWARD/INCLUDE/ERROR TODO
        * Servlet映射器
        * 自定义DispatcherServlet
        * DispatcherServlet与SpringMVC
        * conf/web.xml与应用的web.xml

* Maven

* Spring 入门
    * 官方教程：https://spring.io/quickstart
    * 官方文档：https://docs.spring.io/spring-framework/docs/current/reference/html/index.html
    * 学习Spring之前要先学习什么？ - 沈世钧的文章 - 知乎 https://zhuanlan.zhihu.com/p/64001753
    * Spring源码阅读指南 - 沈世钧的文章 - 知乎 https://zhuanlan.zhihu.com/p/51393923
    * IOC
        * Spring IoC有什么好处呢？ - Mingqi的回答 - 知乎 https://www.zhihu.com/question/23277575/answer/169698662
    * Java 注解
        * Autowire 如何实现 https://zhuanlan.zhihu.com/p/387354943
    * Java 反射
    * Spring 注解
        * 接近8000字的Spring/Spring常用注解总结！安排！ - JavaGuide的文章 - 知乎 https://zhuanlan.zhihu.com/p/137507309


* Demos：
    * QuickStart: 官方的spring boot快速搭建教程，代码位于`./Spring项目代码/helloWorld`
    * Restful server: 官方的demo，位于`./spring项目代码/RESTful`