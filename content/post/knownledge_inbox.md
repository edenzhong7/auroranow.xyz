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
1.  TCP/IP 协议，HTTP协议
    - 三次握手，四次挥手，TIME_WAIT 的作用，
    - 拥塞控制，窗口滑动，
    - TCP和UDP差别，应用场景，常见上层协议
    -  HTTP 协议的返回码、HTTP 的方法，报文解析，
    -  tls加密，
    -  HTTP三大版本的改进对比
2. socket
   - 阻塞io
   - 非阻塞io
   - io多路复用: select/poll/epoll 优缺点及使用场景
   - 如何设置socket参数实现透明代理
   - graceful重启web服务器
   - 常用socket opt

### 数据结构
- 数据库相关数据结构
- hashmap, slice, routine
- 堆，AVL树，B树，B+树，二叉查找树，字典树，跳跃表，图，前缀树后缀树

### 操作系统
- 进程管理，进程线程协程，进程间通讯
- 死锁产生、破坏、预防
- 内存管理，着重虚拟内存

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
- k8s 常见资源概念，service、informer的实现以及作用，约定资源版本号作用，多个版本号并存如何维护
- k8s有什么好处，踩过什么坑
- k8s权限控制
- k8s 开发经验，operator，injector
- 怎么扩展 kubernetes scheduler, 让它能 handle 大规模的节点调度
- helm 使用

## 监控系统

- prom和grafana使用开发经验，监控系统怎么自监控
- promql function实现，指标定义和用法 
- 时序性数据库的存储结构

## Go语言
### plugin和monkey hot fix
### 协程实现及调度
### gc发展，三色标记法，内存分配及内存顺序保证，逃逸分析
### 网络 非阻塞io
### chan, sync, go并发实践，常见并发错误
### 基本数据结构实现: map, slice, array, nil，interface
### cgo，unsafe
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

### 反射使用场景
### 编码规范，TDD
### go工具链使用

## 数据库
### mysql
- 存储引擎用的是什么?（InnoDB）为什么选 InnoDB?
- 聚簇索引和非聚簇索引有什么区别
- B+树和二叉树有什么区别和优劣?
- 索引及查询优化

### redis
- 分布式锁
- zset实现
- 常用场景
- 高可用

## 微服务
1. 服务发现，负载均衡，熔断
2. 全局id生成器
3. 高可用
4. raft/paxos共识算法
5. 对Service Mesh和Serverless的理解

## Istio
### 设计理念及架构
### 功能和优缺点分析
### 使用经验

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