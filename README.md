## 介绍
API ？
- API，英文全称Application Programming Interface，翻译为“应用程序编程接口”。是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节！

即有人写了一个函数可以实现某个功能，你需要这个功能，那只需要调用那个函数即可，无序需知道其底层实现! 那个函数名就是API

网关 ？
- 网关一个大概念，不具体特指一类产品，只要连接两个不同的网络的设备都可以叫网关；而‘路由器’一般特指能够实现路由寻找和转发的特定类产品，路由器很显然能够实现网关的功能。
默认网关是什么，默认网关事实上不是一个产品而是一个网络层的概念，PC本身不具备路由寻址能力，所以PC要把所有的IP包发送到一个默认的中转地址上面进行转发，也就是默认网关。这个网关可以在路由器上，可以在三层交换机上，可以在防火墙上，可以在服务器上，所以和物理的设备无关。

## 参考文献
[API网关作用](https://www.jianshu.com/p/8cbf97b40c26)

## 什么是API网关
网关一词最早出现在网络设备，比如两个相互独立的局域网之间通过路由器进行通信，中间的路由器就被称为网关
任何一个应用系统如果需要被其他系统调用，就需要暴露API，这些API代表着一个一个功能点
如果两个系统中间通信，在系统之间加上一个中介者协助API的调用，这个中介者就是API网关

![api网关](https://img-blog.csdnimg.cn/20200727174832704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

对接两个系统的 API 网关
当然，API 网关可以放在两个系统之间，同时也可以放在客户端与服务端之间。

![api1](https://img-blog.csdnimg.cn/2020072717493337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

为何要使用 API 网关
- 网关作为系统的唯一入口，也就是说，进入系统的所有请求都需要经过 API 网关。
- 当系统外部的应用或者客户端访问系统的时候，都会遇到这样的情况：
- 系统要判断它们的权限
- 如果传输协议不一致，需要对协议进行转换
- 如果调用水平扩展的服务，需要做负载均衡
- 一旦请求流量超出系统承受的范围，需要做限流操作
- 针对每个请求以及回复，系统会记录响应的日志
- 也就是说，只要是涉及到对系统的请求，并且能够从业务中抽离出来的功能，都有可能在网关上实现。

例如：协议转换，负载均衡，请求路由，流量控制等等。

## API网关架构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727175851668.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)


API 网关拆分成为 3 个系统：
- Gateway-Core（核心）
- Gateway-Admin（管理）
- Gateway-Monitor（监控）

Gateway-Core 核心网关，负责接收客户端请求，调度、加载和执行组件，将请求路由到上游服务端，并处理其返回的结果。

大多数的功能都在这一层完成，例如：验证，鉴权，负载均衡，协议转换，服务路由，数据缓存。如果没有其他两个子系统，它也是可以单独运行的。

Gateway-Admin 网关管理界面，可以进行 API、组件等系统基础信息的配置；例如：限流的策略，缓存配置，告警设置。

Gateway-Monitor 监控日志、生成各种运维管理报表、自动告警等；管理和监控系统主要是为核心系统服务的，起到支撑的作用。

## API网关技术原理
### 协议转换
如果解决这个问题呢？API 网关通过泛化调用的方式实现协议之间的转化。

实际上就是将不同的协议转换成“通用协议”，然后再将通用协议转化成本地系统能够识别的协议。

这一转化工作通常在 API 网关完成。通用协议用得比较多的有 JSON，当然也有使用 XML 或者自定义 JSON 文件的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727203036510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

### 链式处理
设计模式中有一种责任链模式，它将“处理请求”和“处理步骤”分开。每个处理步骤，只关心这个步骤上需要做的处理操作，处理步骤存在先后顺序。

消息从第一个“处理步骤”流入，从最后一个“处理步骤”流出，每个步骤对经过的消息进行处理，整个过程形成了一个链条。在 API 网关中也用到了类似的模式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727203252316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

- PRE：前置过滤器，用来处理通用事务，比如鉴权，限流，熔断降级，缓存。并且可以通过 Custom 过滤器进行扩展。

- ROUTING：路由过滤器，在这种过滤器中把用户请求发送给 Origin Server。它主要负责：协议转化和路由的工作。

- POST：后置过滤器，从 Origin Server 返回的响应信息会经过它，再返回给调用者。在返回的 Response 上加入 Response Header，同时可以做 Response 的统计和日志记录。

- ERROR：错误过滤器，当上面三个过滤器发生异常时，错误信息会进到这里，并对错误进行处理。

## API网关实现的功能
#### 负载均衡
当网关后面挂接同一应用的多个副本时，每次用户的请求都会通过网关的负载均衡算法，路由到对应的服务上面。例如：随机算法，权重算法，Hash 算法等等。如果上游服务采取微服务的架构，也可以和注册中心合作实现动态的负载均衡。

#### 路由选择
这个不言而喻，网关可以根据请求的 URL 地址解析，知道需要访问的服务。再通过路由表把请求路由到目标服务上去。

有时候因为网络原因，服务可能会暂时的不可用，这个时候我们希望可以再次对服务进行重试。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727205638845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

#### 流量控制
限流是 API 网关常用的功能之一，当上游服务超出请求承载范围，或者服务因为某种原因无法正常使用，都会导致服务处理能力下滑。这个时候，API 网关作为“看门人”，就可以限制流入的请求，让应用服务器免受冲击。

限流实际上就是限制流入请求的数量，其算法不少，有令牌桶算法，漏桶算法，连接数限制等等。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727205728903.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

#### 统一鉴权
访问应用服务器的请求都需要拥有一定权限，如果说每访问一个服务都需要验证一次权限，这个对效率是很大的影响。可以把权限认证放到 API 网关来进行。目前比较常见的做法是，用户通过登录服务获取 Token，把它存放到客户端，在每次请求的时候把这个 Token 放入请求头，一起发送给服务器。API 网关要做的事情就是解析这个 Token，知道访问者是谁（鉴定），他能做什么/访问什么（权限）。说白了就是看访问者能够访问哪些 URL，这里根据权限/角色定义一个访问列表。如果要实现多个系统的 OSS（Single Sign On 单点登录），API 网关需要和 CAS（Central Authentication Service 中心鉴权服务）做连接，来确定请求者的身份和权限。

#### 熔断降级
当应用服务出现异常，不能继续提供服务的时候，也就是说应用服务不可用了。作为 API 网关需要做出处理，把请求导入到其他服务上。或者对服务进行降级处理，例如：用兜底的服务数据返回客户端，或者提示服务暂时不可用。同时通过服务注册中心，监听存在问题的服务，一旦服务恢复，随即恢复路由请求到该服务。

- 服务熔断：
          一般是指软件系统中，由于某些原因使得服务出现了过载现象，为防止造成整个系统故障，从而采用的一种保护措施，所以很多地方把熔断亦称为过载保护。很多时候刚开始可能只是系统出现了局部的、小规模的故障，然而由于种种原因，故障影响的范围越来越大，最终导致了全局性的后果。
- 适用场景：防止应用程序直接调用那些很可能会调用失败的远程服务或共享资源
- 服务降级:
          当服务器压力剧增的情况下，根据当前业务情况及流量对一些服务和页面有策略的降级，以此释放服务器资源以保证核心任务的正常运行。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727210207227.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

#### 发布测试
在发布版本的时候会采用：金丝雀发布和蓝绿发布。作为 API 网关可以使用路由选择和流量切换来协助上述行为。这里以金丝雀发布为例，看看 API 网关如何做路由转换的。

假设将 4 个服务从 V1 更新到 V2 版本，这 4 个服务的流量请求由 1 个 API 网关管理。
那么先将一台服务与 API 网关断开，部署 V2 版本的服务，然后 API 网关再将流量导入到 V2 版本的服务上。
这里流量的导入可以是逐步进行的，一旦 V2 版本的服务趋于稳定。再如法炮制，将其他服务替换成 V2 版本。

金丝雀发布一般先发 1 台，或者一个小比例，例如 2% 的服务器，主要做流量验证用，也称为金丝雀（Canary）测试（灰度测试）。

其来历是，旷工下矿洞前，先放一只金丝雀探查是否有毒气，金丝雀发布由此得名。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727210340198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

金丝雀测试需要完善的监控设施配合，通过监控指标反馈，观察金丝雀的健康状况，作为后续发布或回滚的依据。

如果金丝测试通过，则把剩余的 V1 版本全部升级为 V2 版本。如果金丝雀测试失败，则直接回退金丝雀，发布失败。

#### 缓存数据
我们可以在 API 网关缓存一些修改频率不高的数据。例如：用户信息，配置信息，通过服务定期刷新这个缓存就行了：

- 用户请求先访问 API 网关，如果发现有缓存信息，直接返回给用户。

- 如果没有发现缓存信息，回源到应用服务器获取信息。

- 另外，有一个缓存更新服务，定期把应用服务器中的信息更新到网关本地缓存中。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727210430262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)

#### 日志记录
通过 API 网关上的过滤器我们可以加入日志服务，记录请求和返回信息。同时可以建立一个管理员的界面去监控这些数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200727210506114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODE3MjIyOQ==,size_16,color_FFFFFF,t_70)
