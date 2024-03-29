### RPC

> RPC 和 HTTP 都是在应用层工作，
>
> 其实RPC就是把拦截到的方法参数，转成可以在网络中传输的二进制，并保证在服务提供方能正确地还原出语义，最终实现像调用本地一样地调用远程的目的。

#### 基础

##### 协议

##### 序列化

​	在 RPC 通信流程时，网络传输的数据必须是二进制数据，但调用方请求的出入参数都是对象。所以我们需要把**对象**转成可传输的**二进制**，并要求转换算法是可逆的，这个过程我们一般叫做`序列化`。这时，服务提供方就可以正确地从**二进制数据**中分割出不同的请求，同时根据请求类型和序列化类型，把二进制的消息体逆向还原成请求**对象**，这个过程称之为`反序列化`。

​	实际上任何一种序列化框架，核心思想就是设计一种序列化协议，将对象的类型、属性类型、属性值一一按照固定的格式写到二进制字节楼中完成序列化，再按照固定的格式读出对象的类型、属性类型、属性值，通过这些信息重新创建出一个新的对象，来完成反序列化。

序列化框架有如下：

- jdk自带的**Serializable**；

- **JSON**：json是典型的key-value方式，没有数据类型，是一种文本型序列化数据。**缺点：** 1. json进行序列化的额外空间开销比较大，对于大数据量服务意味着需要巨大的内存和磁盘开销 2. json没有类型，像Java强类型玉兴，需要通过反射统一解决，所以性能不太好；
- **Hessian**：hessian是动态类型、二进制、紧凑的，并且可跨语言移植的一种序列化框架。性能上要比JDK，JSON序列化高效很多，并且生成的字节数也更小。但Hessian本身也有问题，官方版本对Java里常见的对象类型不支持，例如：Linked系列，LinkedHashMap、LinkedHashSet等，Locale类，Byte/Short反序列化的时候变成Integer等。
- **Protobuf**：google公司内部的混合语言数据标准，是一种轻便、高效的结构化数据存储的格式，可以用户结构化数据序列化，支持Java、Python、C++，Go等。Protobuf 使用的时候需要定义 *IDL（interface description language）*，然后使用不同语言的IDL编译器，生成序列化工具类，它的优点是：
    - 序列化后体积相比JSON、Hessian小很多；
    - IDL 能清晰地描述语义，所以足以帮助并保证应用程序之间的类型不会丢失，无需类似XML解析器；
    - 序列化反序列化速度很快，不需要通过反射获取类型；
    - 消息格式升级和兼容性不错，可以做到向后兼容。

> 总结：在实际选择 RPC 序列协议时，需要考虑`性能`和`效率`，还有`空间开销`，也就是序列化之后的二进制数据的体积大小。序列化后的自己数据体积越小，网络传输的数据量也就越小，传输数据的速度也就越快。还有更重要的就是序列化的`通用性`和`兼容性`。

##### 网络通信

##### 动态代理

#### RPC 需要考虑的问题

##### 服务发现

👻问题：很多 rpc 的注册中心都会使用 zookeeper，zookeeper的一大特点就是强一致性，zk 集群中的每个节点的数据发生更新时，都会通知其它 zk 节点同时执行更新。它要求每个节点的数据能够实时的完全一致，这也就导致了 zk 集群性能上的下降。

在超大规模的服务集群下，注册中心所面临的挑战就是超大批量服务节点同时上下线，注册中心集群接受到大量服务变更请求，集群间各节点需要同步大量服务节点数据，最终导致如下问题：

- 注册中心负载过高
- 各节点数据不一致
- 服务下发不及时或下发错误的服务列表

RPC 框架依赖的注册中心的服务数据的一致性并不需要 CP，只需要满足 AP 注册中心数据的最终一致性即可。

> 在实现 RPC 时，在考虑高可用的情况下，服务提供方都是以集群的方式对外提供服务，而调用者需要及时获取到对应的服务提供方的节点，这个获取过程一般叫做**服务发现**。

1. 服务注册：在服务提供方启动的时候，将对外暴露的接口注册到注册中心，注册中心将这个服务节点的 IP 和接口保存下来。
2. 服务订阅：在服务调用方启动的时候，去注册中心查找并订阅服务提供方的 IP，然后缓存到本地，并用于后续的远程调用。

##### 路由策略

> 前言：在大型应用中，服务提供方是以一个集群的方式提供服务，这对于服务调用方来说，就是一个接口会有多个服务提供方同时提供服务，无论调用方此次请求发送到集群中的哪个节点上，返回的结果都是一样的。但上线新应用时，会选择灰度发布以减少应用不稳定的风险，根据观察后续情况，选择发布更多实例还是回滚已经上线的实例。

为了降低事故发生的概率，要让基础设施能支持更低的试错成本。灰度发布功能作为 RPC 路由功能的一个典型应用场景，我们可以通过路由功能完成像定点调用、黑白名单等一些高级服务治理功能。在 RPC 中，路由策略的核心思想就是让请求按照设定的规则发送到目标节点上，从而实现流量隔离的效果。

##### 负载均衡

##### 异常重试

> 在 RPC 框架中，可能由于网络抖动，导致服务调用者调用服务提供者的请求失败，所以`重试机制`也是 RPC 需要考虑的一个点。

设计重试机制时需要考虑：

1. 重试次数（确保被调用的服务的业务逻辑本身是`幂等`）
2. 异常重试策略，特定异常重试，比如连接异常、超时异常，最好支持可配置。
3. 重试时间（判断请求是否超时，超时返回超时异常，否则重置超时时间等）

##### 熔断限流

业务实现自我保护