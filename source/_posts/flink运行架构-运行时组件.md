---
title: flink运行架构-运行时组件
date: 2023-07-16 22:57:51
categories:
- 大数据
- flink
tags:
- flink运行时组件
---

# flink 运行时组件
![flink运行时组件](2023-07-16-23-10-30.png)

## 作业管理器（JobManager）
+ 控制一个应用程序执行的主进程，每一个应用程序都会被一个不同的JobManager所控制执行
+ JobManager 会先接收到要执行的应用程序，这个应用程序会包括：作业图（JobGraph）、逻辑数据流图（Logical dataflow graph）和打包了所有的类、库和其他资源的JAR包。
+ JobManager 会把JobGraph转换成一个物理层面的数据流图，这个图被叫做"执行图"（ExecutionGraph）,包含了所有可以并发执行的任务。
+ JobManager 会向资源管理器（ResourceManager）请求任务必要的资源，也就是任务管理器（TaskManager）上的插槽（Slot）。一旦它获取到了足够的资源，就会将执行图分发到真正运行他们的TaskManager上。而在运行过程中，JobManager会负责所有需要中央协调的操作，比如检查点（checkpoint）的协调。

## 任务管理器（TaskManager）
