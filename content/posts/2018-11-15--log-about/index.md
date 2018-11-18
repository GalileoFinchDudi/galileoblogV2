---
title: 关于日志的那些事儿
author: Galileo Finch
category: "log"
cover: carolyn-v-546925-unsplash.jpg
---

# 什么是日志？

简单分为两种：

1. 用于人阅读的日志记录
2. 程序阅读的日志记录

## 人阅读的日志记录

这类我们接触的比较多了，当我们现网问题追踪，调试时的日志记录，哪怕js的console.log也能理解成一种日志记录。

如：

### syslog

[**Syslog**](https://en.wikipedia.org/wiki/Syslog)是[消息记录](https://en.wikipedia.org/wiki/Log_file)的标准，它允许分离生成消息的软件，存储消息的系统以及报告和分析消息的软件。每条消息都标有设施代码，表示生成消息的软件类型，并分配了严重性级别。

计算机系统设计人员可以使用syslog进行系统管理和安全审计以及一般信息，分析和调试消息。许多平台上的各种设备（如打印机，路由器和消息接收器）都使用syslog标准。这允许从中央存储库中的不同类型的系统合并日志数据。许多操作系统都存在syslog的实现。

### log4j 2

java上常用的Apache Log4j 2是一个基于Java的日志记录实用程序。

它和Log4j 1、java.util.logging。Log4j 1 的主要区别是：

- 提高可靠性。重新配置框架时，消息不会丢失，如Log4j 1或Logback
- 可扩展性：Log4j 2支持插件系统，允许用户定义和配置自定义组件
- 简化的配置语法
- 支持xml，json，yaml和属性配置
- 改进过滤器
- 属性查找支持在配置文件，系统属性，环境变量，ThreadContext Map和事件中存在的数据中定义的值
- 支持多个API：Log4j 2可以与使用Log4j 2，Log4j 1.2，SLF4J，Commons Logging和java.util.logging（JUL）API的应用程序一起使用。
- 自定义日志级别
- Java 8风格的lambda支持“懒惰日志”
- 标记
- 支持用户定义的Message对象
- 常见配置中的“无垃圾或低垃圾”
- 提高速度

这类日志的用处：

- 线上问题定位
  - 比如一些bug定位，或者是分析日志是否出现内存溢出、sql慢、网络异常等。
- 数据分析
  - 对日志进行统计分享，如行销活动对用户行为进行统计分析。

## 程序阅读的日志记录

接下来讨论程序阅读的日志。

### 数据库中的日志

在数据库里的用法是在崩溃的时候用它来保持各种数据结构和索引的同步。为了保证操作的原子性（`atomic`）和持久性（`durable`)， 在对数据库维护的所有各种数据结构做更改之前，数据库会把要做的更改操作的信息写入日志。 日志记录了发生了什么，而每个表或者索引都是更改历史中的一个投影。由于日志是立即持久化的，发生崩溃时，可以作为恢复其他所有持久化结构的可靠来源。

随着时间的推移，日志的用途从ACID的实现细节成长为数据库间复制数据的一种方法。 结果证明，发生在数据库上的更改序列 即是 与远程副本数据库（`replica database`）保持同步 所需的操作。 Oracle、MySQL 和PostgreSQL都包括一个日志传送协议（`log shipping protocol`），传输日志给作为备库（`slave`）的复本（`replica`）数据库。 Oracle还把日志产品化为一个通用的数据订阅机制，为非Oracle数据订阅用户提供了[`XStreams`](http://docs.oracle.com/cd/E11882_01/server.112/e16545/xstrm_intro.htm)和[`GoldenGate`](http://www.oracle.com/technetwork/middleware/goldengate/overview/index.html)，在MySQL和PostgreSQL中类似设施是许多数据架构的关键组件。

那我们可以使用日志来做什么操作呢？

1. 实现原子性、持久性
2. 日志回滚、归档
3. 数据的分发。使用日志传输实现数据备份、数据订阅

### 分布式系统中的日志

分布式系统以日志为中心的方案是来自于一个简单的观察，我称之为**状态机复制原理**（`State Machine Replication Principle`）：

**如果两个相同的、确定性的进程从同一状态开始，并且以相同的顺序获得相同的输入，那么这两个进程将会生成相同的输出，并且结束在相同的状态。**

再使用之前我们先了解，[确定性](http://en.wikipedia.org/wiki/Deterministic_algorithm)（`deterministic`）意味着处理过程是与时间无关的，而且不会让任何其他『带外』输入（`"out of band" input`）影响其处理结果。 例如，如果一个程序的输出会受到线程执行的具体顺序影响，或者受到`getTimeOfDay`调用、或者其他一些非重复性事件的影响，那么这样的程序一般被认为是非确定性的。

当你理解了这个以后，状态机复制原理就不再复杂或深奥了：这个原理差不多就等于说的是『确定性的处理过程就是确定性的』。不管怎样，它是分布式系统设计中一个更通用的工具。

这样方案的一个美妙之处就在于：用于索引日志的时间戳 就像 用于保持副本状态的时钟 —— 你可以只用一个数字来描述每一个副本，即这个副本已处理的最大日志记录的时间戳。 日志中的时间戳 一一对应了 副本的完整状态。

根据写进日志的内容，这个原理可以有不同的应用方式。举个例子，我们可以记录一个服务的输入请求日志，或者从请求到响应服务的状态变化日志，或者服务所执行的状态转换命令的日志。 理论上来说，我们甚至可以记录各个副本执行的机器指令序列的日志 或是 所调用的方法名和参数序列的日志。 只要两个进程用相同的方式处理这些输入，这些副本进程就会保持一致的状态。

分布式系统文献通常把处理和复制（`processing and replication`）方案宽泛地分成两种。『状态机器模型』常常被称为主-主模型（`active-active model`）， 记录输入请求的日志，各个复本处理每个请求。 对这个模型做了细微的调整称为『主备模型』（`primary-backup model`），即选出一个副本做为`leader`，让`leader`按请求到达的顺序处理请求，并输出它请求处理的状态变化日志。 其他的副本按照顺序应用`leader`的状态变化日志，保持和`leader`同步，并能够在`leader`失败的时候接替它成为`leader`。

![active_and_passive_arch](./active_and_passive_arch.png)

分布式日志可以看作是建模[一致性](https://en.wikipedia.org/wiki/Consensus_(computer_science))（`consensus`）问题的数据结构。 因为日志代表了『下一个』追加值的一系列决策。 你需要眯起眼睛才能从[Paxos](http://en.wikipedia.org/wiki/Paxos_(computer_science))算法簇中找到日志的身影，尽管构建日志是它们最常见的实际应用。 `Paxos`通过称为`multi-paxos`的一个扩展协议来构建日志，把日志建模为一系列一致性值的问题，日志的每个记录对应一个一致性值。 日志的身影在[`ZAB`](http://www.stanford.edu/class/cs347/reading/zab.pdf)、[`RAFT`](https://ramcloud.stanford.edu/wiki/download/attachments/11370504/raft.pdf)和[`Viewstamped Replication`](http://pmg.csail.mit.edu/papers/vr-revisited.pdf)等其它的协议中明显得多，这些协议建模的问题直接就是维护分布式一致的日志。

那这类数据的优点显而易见了：
1. 分布式计算内部实现或模型抽象
2. 数据集成（`Data Integration`） —— 让组织中所有存储和处理系统可以容易地访问组织所有的数据。
3. 实时数据处理 —— 计算生成的数据流。
4. 分布式系统设计 —— 如何通过集中式日志的设计来简化实际应用系统。

日志的好处都来自日志所能提供的简单功能：生成持久化的可重放的历史记录。 令人意外的是，能让多台机器以确定性的方式（deterministic manner）按各自的速度重放历史记录的能力是这些问题的核心。


## 参考文献：

[log-what-every-software-engineer-should-know-about-real-time-datas-unifying](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)

[Syslog](https://en.wikipedia.org/wiki/Syslog)

[Log_file](https://en.wikipedia.org/wiki/Log_file)

[xstrm_intro](http://docs.oracle.com/cd/E11882_01/server.112/e16545/xstrm_intro.htm)

[goldengate](http://www.oracle.com/technetwork/middleware/goldengate/overview/index.html)

[Consensus_(computer_science)](https://en.wikipedia.org/wiki/Consensus_(computer_science))

[Paxos](http://en.wikipedia.org/wiki/Paxos_(computer_science))

[ZAB](http://www.stanford.edu/class/cs347/reading/zab.pdf)

[RAFT](https://ramcloud.stanford.edu/wiki/download/attachments/11370504/raft.pdf)

[Viewstamped Replication](http://pmg.csail.mit.edu/papers/vr-revisited.pdf)
