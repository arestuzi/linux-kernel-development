# Linux内核开发

## 内容简介

### Linux内核介绍
+ [Unix历史](introduction-to-the-linux-kernel/history-of-unix.md)
+ [Linus带来了Linux](introduction-to-the-linux-kernel/introduction-to-linux.md)
+ [操作系统和内核概览](introduction-to-the-linux-kernel/overview-of-operating-system.md)
+ [Linux VS 经典 Unix内核](introduction-to-the-linux-kernel/linux-vs-unix.md)
+ Linux内核版本
+ Linux内核开发社区介绍
+ 我们开始旅程之前

### Linux内核入门
+ 获取内核源码
  + 使用git
  + 安装内核源码
  + 为内核打补丁
+ 内核源码树
+ 编译内核
  + 设置内核
  + 最小化编译杂音？
  + 并发执行编译任务
  + 安装新内核
+ 不同天性的野兽
  + 没有libc和标准头文件
  + GNC C
    + 内联函数
    + 内联汇编
    + 分支注释？
  + 无内存保护
  + 无法(不容易)使用浮点
  + 规格比较小的且固定大小的栈
  + 同步和并发
  + 可移植性的重要性
  + 总结 

### 进程管理
+ 进程
+ 进程描述符和进程的结构
  + 分配进程描述符
  + 存储进程描述符
  + 进程状态
  + 控制当前进程状态
  + 进程上下文
  + 进程树
+ 进程创建
  + Copy-On-Write
  + Forking
  + vfork()
+ Linux如何实现线程调度
  + 创建一个线程
  + 内核线程
+ 进程终止
  + 删除一个进程描述符
  + 孤儿进程的窘境
  + 总结

### 进程调度
+ 多任务
+ Linux进程调度
+ 策略
  + I/O强相关 VS 处理器强相关进程
  + 进程优先级
  + 时间片
  + 实际应用中的调度策略
+ Linux调度算法
  + 调度器类型
  + Unix系统中进程调度
  + 公平调度算法
+ Linux调度算法的实现
  + 统计时间
    + 调度器实体结构
    + 虚拟进行时
  + 进程选择
    + 获取下一个进程
    + 添加进程到进程树
    + 进程树上删除进程
  + 调度器的entry point
  + 睡眠和唤醒
    + 等待队列
    + 唤醒
  + 抢占和上下文切换
    + 用户态抢占
    + 内核态抢占
  + 实时调度策略
  + 与调度算法有关的系统调用
    + 调度策略和优先级相关的系统调用
    + 处理器亲和度相关系统调用
    + Yielding 处理器时间？
  + 总结

### 系统调用
+ 与内核取得联系
+ API、POSIX、和C 库
+ 系统调用
  + 系统调用号码
  + 系统调用性能？
+ 系统调用处理程序
  + 展现正确的系统调用
  + 参数传递
+ 系统调用的实现
  + 如何实现系统调用
  + 参数校对
+ 系统调用上下文
  + 绑定系统调用最后一步
  + 从用户空间访问系统调用
  + 为什么不直接执行系统调用？
+ 总结

### 内核数据结构
+ 链表
  + 单项链表和双向链表
  + 环形链表
  + 链表中传递数据？
  + Linux内核的实现
    + 链表的数据结构
    + 如何定义一个链表
    + 链表头
  + 操作链表
    + 在链表中增加一个节点
    + 在链表中删除一个节点
    + 移动、连接链表中的节点
  + 遍历链表
    + 基本方法
    + 可用方法？
    + 倒序遍历链表？
    + 
1. 中断和中断处理程序
2. 下半部和延迟处理
3.  内核同步机制介绍
4.  内核同步方法
5.  计时器和时间管理
6.  内存管理
7.  虚拟文件系统
8.  Block I/O 层
9.  进程地址空间
10. 页缓存和页写回
11. 设备和内核模块
12. 排错
13. 可移植性
14. 补丁、入侵和社区