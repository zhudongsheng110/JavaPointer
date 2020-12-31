> 八、**SpringBoot快速开发框架**
- [SpringBoot](#springboot)
    - [springboot的常用注解有](#springboot的常用注解有)
    - [springboot自动配置的原理](#springboot自动配置的原理)
    - [什么是SpringBoot](#什么是springboot)
    - [为什么要用SpringBoot](#为什么要用springboot)
    - [SpringBoot启动方式](#springboot启动方式)
    - [SpringBoot与SpringMVC 区别](#springboot与springmvc-区别)
    - [SpringBoot与SpringCloud 区别](#springboot与springcloud-区别)
    - [SpringBoot中用那些注解](#springboot中用那些注解)
    - [SpringBoot热部署使用什么？](#springboot热部署使用什么)
    - [你们项目中异常是如何处理](#你们项目中异常是如何处理)
    - [SpringBoot如何实现异步执行](#springboot如何实现异步执行)
    - [SpringBoot多数据源拆分的思路](#springboot多数据源拆分的思路)
    - [SpringBoot多数据源事务如何管理](#springboot多数据源事务如何管理)
    - [SpringBoot如何实现打包](#springboot如何实现打包)
    - [SpringBoot性能如何优化](#springboot性能如何优化)
    - [SpringBoot执行流程](#springboot执行流程)
    - [SpringBoot底层实现原理](#springboot底层实现原理)
    - [SpringBoot装配Bean的原理](#springboot装配bean的原理)
    - [springboot的版本问题](#springboot的版本问题)
_____
### SpringBoot
 springboot是spring4.0之后提供的一个自动化启动框架,采用习惯优于配置
    的理念,可以进行自动化配置(EnableAutoConfiguration),框架采用
    注解+properties(或yaml)代替传统的xml配置,极大的提高了开发效率.
    springboot通过main方法启动,而且内置web容器(tomcat),打包方式为jar
#### springboot的常用注解有 
    @springbootapplication  标识启动类
    @configuration      标识配置类
	@EnableAutoConfiguration  开启自动配置
	@ComponentScan    注解扫包器
	@MapperScan      扫描mybatis的mapper接口
	@RestController  rest风格返回json

#### springboot自动配置的原理
      spring-boot-autoconfigure jar包下meta-inf文件夹下,
      spring.factories配置文件提供了各种技术框架的默认配置
      我看过很多自动配置类的源码,比如springmvc,自动配置里
      指定dispatcherServlet请求路径为/,指定了处理器映射器
      requestMappingHandlerMapping等组件的配置
#### 什么是SpringBoot
SpringBoot是快速开发的Spring框架，能够快速整合主流框架，简化xml配置，采用全注解化，内置Http服务器（如tomcat、jetty等），通过java部署运行。

#### 为什么要用SpringBoot
快速开发，快速整合，配置简化、内嵌服务容器

#### SpringBoot启动方式
主类@SpringBootApplication注解或添加@ComponentScan和@EnableAutoConfiguration注解，使用@SpringBootApplication时注意自动扫描当前包

#### SpringBoot与SpringMVC 区别
SpringMVC是SpringBoot的Web开发框架

#### SpringBoot与SpringCloud 区别
     SpringBoot是快速开发的Spring框架，SpringCloud是完整的微服务框架，SpringCloud依赖于SpringBoot。
     spring boot使用了默认大于配置的理念，集成了快速开发的spring多个插件，同时自动过滤不需要配置的多余的插件，简化了项目的开发配置流程，一定程度上取消xml配置，是一套快速配置开发的脚手架，能快速开发单个微服务；
     spring cloud大部分的功能插件都是基于springBoot去实现的，springCloud关注于全局的微服务整合和管理，将多个springBoot单体微服务进行整合以及管理；  springCloud依赖于springBoot开发，而springBoot可以独立开发；

#### SpringBoot中用那些注解
     @EnableAutoConfiguration作用：自动扫描并添加jar包依赖
     @SpringBootApplication原理：是一个组合注解，相当于@EnableAutoConfiguration和@ComponentScan
#### SpringBoot热部署使用什么？
devtools

#### 你们项目中异常是如何处理
在web项目中，使用全局捕获异常返回统一错误信息。

#### SpringBoot如何实现异步执行
在启动类添加@EnableAsync表示开启对异步任务的支持，在异步服务上添加@Async

#### SpringBoot多数据源拆分的思路
先在properties配置文件中配置两个数据源，创建分包mapper，使用@ConfigurationProperties读取properties中的配置，使用@MapperScan注册到对应的mapper包中

#### SpringBoot多数据源事务如何管理
    第一种方式是在service层的@TransactionManager中使用transactionManager指定DataSourceConfig中配置的事务
    第二种是使用jta-atomikos实现分布式事务管理
#### SpringBoot如何实现打包
进入项目目录在控制台输入mvn clean package，clean是清空已存在的项目包，package进行打包
#### SpringBoot性能如何优化
如果项目比较大，类比较多，不使用@SpringBootApplication，采用@Compoment指定扫包范围
在项目启动时设置JVM初始内存和最大内存相同
将springboot内置服务器由tomcat设置为undertow

#### SpringBoot执行流程
使用SpringApplication.run()启动，在该方法所在类添加@SpringBootApplication注解，该注解由@EnableAutoConfiguration和@ComponentScan等注解组成，@EnableAutoConfiguration自动加载SpringBoot配置和依赖包，默认使用@ComponentScan扫描当前包及子包中的所有类，将有spring注解的类交给spring容器管理

#### SpringBoot底层实现原理
使用maven父子包依赖关系加载相关jar包，使用java操作Spring的初始化过程生成class文件，然后用java创建tomcat服务器加载这些class文件

#### SpringBoot装配Bean的原理
     通过@EnableAutoConfiguration自动获取配置类信息，使用反射实例化为spring类，然后加载到spring容器
#### springboot的版本问题
    springboot 1.5  搭配spring 4
    springboot 2.0.2  搭配spring 5
