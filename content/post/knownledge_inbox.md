---
title: "知识点暂存区"
date: 2020-05-10T10:00:09+08:00
lastmod: 2020-05-10T10:00:09+08:00
draft: true
author: ""
authorLink: ""
description: ""
license: ""

tags: []
categories: []
hiddenFromHomePage: false

featuredImage: ""
featuredImagePreview: ""

toc: false
autoCollapseToc: true
math: false
lightgallery: true
linkToMarkdown: true
share:
  enable: true
comment: true
---

<!--more-->
## 基础
### 网络
### 网络/协议 
#### TCP/IP 协议
引用: 
- https://www.cnblogs.com/lipengfei159263/p/9745986.html
- https://blog.csdn.net/Wu000999/article/details/89293717

1. 三次握手
最开始的时候客户端和服务器都是处于CLOSED状态。主动打开连接的为客户端，被动打开连接的是服务器。
- TCP服务器进程先创建传输控制块TCB，时刻准备接受客户进程的连接请求，此时服务器就进入了LISTEN（监听）状态；
- TCP客户进程也是先创建传输控制块TCB，然后向服务器发出连接请求报文，这是报文首部中的同部位SYN=1，同时选择一个初始序列号 seq=x ，此时，TCP客户端进程进入了 SYN-SENT（同步已发送状态）状态。TCP规定，SYN报文段（SYN=1的报文段）不能携带数据，但需要消耗掉一个序号。
- TCP服务器收到请求报文后，如果同意连接，则发出确认报文。确认报文中应该 ACK=1，SYN=1，确认号是ack=x+1，同时也要为自己初始化一个序列号 seq=y，此时，TCP服务器进程进入了SYN-RCVD（同步收到）状态。这个报文也不能携带数据，但是同样要消耗一个序号。
- TCP客户进程收到确认后，还要向服务器给出确认。确认报文的ACK=1，ack=y+1，自己的序列号seq=x+1，此时，TCP连接建立，客户端进入ESTABLISHED（已建立连接）状态。TCP规定，ACK报文段可以携带数据，但是- 如果不携带数据则不消耗序号。
- 当服务器收到客户端的确认后也进入ESTABLISHED状态，此后双方就可以开始通信了。

最后一次客户端的确认主要防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误。

在三次握手过程中，Server发送SYN-ACK之后，收到Client的ACK之前的TCP连接称为半连接（half-open connect），此时Server处于SYN_RCVD状态，当收到ACK后，Server转入ESTABLISHED状态。SYN攻击就是Client在短时间内伪造大量不存在的IP地址，并向Server不断地发送SYN包，Server回复确认包，并等待Client的确认，由于源地址是不存在的，因此，Server需要不断重发直至超时，这些伪造的SYN包将产时间占用未连接队列，导致正常的SYN请求因为队列满而被丢弃，从而引起网络堵塞甚至系统瘫痪。SYN攻击时一种典型的DDOS攻击，检测SYN攻击的方式非常简单，即当Server上有大量半连接状态且源IP地址是随机的，则可以断定遭到SYN攻击了，使用如下命令可以让之现行：`#netstat -nap | grep SYN_RECV`

2. 四次挥手
数据传输完毕后，双方都可释放连接。最开始的时候，客户端和服务器都是处于ESTABLISHED状态，然后客户端主动关闭，服务器被动关闭。

- 客户端进程发出连接释放报文，并且停止发送数据。释放数据报文首部，FIN=1，其序列号为seq=u（等于前面已经传送过来的数据的最后一个字节的序号加1），此时，客户端进入FIN-WAIT-1（终止等待1）状态。 TCP规定，FIN报文段即使不携带数据，也要消耗一个序号。
- 服务器收到连接释放报文，发出确认报文，ACK=1，ack=u+1，并且带上自己的序列号seq=v，此时，服务端就进入了CLOSE-WAIT（关闭等待）状态。TCP服务器通知高层的应用进程，客户端向服务器的方向就释放了，这时候处于半关闭状态，即客户端已经没有数据要发送了，但是服务器若发送数据，客户端依然要接受。这个状态还要持续一段时间，也就是整个CLOSE-WAIT状态持续的时间。
- 客户端收到服务器的确认请求后，此时，客户端就进入FIN-WAIT-2（终止等待2）状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最后的数据）。
- 服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，FIN=1，ack=u+1，由于在半关闭状态，服务器很可能又发送了一些数据，假定此时的序列号为seq=w，此时，服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。
- 客户端收到服务器的连接释放报文后，必须发出确认，ACK=1，ack=w+1，而自己的序列号是seq=u+1，此时，客户端就进入了TIME-WAIT（时间等待）状态。注意此时TCP连接还没有释放，必须经过2∗∗MSL（最长报文段寿命）的时间后，当客户端撤销相应的TCB后，才进入CLOSED状态。 
- 服务器只要收到了客户端发出的确认，立即进入CLOSED状态。同样，撤销TCB后，就结束了这次的TCP连接。可以看到，服务器结束TCP连接的时间要比客户端早一些。

3. 为什么客户端最后还要等待2MSL？
MSL（Maximum Segment Lifetime），TCP允许不同的实现可以设置不同的MSL值。

第一，保证客户端发送的最后一个ACK报文能够到达服务器，因为这个ACK报文可能丢失，站在服务器的角度看来，我已经发送了FIN+ACK报文请求断开了，客户端还没有给我回应，应该是我发送的请求断开报文它没有收到，于是服务器又会重新发送一次，而客户端就能在这个2MSL时间段内收到这个重传的报文，接着给出回应报文，并且会重启2MSL计时器。

第二，防止类似与“三次握手”中提到了的“已经失效的连接请求报文段”出现在本连接中。客户端发送完最后一个确认报文后，在这个2MSL时间中，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样新的连接中不会出现旧连接的请求报文。

为什么建立连接是三次握手，关闭连接确是四次挥手呢？

建立连接的时候， 服务器在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。
而关闭连接时，服务器收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，而自己也未必全部数据都发送给对方了，所以己方可以立即关闭，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送，从而导致多了一次。

4. TIME_WAIT 的作用
   主动关闭的Socket端ack对方的FIN请求后会进入TIME_WAIT状态，并且持续2MSL时间长度，默认30s, 确保最后一个确认报文能够到达。如果没能到达，服务端就会会重发FIN请求释放连接。等待一段时间没有收到重发就说明服务的已经CLOSE了。如果有重发，则客户端再发送一次LAST ack信号，等待一段时间是为了让本连接持续时间内所产生的所有报文都从网络中消失，使得下一个新的连接不会出现旧的连接请求报文。
5. 拥塞控制，窗口滑动
   窗口滑动
   窗口是缓存的一部分，用来暂时存放字节流。发送方和接收方各有一个窗口，接收方通过 TCP 报文段中的窗口字段 告诉发送方自己的窗口大小，发送方根据这个值和其它信息设置自己的窗口大小。发送窗口内的字节都允许被发送，接收窗口内的字节都允许被接收。如果发送窗口左部的字节已经发送并且收到了确 认，那么就将发送窗口向右滑动一定距离，直到左部第一个字节不是已发送并且已确认的状态;接收窗口的滑动类 似，接收窗口左部字节已经发送确认并交付主机，就向右滑动接收窗口。接收窗口只会对窗口内最后一个按序到达的字节进行确认，例如接收窗口已经收到的字节为 {31, 34, 35}，其中 {31} 按序到达，而 {34, 35} 就不是，因此只对字节 31 进行确认。发送方得到一个字节的确认之后，就知道这个字节之前 的所有字节都已经被接收。接收方发送的确认报文中的窗口字段可以用来控制发送方窗口大小，从而影响发送方的发送速率。将窗口字段设置为 0，则发送方不能发送数据。
   
   拥塞控制
   如果网络出现拥塞，分组将会丢失，此时发送方会继续重传，从而导致网络拥塞程度更高。因此当出现拥塞时，应当 控制发送方的速率。这一点和流量控制很像，但是出发点不同。流量控制是为了让接收方能来得及接收，而拥塞控制 是为了降低整个网络的拥塞程度。

   发送的最初执行慢开始，令 cwnd = 1，发送方只能发送 1 个报文段;当收到确认后，将 cwnd 加倍，因此之后发送方能够发送的报文段数量为:2、4、8 ...。注意到慢开始每个轮次都将 cwnd 加倍，这样会让 cwnd 增长速度非常快，从而使得发送方发送的速度增长速度过 快，网络拥塞的可能性也就更高。设置一个慢开始门限 ssthresh，当 cwnd >= ssthresh 时，进入拥塞避免，每个轮 次只将 cwnd 加 1。如果出现了超时，则令 ssthresh = cwnd / 2，然后重新执行慢开始。
    
   在发送方，如果收到三个重复确认，那么可以知道下一个报文段丢失，此时执行快重传，立即重传下一个报文段。例如收到三个 M2，则 M3 丢失，立即重传 M3。 在这种情况下，只是丢失个别报文段，而不是网络拥塞。因此执行快恢复，令 ssthresh = cwnd / 2 ，cwnd =ssthresh，注意到此时直接进入拥塞避免。
   
   慢开始和快恢复的快慢指的是 cwnd 的设定值，而不是 cwnd 的增长速率。慢开始 cwnd 设定为 1，而快恢复 cwnd
设定为 ssthresh。

6. TCP和UDP差别，应用场景，常见上层协议
用户数据报协议 UDP(User Datagram Protocol)是无连接的，尽最大可能交付，没有拥塞控制，面向报文 (对于应用程序传下来的报文不合并也不拆分，只是添加 UDP 首部)，支持一对一、一对多、多对一和多对多 的交互通信。8字节
传输控制协议 TCP(Transmission Control Protocol)是面向连接的，提供可靠交付，有流量控制，拥塞控 制，提供全双工通信，面向字节流(把应用层传下来的报文看成字节流，把字节流组织成大小不等的数据 块)，每一条 TCP 连接只能是点对点的(一对一)。20字节

// TODO 应用场景

常见上层协议
基于TCP

| 协议|	全称	|默认端口|
|----|----|---|
| HTTP	 | HyperText Transfer Protocol（超文本传输协议）	|80|
| FTP	 | File Transfer Protocol (文件传输协议) |	20用于传输数据，21用于传输控制信息|
| SMTP	 | Simple Mail Transfer Protocol (简单邮件传输协议)	|25|
| SSH	 | Secure Shell	|22|
| TELNET | 	Teletype over the Network (网络电传)	|23|

基于UDP

| 协议 |	全称	|端口|
|---|---|---|
| DNS | Domain Name Service (域名服务) |	53|
| TFT | Trivial File Transfer Protocol (简单文件传输协议)|	69|
| SNM | Simple Network Management Protocol (简单网络管理协议)	|通过UDP端口161接收，只有Trap信息采用UDP端口162。|
| NTP | Network Time Protocol (网络时间协议)	|123|


### HTTP协议
1. HTTP 协议的返回码、HTTP 的方法，报文解析，
// TODO CS-Notes
2. tls加密
// TODO http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html
3.  HTTP三大版本的改进对比
// TODO https://blog.fundebug.com/2019/03/07/understand-http2-and-http3/ https://juejin.im/post/5dc37277f265da4d57771f87
### socket
1. 阻塞io
2. 非阻塞io
3. io多路复用: select/poll/epoll 优缺点及使用场景
// TODO 以上CS-Notes
4. 如何设置socket参数实现透明代理
// TODO https://www.codenong.com/js5393fb5e2c87/ xmesh
5. graceful重启web服务器
// TODO https://gravitational.com/blog/golang-ssh-bastion-graceful-restarts/
6. 常用socket opt
// TODO https://notes.shichao.io/unp/ch7/

### 数据结构
- 数据库相关数据结构
// TODO mysql https://juejin.im/post/5cb7247df265da03af27ccf9
// TODO redis https://juejin.im/post/5d71d3bee51d453b5f1a04f1 Redis深度历险
- hashmap, slice, string...
// TODO https://draveness.me/golang/docs/part2-foundation/ch03-datastructure/golang-array/ https://gfw.go101.org/article/container.html
- 堆，AVL树，二叉查找树，字典树，图，前缀树后缀树
// TODO https://blog.fundebug.com/2018/08/27/code-interview-data-structure/

### 操作系统
- 进程管理，进程线程协程，进程间通讯
- 死锁产生、破坏、预防
- 内存管理，着重虚拟内存
// TODO CS-Notes

### 算法
x => leetcode

y => 剑指Offer

z => 程序员代码面试指南

### 动态规划/贪心
### 数组
### 堆栈
### 位运算
### 树
### BFS/DFS
### 并查集
### 递归、二分、回溯
### 排序
### 图
- A star
- 最短路径
- bfs，dfs

### 字符串
### 数学、几何、概率

## k8s
- kubernetes源码分析: kubelet、apiserver、scheduler、controller-manager、adimissionWebHook
- k8s常见流程: kebectl exec、pod创建、statefulset滚动升级
  // TODO
  - https://blog.fleeto.us/post/how-kubectl-exec-works/
  - http://www.xuyasong.com/?p=1908
  - https://aleiwu.com/post/kubectl-debug-intro/
  - https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
  - https://www.jianshu.com/p/5e0c9d1dbe95
  - https://draveness.me/kubernetes-pod/
- k8s 常见资源概念，service、informer的实现以及作用，约定资源版本号作用，多个版本号并存如何维护
  // TODO
  - https://zhuanlan.zhihu.com/p/111244353
- k8s有什么好处，踩过什么坑
- k8s权限控制
  // TODO
  - https://zhuanlan.zhihu.com/p/94361682
  - https://juejin.im/post/5d60fe39f265da03f564ee68
- k8s 开发经验，operator，injector
- 怎么扩展 kubernetes scheduler, 让它能 handle 大规模的节点调度
- helm 使用
  // TODO
  - https://blog.csdn.net/weixin_36938307/article/details/105226395
  - https://helm.sh/docs/chart_template_guide/

## 监控系统
- prom和grafana使用开发经验，监控系统怎么自监控
  // TODO
  - https://www.cyningsun.com/09-13-2019/micro-service-monitor-prometheus-ha.html
  - https://www.cyningsun.com/02-22-2020/hidden-secret-to-understanding-prometheus.html
  - https://www.aneasystone.com/archives/2018/11/prometheus-in-action.html
- promql function实现，指标定义和用法
  // TODO
  - https://prometheus.io/docs/prometheus/latest/querying/functions/
  - https://prometheus.io/docs/concepts/metric_types/
  - https://juejin.im/entry/5dd1e45c6fb9a0200e1a9abf
  - https://www.qikqiak.com/post/grafana-usage-in-k8s/
- 时序性数据库的存储结构
  // TODO
  - https://www.cnblogs.com/jimbo17/p/8659119.html
  - https://www.jianshu.com/p/096e59cddb24


## Go语言
### plugin和monkey hot fix
// TODO 
- https://draveness.me/golang/docs/part4-advanced/ch08-metaprogramming/golang-plugin/  
- https://bou.ke/blog/monkey-patching-in-go/
### 协程实现及调度
// TODO
- https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/
- https://www.w3cschool.cn/go_internals/go_internals-65bd282x.html 
- https://taohuawu.club/high-performance-implementation-of-goroutine-pool
### gc发展，三色标记法，内存分配及内存顺序保证，逃逸分析
// TODO
  - https://www.jianshu.com/p/bfc3c65c05d1
  - https://www.infoq.cn/article/development-history-and-current-situation-of-gc-algorithm
  - https://www.bookstack.cn/read/For-learning-Go-Tutorial/src-spec-02.0.md
  - https://draveness.me/golang/docs/part3-runtime/ch07-memory/golang-memory-allocator/
  - https://www.w3cschool.cn/go_internals/go_internals-kho3283t.html
  - https://draveness.me/golang/docs/part3-runtime/ch07-memory/golang-stack-management/
  - https://gfw.go101.org/article/memory-model.html
### 网络 非阻塞io
// TODO 
- https://taohuawu.club/go-netpoll-io-multiplexing-reactor
- https://www.w3cschool.cn/go_internals/go_internals-wlhn283l.html
### chan, sync, context, go并发实践，常见并发错误
// TODO 
- Go101
- https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-context/
- https://www.w3cschool.cn/go_internals/go_internals-dea92830.html
### cgo，unsafe
// TODO
- https://www.w3cschool.cn/go_internals/go_internals-419t283o.html
- https://gfw.go101.org/article/unsafe.html
- https://chai2010.cn/advanced-go-programming-book/ch2-cgo/readme.html
#### unsafe
- string <-> []byte
```
// string被标记为不可用的，所以[]byte(string)会对底层数组进行拷贝，使用unsafe转换的[]byte需要保证不会修改内容
func StringToBytes(s string) (b []byte) {
    stringHeader := (*reflect.StringHeader)(unsafe.Pointer(&s))
    sliceHeader := (*reflect.SliceHeader)(unsafe.Pointer(&bytes))
    sliceHeader.Data = stringHeader.Data
    sliceHeader.Len = stringHeader.Len
    sliceHeader.Cap = stringHeader.Len
    return b
}
```

### reflect
// TODO 
- https://draveness.me/golang/docs/part2-foundation/ch04-basic/golang-reflect/
- https://gfw.go101.org/article/reflection.html

### 编码规范，TDD
// TODO
- https://github.com/xxjwxc/uber_go_guide_cn
- tencent编码规范

### go工具链使用
- pprof
// TODO
  - https://segmentfault.com/a/1190000016412013
  - https://blog.golang.org/pprof
- trace
  // TODO
  - https://medium.com/a-journey-with-go/go-discovery-of-the-trace-package-e5a821743c3c
  - https://making.pusher.com/go-tool-trace/
- test
// TODO
  - https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter09/09.1.html
  - http://frobisher.me/2017/06/26/golang-about-tests-toolchains/
  - https://www.flysnow.org/2017/05/16/go-in-action-go-unit-test.html
- build参数、tag
// TODO https://golang.org/cmd/go/
- dlv调试
// TODO
  - https://hustcat.github.io/getting-started-with-delve/
  - https://wiki.crdb.io/wiki/spaces/CRDB/pages/82411849/Debugging+with+Delve

## 数据库
### mysql
- 存储引擎用的是什么?（InnoDB）为什么选 InnoDB?
  // TODO
  - https://zhuanlan.zhihu.com/p/50564425
  - https://juejin.im/post/5da33cd2518825338b22a780
  - https://www.sunjs.com/article/detail/366663d089e74167bd447593891096c4.html
- 聚簇索引和非聚簇索引有什么区别
// TODO https://juejin.im/post/5cdd701ee51d453a36384939
- B+树和二叉树有什么区别和优劣?
// TODO
  - https://my.oschina.net/lienson/blog/2987474
  - https://www.cnblogs.com/qlqwjy/p/7770580.html
- 索引及查询优化
  // TODO
  - https://juejin.im/post/5cb1dec9f265da0382610968
  - https://juejin.im/post/5c2c8dace51d455d382ee046
  - https://tech.meituan.com/2014/06/30/mysql-index.html

### redis
- 分布式锁
// TODO https://chai2010.cn/advanced-go-programming-book/ch6-cloud/ch6-02-lock.html Redis深度历险
- 延时任务系统
// TODO
  - https://chai2010.cn/advanced-go-programming-book/ch6-cloud/ch6-03-delay-job.html
  - https://www.chainnews.com/articles/332847148440.htm
  - https://juejin.im/post/5caf45b96fb9a0688b573d6c
- zset,ziplist实现
  // TODO  Redis深度历险
  - https://zsr.github.io/2017/07/03/redis-zset%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0/
  - https://origin.redisbook.com/compress-datastruct/ziplist.html
- 常用场景
// TODO
  - https://zhuanlan.zhihu.com/p/29665317
  - https://zhuanlan.zhihu.com/p/111985190
  - Redis深度历险
- 高可用
// TODO
  - https://yq.aliyun.com/articles/626532
  - https://www.jianshu.com/p/5de2ab291696
  - Redis深度历险

## 微服务
1. 服务发现，负载均衡，熔断
// TODO
- https://juejin.im/post/5d746acb51882579df5800f1
- https://www.infoq.cn/article/background-architecture-and-solutions-of-service-discovery
- https://www.jianshu.com/p/1bf9a46efe7a
- https://juejin.im/post/5b39eea0e51d4558c1010e36
- https://www.jianshu.com/p/8f7242cbf469
- https://www.cnblogs.com/xybaby/p/7867735.html
- https://juejin.im/post/5cced96e6fb9a032514bbf94
- https://zhuanlan.zhihu.com/p/61363959
2. 全局id生成器
// TODO
- http://www.52im.net/thread-1998-1-1.html
- https://chai2010.cn/advanced-go-programming-book/ch6-cloud/ch6-01-dist-id.html
3. raft/paxos共识算法
// TODO
- https://juejin.im/post/5ce7be0fe51d45775c73dc57
- https://www.infoq.cn/article/raft-paper
- http://blog.itpub.net/31556438/viewspace-2637112/
- https://owent.net/2016/1226.html
- https://zhuanlan.zhihu.com/p/31780743
- https://www.cnblogs.com/linbingdong/p/6253479.html
- https://juejin.im/post/5b2664bd51882574874d8a76
- https://ocavue.com/paxos.html
4. 对Service Mesh和Serverless的理解
// TODO
- https://www.infoq.cn/article/xVEoHtcORtxRSpCf2Ldd
- https://www.servicemesher.com/blog/why-is-service-mesh/
- https://zhuanlan.zhihu.com/p/61901608
- https://jimmysong.io/blog/what-is-a-service-mesh/
- https://jimmysong.io/kubernetes-handbook/usecases/understanding-serverless.html
- https://skyao.io/learning-serverless/introduction/scene.html


## Istio
读书笔记
### 设计理念及架构
// TODO 
- https://istio.io/zh/docs/ops/deployment/architecture/
- https://www.kubernetes.org.cn/7374.html
### 功能和优缺点分析
// TODO
- https://www.sofastack.tech/blog/service-mesh-development-trend-2/
- https://zhuanlan.zhihu.com/p/54123996
### 使用经验
// TODO
- https://juejin.im/post/5b7fc952f265da4358368734
- https://zhuanlan.zhihu.com/p/90720475
- https://www.servicemesher.com/blog/redefining-extensibility-in-proxies/
- https://www.servicemesher.com/blog/istio-1-5-explanation/

## 其他问题
- 堆和栈在内存中的区别是什么(数据结构方面以及实际实现方面）
- 最快的排序算法是哪个？给阿里2万多名员工按年龄排序应该选择哪个算法？堆和树的区别；写出快排代码；链表逆序代码；
- 求1000以内的水仙花数以及40亿以内的水仙花数；
- 子串包含问题(KMP 算法)写代码实现；
- 万亿级别的两个URL文件A和B，如何求出A和B的差集C,(Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)
- 蚁群算法与蒙特卡洛算法；
- 写出你所知道的排序算法及时空复杂度，稳定性；
- 百度POI中如何试下查找最近的商家功能(坐标镜像+R树)。
- 遍历二叉树
- 快速排序和冒泡的排序，怎么转换一下
  
## 项目
### xmesh
参考Envoy和阿里Mosn的设计，实现简单的网络代理，配合iptables对pod所有进出站tcp/udp流量进行代理
1. 数据库代理
  // TODO
  - https://docs.mongodb.com/manual/reference/mongodb-wire-protocol/#wire-msg-sections
  - https://dev.mysql.com/doc/dev/mysql-server/8.0.11/page_protocol_basic_packets.html#sect_protocol_basic_packets_packet
  - https://redis.io/topics/protocol
2. tcp/udp透明代理
3. iptables
4. filter设计，http2/quic/自定义协议检测，在代理层实现Metric和Trace上报，以及认证、限流等操作；

### 工作经验
微服务框架开发，参考gRPC 拦截器形式实现metrics统计和trace上报，Grafana / Prometheus / Helm / k8s的学习和使用: 
1. 基于grafana的auth proxy鉴权方式以及grafana代码中sqlmodel实现无侵入权限代理，将内部系统游戏项目映射到grafana org；
2. 使用Prometheus client sdk实现统一收集metrics上报，以及通过解析yaml定义规则将json统计日日志转换成prom metrics，支持物理机以Endpoint方式注册到k8s中，通过ServiceMonitor做监控目标的自动发现，使用k8s api实现Registry，通过操作ServiceMonitor/Service/Endpoint资源来实现自动发现物理机exporter，使用Thanos方案实现Prom高可用和数据持久化；
3. 实现sidecar injector用于自动注入框架依赖容器和其他配置，用户编写业务的helm chart无需额外编写框架配置，支持多模板选择，pod全部属性的patch；
4. 自定义k8s controller对k8s资源事件进行监听并通过定义规则进行上报
5. k8s调试器

### xrpc
- 修改proto-gen-go增加plugin generator兼容pb生成stub代码，基于go/ast实现从定义interface的go文件中生成stub代码，基于反射直接注册函数/结构体，与其他rpc不同的是，本框架不规定函数入参和返回值的个数和类型，有像本地函数般的体验；
- 支持基于tcp/kcp的自定义协议，也支持http2(抄gRPC)和quic；
- 参考rpcx项目的插件系统设计，各阶段设计单独的插件接口，提供基于consul的服务注册/发现、prometheus的监控上报以及Jaeger的链路追踪插件；
- 使用chord算法实现分布式kv提供服务发现

### 组件开发
- memory cache组件(参考BigCache实现)，go 1.5后针对map做了优化，对于key和value都不是指针的map不会进行GC扫描，设计成map[int]int, key是存储key的hash，value是在分片中的[]byte数组下标，并不直接对实际value进行存储而是追加序列化的[]byte到分片的全局[]byte中，依赖先进先出自动过期删除，删除接口只删除map中的下标记录，并不清理全局[]byte，回收代价太大；
- 海量消息ID生成，每条消息一个自增且唯一的消息ID分拆成两个关键属性——消息ID、消息序列号，前者需要唯一性不需要顺序性，使用UUID即可，后者需要顺序性不需要唯一性，每个用户保存一个单独的计数；
- yaml配置解析器，多叉树递归解析，实现直接以yaml路径(包含正则匹配)的形式获取yaml节点并提供接口转换成各种Go基本数据类型；
- 基于go/ast实现go文件所有interface的信息提取，可生成rpc stub代码，以及web框架路由注册，不需要反射； 
- 共享内存控制进程库，运行进程定时查询共享内存获取状态来触发重启停止重载配置等操作；
