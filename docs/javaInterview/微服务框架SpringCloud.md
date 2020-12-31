> 九、**微服务框架SpringCloud**

- [微服务框架SpringCloud](#微服务框架springcloud)
    - [请说一下你们项目使用到的springcloud中所用的组件？](#请说一下你们项目使用到的springcloud中所用的组件)
    - [什么是SpringCloud](#什么是springcloud)
    - [为什么要使用SpringCloud](#为什么要使用springcloud)
    - [SpringCloud服务注册发现原理](#springcloud服务注册发现原理)
    - [SpringCloud 支持那些注册中心](#springcloud-支持那些注册中心)
    - [如果现在Eureka闭源了，可以通过什么注册中心替代Eureka呢？](#现在eureka闭源了可以通过什么注册中心替代eureka呢)
    - [谈谈你对微服务服务治理的思想](#谈谈你对微服务服务治理的思想)
    - [Eureka如何实现高可用](#eureka如何实现高可用)
    - [@LoadBalanced注解的作用](#loadbalanced注解的作用)
    - [Nginx与Ribbon的区别](#nginx与ribbon的区别)
    - [Ribbon底层实现原理](#ribbon底层实现原理)
    - [SpringCloud有几种调用接口方式](#springcloud有几种调用接口方式)
    - [DiscoveryClient的作用](#discoveryclient的作用)
    - [谈谈服务雪崩效应](#谈谈服务雪崩效应)
    - [在微服务中，如何保护服务?](#在微服务中如何保护服务)
    - [服务雪崩效应产生的原因](#服务雪崩效应产生的原因)
    - [谈谈Hystrix服务保护的原理](#谈谈hystrix服务保护的原理)
    - [谈谈服务降级、熔断、服务隔离](#谈谈服务降级熔断服务隔离)
    - [服务降级底层是如何实现的？](#服务降级底层是如何实现的)
    - [分布式配置中心有那些框架？](#分布式配置中心有那些框架)
    - [分布式配置中心的作用？](#分布式配置中心的作用)
    - [SpringCloud Config 可以实现实时刷新吗？](#springcloud-config-可以实现实时刷新吗)
    - [什么是网关?](#什么是网关)
    - [网关的作用是什么](#网关的作用是什么)
    - [网关与过滤器有什么区别](#网关与过滤器有什么区别)
    - [常用网关框架有那些？](#常用网关框架有那些)
    - [Zuul与Nginx有什么区别？](#zuul与nginx有什么区别)
    - [既然Nginx可以实现网关？为什么还需要使用Zuul框架](#既然nginx可以实现网关为什么还需要使用zuul框架)
    - [如何设计一套API接口](#如何设计一套api接口)
    - [ZuulFilter常用有那些方法](#zuulfilter常用有那些方法)
    - [如何实现动态Zuul网关路由转发](#如何实现动态zuul网关路由转发)
    - [Zuul网关如何搭建集群](#zuul网关如何搭建集群)
____
### 微服务框架SpringCloud
#### 请说一下你们项目使用到的springcloud中所用的组件？
    Eureka 注册中心
    Ribbon 本地负载均衡
    Fegin 服务调用
    RestTemplent 服务调用
    Hystrix 熔断降级
    Zuul 网关
    链路追踪
    配置中心
#### 什么是SpringCloud
	SpringCloud是微服务的一种解决方案，依赖SpringBoot实现。包含注册中心(eureka)、客户端负载均衡(Ribbon)、网关(zull)、分布式锁、分布式会话等。

#### 为什么要使用SpringCloud
	SpringCloud是一套非常完整的微服务解决方案，俗称“微服务全家桶”，几乎内置了微服务所使用的各种技术，可以不必集成第三方依赖。

#### SpringCloud服务注册发现原理
	每个SpringCloud服务器启动后向注册中心注册本服务器信息，如服务别名、服务器IP、端口号等，其他服务进行请求时先根据服务别名从注册中心获取到目标服务器IP和端口号，并将获取到的信息缓存到本地，然后通过本地使用HttpClient等技术进行远程调用。
#### SpringCloud 支持那些注册中心
	Eureka、Consul、Zookeeper
#### 现在Eureka闭源了，可以通过什么注册中心替代Eureka呢？
	Consul或Zookeeper
#### 谈谈你对微服务服务治理的思想

#### Eureka如何实现高可用
	启动多台Eureka服务器，然后作为SpringCloud服务互相注册，客户端从Eureka集群获取信息时，按照注册的Eureka顺序对第一个Eureka进行访问。
#### @LoadBalanced注解的作用
	开启客户端负载均衡。
#### Nginx与Ribbon的区别

	Nginx是反向代理同时可以实现负载均衡，nginx拦截客户端请求采用负载均衡策略根据upstream配置进行转发，相当于请求通过nginx服务器进行转发。
	Ribbon是客户端负载均衡，从注册中心读取目标服务器信息，然后客户端采用轮询策略对服务直接访问，全程在客户端操作。

#### Ribbon底层实现原理
	Ribbon使用discoveryClient从注册中心读取目标服务信息，对同一接口请求进行计数，使用%取余算法获取目标服务集群索引，返回获取到的目标服务信息。

#### SpringCloud有几种调用接口方式
	使用Feign和RestTemplate

#### DiscoveryClient的作用
	可以从注册中心中根据服务别名获取注册的服务器信息。
#### 谈谈服务雪崩效应
      雪崩效应是在大型互联网项目中，当某个服务发生宕机时，调用这个服务的其他服务也会发生宕机，大型项目的微服务之间的调用是互通的，这样就会将服务的不可用逐步扩大到各个其他服务中，从而使整个项目的服务宕机崩溃.发生雪崩效应的原因有以下几点
      1.单个服务的代码存在bug.
      2请求访问量激增导致服务发生崩溃(如大型商城的枪红包，秒杀功能). 
      3.服务器的硬件故障也会导致部分服务不可用.
#### 在微服务中，如何保护服务?
一般使用使用Hystrix框架，实现服务隔离来避免出现服务的雪崩效应，从而达到保护服务的效果。当微服务中，高并发的数据库访问量导致服务线程阻塞，使单个服务宕机，服务的不可用会蔓延到其他服务，引起整体服务灾难性后果，使用服务降级能有效为不同的服务分配资源,一旦服务不可用则返回友好提示，不占用其他服务资源，从而避免单个服务崩溃引发整体服务的不可用.

#### 服务雪崩效应产生的原因
	因为Tomcat默认情况下只有一个线程池来维护客户端发送的所有的请求，这时候某一接口在某一时刻被大量访问就会占据tomcat线程池中的所有线程，其他请求处于等待状态，无法连接到服务接口。

#### 谈谈Hystrix服务保护的原理
    通过服务降级、服务熔断、服务隔离为高并发服务提供保护。
#### 谈谈服务降级、熔断、服务隔离
    服务降级：当客户端请求服务器端的时候，防止客户端一直等待，不会处理业务逻辑代码，直接返回一个友好的提示给客户端。
    服务熔断是在服务降级的基础上更直接的一种保护方式，当在一个统计时间范围内的请求失败数量达到设定值（requestVolumeThreshold）或当前的请求错误率达到设定的错误率阈值（errorThresholdPercentage）时开启断路，之后的请求直接走fallback方法，在设定时间（sleepWindowInMilliseconds）后尝试恢复。
    服务隔离就是Hystrix为隔离的服务开启一个独立的线程池，这样在高并发的情况下不会影响其他服务。服务隔离有线程池和信号量两种实现方式，一般使用线程池方式。
#### 服务降级底层是如何实现的？
	Hystrix实现服务降级的功能是通过重写HystrixCommand中的getFallback()方法，当Hystrix的run方法或construct执行发生错误时转而执行getFallback()方法。

#### 分布式配置中心有那些框架？
    Apollo(阿波罗)、zookeeper、springcloud config。

#### 分布式配置中心的作用？
	动态变更项目配置信息而不必重新部署项目。
#### SpringCloud Config 可以实现实时刷新吗？
	springcloud config实时刷新采用SpringCloud Bus消息总线。

#### 什么是网关?
	网关相当于一个网络服务架构的入口，所有网络请求必须通过网关转发到具体的服务。

#### 网关的作用是什么
	统一管理微服务请求，权限控制、负载均衡、路由转发、监控、安全控制黑名单和白名单等

#### 网关与过滤器有什么区别
	网关是对所有服务的请求进行分析过滤，过滤器是对单个服务而言。

#### 常用网关框架有那些？
	Nginx、Zuul、Gateway

#### Zuul与Nginx有什么区别？
	Zuul是java语言实现的，主要为java服务提供网关服务，尤其在微服务架构中可以更加灵活的对网关进行操作。Nginx是使用C语言实现，性能高于Zuul，但是实现自定义操作需要熟悉lua语言，对程序员要求较高，可以使用Nginx做Zuul集群。

#### 既然Nginx可以实现网关？为什么还需要使用Zuul框架
	Zuul是SpringCloud集成的网关，使用Java语言编写，可以对SpringCloud架构提供更灵活的服务。

#### 如何设计一套API接口
	考虑到API接口的分类可以将API接口分为开发API接口和内网API接口，内网API接口用于局域网，为内部服务器提供服务。开放API接口用于对外部合作单位提供接口调用，需要遵循Oauth2.0权限认证协议。同时还需要考虑安全性、幂等性等问题。

#### ZuulFilter常用有那些方法
	Run()：过滤器的具体业务逻辑
	shouldFilter()：判断过滤器是否有效
	filterOrder()：过滤器执行顺序
	filterType()：过滤器拦截位置

#### 如何实现动态Zuul网关路由转发
	通过path配置拦截请求，通过ServiceId到配置中心获取转发的服务列表，Zuul内部使用Ribbon实现本地负载均衡和转发。

#### Zuul网关如何搭建集群
	使用Nginx的upstream设置Zuul服务集群，通过location拦截请求并转发到upstream，默认使用轮询机制对Zuul集群发送请求。