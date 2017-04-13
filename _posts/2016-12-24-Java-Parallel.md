---
layout: post
title: Java Parallel
---

# {{page.title}}

{{page.date | date_to_string}} - Beijing

## 按照共享资源的方式划分

Disorder,Exclusive | Disorder,Count Limit | Order,Sequence | Order,Priority | read?write?
------------------ | -------------------- | -------------- | -------------- | -----------
Lock?              | Semaphore?           | ?              | wait,notify?   | ?

## 按照线程（进程？）的执行空间划分

Linear   | Topology
-------- | ----------------------
Executor | ForkJoinPool,fork,join

## 应用实例
