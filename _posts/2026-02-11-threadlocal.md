---
layout: post
title: ThreadLocal 线程局部变量
tags: [ThreadLocal, 后端开发技巧]
---
#### ThreadLocal

1. 什么是 ThreadLocal

ThreadLocal 是一个线程的私有存储空间。一个线程可以在其中存储变量，而变量的作用域是整个线程。

它的底层是一个存储在**堆空间**上的 ThreadLocalMap 的一个 Entry 对象，由于访问 ThreadLocal 首先需要获取线程，再获取 Map，这就隔离了不同线程的 ThreadLocal。

因此一个 ThreadLocal 实例只能存储一个对象，但一个线程可以有多个 ThreadLocal，它们都在线程的 ThreadLocalMap 里，一个线程一个 ThreadLocalMap

2. 为什么用它

对于需要获取请求详细参数的数据，如 header 的 jwt 令牌，可以在给定方法加 HttpServletRequest 参数，然后手动解析。但逻辑分散，代码侵入性高，只要方法需要就需要修改参数。使用 TreadLocal 可以只在拦截器处写一次，之后使用全局上下文类获取。

3. 常见问题
    - 释放内存：必须在请求结束时释放内存。springboot 使用线程池，每个请求使用线程池中的一个线程，新请求可能会读到应当销毁的数据造成内存泄漏。
    - key 的弱引用：保证当 key 丢失时（一个 threadLocal 实例的引用丢失），GC 可以回收 threadLocal 本身。但 value 是强引用，没法回收

