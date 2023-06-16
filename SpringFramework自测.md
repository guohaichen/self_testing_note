### Spring Framework

#### Spring

##### 什么是Spring框架？它的主要功能是什么？

> Spring是一个开源，轻量级的Java开发框架， Spring和其他单层框架不同，Spring提供一个统一的，高效的方式构造整个应用，并且将这些单层框架组合在一起建立一个连贯的体系。主要是为了使Java EE 开发更加容易；

##### Spring的核心模块是哪些？分别介绍它们的作用。

定期口述；

##### 什么是IoC（控制反转）和依赖注入（Dependency Injection）？它们有什么区别？

> IOC inversion of controller控制反转，它是一种**设计思想**，指将对象的创建的控制权由用户本身交给Spring容器管理；
>
> 将对象的依赖关系交由spring容器管理，并由**IOC容器完成对象的注入**。对象实例通常以`哈希表`的形式存放，其中 Key 和 Value 分别表示对象的标识符和实际的对象实例。
>
> **DI依赖注入是IOC的一种实现方式**，它是指通过外部方式将依赖关系注入到该对象中。

##### Spring的Bean是什么？如何定义Bean？Bean的生命周期

> Bean 代指的就是那些被 IoC 容器所管理的对象。
>
> 可以通过xml配置文件，配置类，注解的方式来定义Bean。
>
> **Bean的生命周期**：
>
> 1. `实例化`: 在这个阶段，Spring容器根据配置或注解等方式示例化一个Bean。实例化的方式可以是通过构造含函数创建一个新的对象，或者通过工厂方法来获取对象示例。
>
> 2. `属性赋值`: 在实例化化之后，Spring容器会将配置的属性值或以来注入到Bean中。这包括设置对象的属性、注入以来对象和调用相关的初始化方法。
>
> 3. `初始化`：在属性赋值后，Spring容器会调用Bean的初始化方法。
>
> 4. `使用`：在初始化之后，Bean被放置在Spring容器中供应用程序使用。
>
> 5. `销毁`：在应用程序关闭或者手动销毁Bean时，Spring容器会调用Bean的销毁方法。
>
> ![img](./SpringFramework自测.assets/07fd8589e54e6d3dbcb371f93448611f.png)

##### Bean的作用域

> `singleton`: 默认的作用域，Spring容器只会创建一个Bean的实例，并且该示例会被**共享和重用**。（且默认是**饿汉式加载**，再Spring启动时就会创建并初始化所有的Bean。可以通过@Lazy将Bean设置为懒加载。）
>
> `prototype`：原型作用域，每次请求都会创建**新的实例bean**。
>
> `application`:在一个Http servlet Context中，定义一个Bean实例。
>
> `request`：每次Http请求会创建新的Bean实例，一次Http请求和响应共享Bean。
>
> `session`：在一个Http session中，定义一个Bean实例，用户会话共享Bean。

##### Spring AOP是什么？它的作用和应用场景是什么？





##### Spring事务管理是如何实现的？介绍Spring事务的传播行为和隔离级别。





#### Spring MVC

##### 什么是Spring MVC框架？它的主要组件是什么？

> Spring MVC框架主要目标是将应用程序的不同层进行解耦，将应用程序划分为==模型Model==、==视图View==和==控制器Controller==。
>
> - Model：表示应用程序的数据模型，负责封装和处理数据。通常是实体类。
> - View：负责展示数据，并处理交互。通常由JSP、Thymeleaf、Freemaker等模板引擎生成的动态页面。
> - Controller：处理用于请求并协调模型和视图之间的交互。控制器负责接受用户的请求，调用实当的业务逻辑处理请求，并将结果返回。
>
> 主要组件：
>
> - DispatcherServlet:核心中央处理器，负责接受请求、分发、并给予客户端响应。
> - HandlerMapping:负责将请求(url)映射到相应的处理器。比如控制器类中使用@RequestMapping注解来映射请求路径。
> - HandlerAdapter:负责执行具体的处理器方法，将请求的参数绑定到方法的参数中，并处理业务逻辑。例如（addUser方法中的参数）
> - ViewResolver:视图解析器，根据Handler返回的逻辑视图，解析并渲染真正的视图。例如"redirect:/user/list" 逻辑视图重定向到/user/list。再将逻辑视图解析为具体的视图对象。
> - View: Dispatcher把返回的Model传给View（视图渲染）并生成最终的HTMl或其他格式的响应。

##### Spring MVC的请求处理流程是怎样的？

流程见上Spring MVC的主要组件，结合主要组件说。

| ![img](./SpringFramework自测.assets/de6d2b213f112297298f3e223bf08f28.png) |
| ------------------------------------------------------------ |

##### 什么是控制器（Controller）？如何定义和使用控制器？

> 控制器是处理用户请求并返回响应的组件。控制器负责接受请求、处理业务逻辑、调用相应的服务或dao方法，并最总返回数据或视图给客户端。
>
> 定义和使用控制器：
>
> 1. 定义一个控制器类 UserController;并使用@Controller标记；
> 2. 定义处理接受的请求方法，如getUser，并使用@RequestMapping标记；
> 3. 调用相应的服务，如getUserDao,并最终生成响应并返回，通常是返回一个视图或数据给客户端。

##### 如何处理异常和错误情况？介绍Spring MVC的异常处理机制。

#### SpringBoot

##### 什么是SpringBoot？它与传统的Spring框架有何不同？

##### Spring Boot的核心特点是什么？为什么使用SpringBoot？

##### 如何创建一个SpringBoot应用程序？需要哪些配置和依赖？

##### 介绍@SpringBootApplication注解

> @SpringBootApplication是一个合成注解，包含`@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan`；作用分别是
>
> @Configuration: 允许在上下文中注册额外的bean或导入其他配置类；
>
> @EnableAutoConfiguration: 启用SpringBoot自动装配机制；
>
> @ComponentScan：扫描该类包所在的包下所有的类；

##### SpringBoot的自动配置是如何工作的？如何自定义和禁用自动配置？

##### 什么是SpringBoot Starter？如何使用和创建自定义的Starter？

##### SpringBoot如何处理外部配置和属性文件？

#### SpringCloud ALIBABA

##### Nacos

**nacos的核心功能**

> nacos 全称`Dynamic Naming and Configuration service` 一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
>
> 提供了三大功能：
>
> - 服务注册与发现：服务提供者将自己的服务实例注册到Nacos中，服务消费者可以通过Nacos获取可用的服务实例列表。
> - 配置管理：Nacos提供了统一的配置管理功能，可以动态管理和推送配置，配置热更新，支持多种数据格式，如properties，yml等。

Nacos的服务注册面板，其中nacos的服务名为boot程序配置文件中spring.application.name；
| ![image-20230616141255031](./SpringFramework自测.assets/image-20230616141255031.png) |
| ------------------------------------------------------------ |

Nacos的配置列表面板，其中`Data Id`为**${prefix服务名}-${spring.profiles.active环境}.${file-extension后缀}**。

| ![image-20230616142257177](./SpringFramework自测.assets/image-20230616142257177.png) |
| ------------------------------------------------------------ |

Nacos的配置管理流程：

1. 注册配置:配置文件在Nacos中注册；
2. 服务订阅：服务订阅指定的配置，例如订阅符合Data Id的配置；
3. 配置拉取：服务定期从Nacos配置中心拉取配置。（通过与配置中心**建立长连接并监听配置**变化实现的。当配置发生变化时，Nacos会**推送通知**给订阅的服务，服务收到通知发起**配置拉取**。）@RefreshScope/@ConfigurationProperties两者都可以实现自动刷配置热更新。
4. 配置更新：服务更新自身的配置；

**配置文件优先级：**{服务名}-{active环境}.{文件名} > {服务名}.{文件名} > 本地配置；

##### Gateway

##### OpenFeign

##### Sentinel

##### Seata分布式事务