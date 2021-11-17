
- [Network Namespace](https://mp.weixin.qq.com/s/Vf_Pj5ofj0am6SRPtMn6GA)

  包含网卡（Network Interface），回环设备（Lookback Device），路由表（Routing Table）和iptables规则
    - Veth Pair：Veth设备对的引入是为了实现在不同网络命名空间的通信，总是以两张虚拟网卡（veth peer）的形式成对出现的。并且，从其中一端发出的数据，总是能在另外一端收到
    - Iptables/Netfilter：
        - Netfilter负责在内核中执行各种挂接的规则（过滤、修改、丢弃等），运行在内核中
        - Iptables模式是在用户模式下运行的进程，负责协助维护内核中Netfilter的各种规则表
    - 网桥：网桥是一个二层网络虚拟设备，类似交换机，主要功能是通过学习而来的Mac地址将数据帧转发到网桥的不同端口上
    - 路由: Linux系统包含一个完整的路由功能，当IP层在处理数据发送或转发的时候，会使用路由表来决定发往哪里

  - 同宿主机的容器时间如何通信呢？

    我们可以简单把他们理解成两台主机，主机之间通过网线连接起来，如果要多台主机通信，我们通过交换机就可以实现彼此互通，在Linux中，我们可以通过网桥来转发数据。

    在容器中，以上的实现是通过docker0网桥，凡是连接到docker0的容器，就可以通过它来进行通信。要想容器能够连接到docker0网桥，我们也需要类似网线的虚拟设备Veth Pair来把容器连接到网桥上。

    ![img.png](docker_network_container.png)

    默认情况下，通过network namespace限制的容器进程，本质上是通过Veth peer设备和宿主机网桥的方式，实现了不同network namespace的数据交换。

    与之类似地，当你在一台宿主机上，访问该宿主机上的容器的IP地址时，这个请求的数据包，也是先根据路由规则到达docker0网桥，然后被转发到对应的Veth Pair设备，最后出现在容器里。

  -跨主机网络通信

    CNI即容器网络的API接口

    CNI维护了一个单独的网桥来代替 docker0。这个网桥的名字就叫作：CNI 网桥，它在宿主机上的设备名称默认是：cni0。cni的设计思想，就是：Kubernetes在启动Infra容器之后，就可以直接调用CNI网络插件，为这个Infra容器的Network Namespace，配置符合预期的网络栈

    CNI插件三种网络实现模式
    - Overlay模式是基于隧道技术实现的，整个容器网络和主机网络独立，容器之间跨主机通信时将整个容器网络封装到底层网络中，然后到达目标机器后再解封装传递到目标容器。不依赖与底层网络的实现。实现的插件有Flannel（UDP、vxlan）、Calico（IPIP）等等
    - 三层路由模式中容器和主机也属于不通的网段，他们容器互通主要是基于路由表打通，无需在主机之间建立隧道封包。但是限制条件必须依赖大二层同个局域网内。实现的插件有Flannel（host-gw）、Calico（BGP）等等
    - Underlay网络是底层网络，负责互联互通。容器网络和主机网络依然分属不同的网段，但是彼此处于同一层网络，处于相同的地位。整个网络三层互通，没有大二层的限制，但是需要强依赖底层网络的实现支持.实现的插件有Calico（BGP）等等
    ![img.png](docker_network_cni.png)

- K8S暴露服务的方式有哪些吗?

  ClusterIP、NodePort、Ingress将流量路由到集群中的服务。
  - ClusterIP更多是为集群内服务的通信而设计
  - 某些向集群外部暴露的TCP和UDP服务适合使用NodePort。
  - 而如果向外暴露的是HTTP服务，且需要提供域名和URL路径路由能力时则需要在Service上面再加一层Ingress做反向代理才行。
  
- GOMAXPROCS
  - CPU Affinity

    CPU Affinity 是一种调度属性，它可以将单个进程绑定到一个或一组 CPU 上。

    在 SMP（Symmetric Multi-Processing 对称多处理）架构下，Linux 调度器（Scheduler）会根据 CPU affinity 的设置让指定的进程运行在绑定的 CPU 上，而不会在别的 CPU 上运行。 CPU Affinity 就是进程要在某个给定的 CPU 上尽量长时间地运行而不被迁移到其他处理器的倾向性。Linux 内核进程调度器天生就具有被称为软 CPU Affinity 的特性，这意味着进程通常不会在处理器之间频繁迁移。合理的设置 CPU Affinity（进程独占 CPU Core）可以提高程序处理性能。
  - GOMAXPROCS

    Golang 的 Runtime 包中获取和设置 GOMAXPROCS，也就是 Go Scheduler 确定 P 数量的逻辑。在 Linux 上，它会利用系统调用 sched_getaffinity 来获得系统的 CPU 核数。

    可以通过 runtime.GOMAXPROCS() 来设定 P 的值，当前 Go 版本的 GOMAXPROCS 默认值已经设置为 CPU 的（逻辑核）核数， 这允许我们的 Go 程序充分使用机器的每一个 CPU, 最大程度的提高我们程序的并发性能。不过从实践经验中来看，IO 密集型的应用，可以稍微调高 P 的个数；而本文讨论的 Affinity 设置更适合 CPU 密集型的应用。
  - Docker CPU 调度
    - 默认容器会使用宿主机 CPU 是不受限制的
    - 要限制容器使用 CPU，可以通过参数设置 CPU 的使用，又细分为两种策略：
      - 将容器设置为普通进程，通过完全公平调度算法（CFS，Completely Fair Scheduler）调度类实现对容器 CPU 的限制 – 默认方案
      - 将容器设置为实时进程，通过实时调度类进行限制
    docker（docker run）配置 CPU 使用量的参数主要下面几个，这些参数主要是通过配置在容器对应 cgroup 中，由 cgroup 进行实际的 CPU 管控。其对应的路径可以从 cgroup 中查看到
     ```shell
       --cpu-shares                    CPU shares (relative weight)
       --cpu-period                    Limit CPU CFS (Completely Fair Scheduler) period
       --cpu-quota                     Limit CPU CFS (Completely Fair Scheduler) quota
       --cpuset-cpus                   CPUs in which to allow execution (0-3, 0,1)
     ```
  - K8S里的CPU调度

    kubernetes 对容器可以设置两个关于 CPU 的值：limits 和 requests，即 spec.containers[].resources.limits.cpu 和 spec.containers[].resources.requests.cpu
    - limits：该（单）pod 使用的最大的 CPU 核数 limits=cfs_quota_us/cfs_period_us 的值。比如 limits.cpu=3（核），则 cfs_quota_us=300000，cfs_period_us 值一般都使用默认的 100000
    - requests：该（单）pod 使用的最小的 CPU 核数，为 pod 调度提供计算依据
      - 一方面则体现在容器设置 --cpu-shares 上，比如 requests.cpu=3，–cpu-shares=1024，则 cpushare=1024*3=3072。
      - 另一方面，比较重要的一点，用来计算 Node 的 CPU 的已经分配的量就是通过计算所有容器的 requests 的和得到的，那么该 Node 还可以分配的量就是该 Node 的 CPU 核数减去前面这个值。当创建一个 Pod 时，Kubernetes 调度程序将为 Pod 选择一个 Node。每个 Node 具有每种资源类型的最大容量：可为 Pods 提供的 CPU 和内存量。调度程序确保对于每种资源类型，调度的容器的资源请求的总和小于 Node 的容量。尽管 Node 上的实际内存或 CPU 资源使用量非常低，但如果容量检查失败，则调度程序仍然拒绝在节点上放置 Pod。

  - 在 Docker-container 和 Kubernetes 集群中，存在 GOMAXPROCS 会错误识别容器 cpu 核心数的问题

    Uber 的这个库 automaxprocs，大致原理是读取 CGroup 值识别容器的 CPU quota，计算得到实际核心数，并自动设置 GOMAXPROCS 线程数量
 
- [为什么容器内存占用居高不下，频频 OOM](https://eddycjy.com/posts/why-container-memory-exceed/)
  
  排查方向
  - 频繁申请重复对象
    - 怀疑是否在量大时频繁申请重复对象，而 Go 本身又没有及时释放内存，因此导致持续占用。
    - 想解决 “频繁申请重复对象”，我们大多会采用多级内存池的方式，也可以用最常见的 sync.Pool
      - 形成 “并发⼤－占⽤内存⼤－GC 缓慢－处理并发能⼒降低－并发更⼤”这样的恶性循环
    - 通过拉取 PProf goroutine，可得知 Goroutine 数并不高
  - 不知名内存泄露
    - 可以借助 PProf heap（可以使用 base -diff）
    - 接下通过命令也可确定 Go 进程的 RSS 并不高。 [但 VSZ 却相对 “高” 的惊人](https://eddycjy.com/posts/go/talk/2019-09-24-why-vsz-large/)
    - 从结论上来讲，也不像 Go 进程内存泄露的问题，因此也将其排除
  - madvise 策略变更
    - 在 Go1.12 以前，Go Runtime 在 Linux 上使用的是 MADV_DONTNEED 策略，可以让 RSS 下降的比较快，就是效率差点。
    - 在 Go1.12 及以后，Go Runtime 专门针对其进行了优化，使用了更为高效的 MADV_FREE 策略。但这样子所带来的副作用就是 RSS 不会立刻下降，要等到系统有内存压力了才会释放占用，RSS 才会下降。
    - MADV_FREE 的策略改变，[需要 Linux 内核在 4.5 及以上](https://github.com/golang/go/issues/23687). go的老版本需要 run binary with GODEBUG=madvdontneed=1 就可以归还给系统了，或者直接升级到go 1.16
  - 监控/判别条件有问题
    - OOM 的判断标准是 container_memory_working_set_bytes 指标
  - 容器环境的机制
    - container_memory_working_set_bytes 是由 cadvisor 提供的，对应下述指标 `kc top`
    - Memory 换算过来是 4GB+, 显然和 RSS 不对标
  
  原因
  - 从 [cadvisor/issues/638](https://github.com/google/cadvisor/issues/638) 可得知 container_memory_working_set_bytes 指标的组成实际上是 RSS + Cache。而 Cache 高的情况，常见于进程有大量文件 IO，占用 Cache 可能就会比较高
  - 只要是涉及有大量文件 IO 的服务，基本上是这个问题的老常客了
  
  解决
  - cadvisor 所提供的判别标准 container_memory_working_set_bytes 是不可变更的，也就是无法把判别标准改为 RSS
  - 使用类 sync.Pool 做多级内存池管理，防止申请到 “不合适”的内存空间，常见的例子： ioutil.ReadAll：
  - 核心是做好做多级内存池管理，因为使用多级内存池，就会预先定义多个 Pool，比如大小 100，200，300的 Pool 池，当你要 150 的时候，分配200，就可以避免部分的内存碎片和内存碎块

   

