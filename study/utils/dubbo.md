## 简介
	
	Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。

> 其核心部分包含:

+ **远程通讯**: 提供对多种基于长连接的NIO框架抽象封装，包括多种线程模型，序列化，以及“请求-响应”模式的信息交换方式。
+ **集群容错**: 提供基于接口方法的透明远程过程调用，包括多协议支持，以及软负载均衡，失败容错，地址路由，动态配置等集群支持。
+ **自动发现**: 基于注册中心目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

## 结构

![Alt dubbo_1](../../_media/dubbo/dubbo_1.png)

|节点|	角色规范|
|-----------|--------|
|Provider|	提供者公开远程服务|
|Consumer|	消费者呼叫远程服务|
|Registry|	注册表负责服务发现和配置|
|Monitor|	监视器计算服务调用的数量并且耗时|
|Container|	容器管理服务的生命周期|

> 服务关系

1. Container负责启动，加载和运行服务Provider。
2. ProviderRegister在其启动时注册其服务。
3. Consumer订阅它从Register启动时所需的服务。
4. Register返回Providers列表Consumer，当它更改时，Register将Consumer通过长连接推送更改的数据。
5. Consumer选择一个Provider基于软负载均衡算法的s并执行调用，如果失败，则选择另一个Provider。
6. 双方Consumer并Provider会数数的服务调用和耗时的内存，并发送统计Monitor每一分钟。

## 使用

**创建三个项目**
+ dubbo-demo-api ：公共服务api
+ dubbo-demo-provider：演示提供商代码
+ dubbo-demo-consumer：演示消费者代码

### 服务端

> dubbo-demo-api 定义服务接口

DemoService.java 

	package org.apache.dubbo.demo;

	public interface DemoService {
	    String sayHello(String name);

	}

_dubbo-demo-api 项目目录结构如下_

	.
	├── dubbo-demo-api
	│   ├── pom.xml
	│   └── src
	│       └── main
	│           └── java
	│               └── org
	│                   └── apache
	│                       └── dubbo
	│                           └── demo
	│                               └── DemoService.java

> dubbo-demo-provider 实现服务接口

DemoServiceImpl.java
	
	package org.apache.dubbo.demo.provider;
	import org.apache.dubbo.demo.DemoService;

	public class DemoServiceImpl implements DemoService {
	    public String sayHello(String name) {
	        return "Hello " + name;
	    }
	}

> 使用Spring配置公开服务

dubbo-demo-provider.xml

	<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

    <!-- provider's application name, used for tracing dependency relationship -->
    <dubbo:application name="demo-provider"/>
    <!-- use multicast registry center to export service -->
    <dubbo:registry address="multicast://224.5.6.7:1234"/>
    <!-- use dubbo protocol to export service on port 20880 -->
    <dubbo:protocol name="dubbo" port="20880"/>
    <!-- service implementation, as same as regular local bean -->
    <bean id="demoService" class="org.apache.dubbo.demo.provider.DemoServiceImpl"/>
    <!-- declare the service interface to be exported -->
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService"/>
	</beans>

该演示使用多播作为注册表，因为它很简单，不需要额外的安装。如果您喜欢像zookeeper这样的注册，请在这里查看示例。

>  提供服务

Provider.java		
	
	package org.apache.dubbo.demo.provider;

	import org.springframework.context.support.ClassPathXmlApplicationContext;

	public class Provider {

	    public static void main(String[] args) throws Exception {
	        System.setProperty("java.net.preferIPv4Stack", "true");
	        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"META-INF/spring/dubbo-demo-provider.xml"});
	        context.start();
	        System.out.println("Provider started.");
	        System.in.read(); // press any key to exit
	    }
	}


### 客户端 dubbo-demo-consumer

> 使用Spring引用远程服务
dubbo-demo-consumer.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	       xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
	       xmlns="http://www.springframework.org/schema/beans"
	       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
	       http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd">

	    <!-- consumer's application name, used for tracing dependency relationship (not a matching criterion),
	    don't set it same as provider -->
	    <dubbo:application name="demo-consumer"/>
	    <!-- use multicast registry center to discover service -->
	    <dubbo:registry address="multicast://224.5.6.7:1234"/>
	    <!-- generate proxy for the remote service, then demoService can be used in the same way as the
	    local regular interface -->
	    <dubbo:reference id="demoService" check="false" interface="org.apache.dubbo.demo.DemoService"/>
	</beans>

> 消费服务
Consumer.java

	import org.springframework.context.support.ClassPathXmlApplicationContext;
	import org.apache.dubbo.demo.DemoService;
	 
	public class Consumer {
	    public static void main(String[] args) throws Exception {
	        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(new String[] {"META-INF/spring/dubbo-demo-consumer.xml"});
	        context.start();
	        // Obtaining a remote service proxy
	        DemoService demoService = (DemoService)context.getBean("demoService");
	        // Executing remote methods
	        String hello = demoService.sayHello("world");
	        // Display the call result
	        System.out.println(hello);
	    }
	}

	启动Provider
	启动Provider

	打印 Hello world

## 特性

### 服务分组与版本号

通用xml

	<bean id="demoService1" class="org.apache.dubbo.demo.provider.DemoServiceImpl1"/>
	<bean id="demoService2" class="org.apache.dubbo.demo.provider.DemoServiceImpl2"/>

+ 同一接口的两种不同实现

> Provider
	
    
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService1" group="impl1"/>
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService2" group="impl2"/>

> Consumer

	<dubbo:reference id="demoService1" check="false" interface="org.apache.dubbo.demo.DemoService" group="impl1"/>
	<dubbo:reference id="demoService2" check="false" interface="org.apache.dubbo.demo.DemoService" group="impl2"/>

+ 同一分组版本不兼容,过渡升级

> Provider
	
    
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService1" group="impl" version="1.0/>
    <dubbo:service interface="org.apache.dubbo.demo.DemoService" ref="demoService2" group="impl" version="2.0/>

> Consumer

	<dubbo:reference id="demoService1" check="false" interface="org.apache.dubbo.demo.DemoService" group="impl" version="1.0/>
	<dubbo:reference id="demoService2" check="false" interface="org.apache.dubbo.demo.DemoService" group="impl" version="2.0/>


### 服务直连

为了方便开发及测试，一般需要绕过注册中心，只测试指定ip的服务提供者，这时候服务消费方和服务提供方就是点对点直联方式。这时候服务消费方会忽略注册中心的提供者列表。另外直连方式以服务接口为单位，假如A 接口配置点对点，不影响 B 接口从注册中心获取列表。

**直连方法(只在测试阶段使用)**

> 通过-D参数指定

在服务消费进程启动时候 JVM 启动参数中加入-D参数映射服务地址 ，如：`-Dcom.test.UserServiceBo=dubbo://30.8.59.182:20880`；则表示当调用com.test.UserServiceBo接口时候访问30.8.59.182:20880提供的服务，忽略zk发现列表。

> 通过 XML 配置

如果是xml方式需要点对点，可在 <dubbo:reference> 中配置 url 指向提供者，将绕过注册中心，多个地址用分号隔开，配置如下：

	<dubbo:reference id="userService" interface="com.test.UserServiceBo" group="dubbo" version="1.0.0" timeout="3000" url="dubbo://30.8.59.182:20880"/>

> 通过文件映射

如果服务比较多，也可以用文件映射，用 -Ddubbo.resolve.file 指定映射文件路径，此配置优先级高于 <dubbo:reference> 中的配置 ，如：
	
	java -Ddubbo.resolve.file=xxx.properties

然后在映射文件 xxx.properties 中加入配置，其中 key 为服务名，value 为服务提供者
URL：

	com.test.UserServiceBo=dubbo://30.8.59.182:20880

### 消费端泛化调用

### 消费端异步调用
	
无论是正常调用还是泛化调用也好，都是进行同步调用的，也就是服务消费方发起一个远程调用后，调用线程要被阻塞挂起，直到服务提供方返回。本节来讲解下异步调用，异步调用是指服务消费方发起一个远程调用后，不等服务提供方返回结果，调用方法就返回了，也就是当前线程不会被阻塞，这就允许调用方同时调用多个远程方法。	



## 原理

参照: [Dubbo-从入门到深入](http://ifeve.com/dubbo-learn-book/)