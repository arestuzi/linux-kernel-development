# 简介
本章主要介绍Linux内核和Linux操作系统与Unix系统的历史羁绊。 当今， Linux操作系统家族和Unix 执行相似的应用程序接口（API），而且还共享一些程序设计。 到目前为止，Unix仍然是一个特别的操作系统，而且距今已经50多年了。 为了更加深入的了解Linux，我们必须先要了解Unix。

# Unix历史
经过50年生产业务的考验， 计算机科学家目前为止仍然认为Unix系统是迄今为止最为强大且简洁的操作系统之一。 Unix系统创建于1969年，它的创造者Dennis Ritchie和Ken Thompson已经成为传奇，然而Unix系统经受住了时间的考验，至今屹立不倒。

Unix的灵感来自于Multics， Multices是Bell实验室开发的一个失败的多用户操作系统。 当Multics项目终止时，Bell实验室计算机科学研究中心的成员发现他们还没有一个好用的可交互式操作系统。1969年夏天， Bell实验室程序员设计了文件系统，并且最终在Unix系统上进行了实现。 为了测试这个文件系统， Thompson在闲置的PDP-7设备上创建了一个新的系统。 到1971年，Unix系统被迁移至PDP-11，到了1973年，开发者用C语音重写了操作系统 --- 这个在当时史无前例的决定为将来的系统迁移铺平了道路。 第一个出了Bell实验室并且被大规模使用的Unix版本为第六版本，通常称其为V6。

其他公司同样把Unix一直到其他机器，这些移植带来其他Unix操作系统的变体。1977年，Bell实验室把这些变体整合到了一个系统 --- Unix System III； 1982年，AT&T公司发布了System V。

Unix简单的设计和其代码分发的特性，使很多外部组织可以参与到开发中。 其中最著名的要数伯克利的加利福尼亚大学的贡献者们。 伯克利的Unix变体我们现今称其为Berkeley Software Distributions，或是BSD。伯克利的第一个发行版1BSD于1977年发布，此发行版以Bell实验室的Unix为基础，病添加了各种补丁以及一些软件包。 第二代Unix系统2BSD于1978年发布，此版本包含了csh和vi组件，并且如今这些组件仍在Unix系统中使用。 第一个独立的伯克利Unix系统 -- 3BSD发布于1979年， 其添加了虚拟内存的概念。 3BSD之后，又发布了一系列4BSD发行版，4.0BSD, 4.1BSD, 4.2BSD, 4.3BSD。这些版本的Unix增加了工作控制，分页和TCP/IP协议。 1994年，大学发布了最终官方版本的伯克利Unix -- 4.4BSD， 并且重写了虚拟内存子系统。如今，多亏了BSD宽松的授权许可，使BSD仍可以在Darwin， FreeBSD， NetBSD和OpenBSD系统上进行开发。

上个世纪80年代到90年代， 工作站和服务器厂商发布了他们自己的商业版Unix系统。 这些系统都是基于AT&T或是伯克利发行版进行制作，并且针对他们特定的硬件架构进行了优化，例如， Digital's Tru64, 惠普的HP-UX， IBM的IAX，Sequent的DYNIX/ptx, SGI的IRIX，以及Sun公司的Solaris & SunOS。

Unix其优雅的设计理念，多年的发展和进化， 使其成为一个强大的、健壮且稳定的操作系统。 Unix有一系列的优点，首先。相对于一些有成百上千的系统调用和不明目的的系统来讲，Unix很简单。 Unix只包含几百个系统调用，而且很直接的，甚至可以说是很基础的设计。第二，在Unix系统中，一切皆文件。这种设计使得不管是操纵数据还是设备，都可以使用同一组核心系统调用：open(), read(), write(), lseek()以及close()。 第三， Unix内核和其相关的系统组件使用C语言进行的编写，使得Unix对于不同的硬件展现出了出色的可移植能力，使不同领域的开发者可以参与其中。 第四， Unix创建进程的时间非常快，而且还有独特的fork()系统调用。 最后， Unix提供了虽然单一但是很健壮的进程间通信（IPC)， 和其快速的进程创建时间一起，向我们展示了什么事做一件事，并且做到最好。因此，Unix系统提供了简洁的分层系统，有效的隔离了策略和机制。

如今， 现代Unix系统支持多任务抢占，多线程， 虚拟内存，按需分页，按需读取共享库文件，以及TCP/IP网络协议。 Unix系统可以运行在几百个处理器上，还有些Unix系统可以运行在嵌入式设备上。 虽然Unix已经不再是一个研究项目， 但是其务实且以目标为导向的设计理念仍然影响着其他操作系统。

Unix由于其简洁和优雅的设计理念而获得了成功， Dennis Ritchie， Ken Thompson和其他早期的开发者共同提出了这一理念。

[上一页](../README.md) [下一页](introduction-to-linux.md)



