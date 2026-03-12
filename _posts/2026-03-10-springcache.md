---
layout: post
title: Spring Cache
tags: [Spring Cache, spring, 后端开发技巧]
---

### Spring Cache

> 启用 Spring Cache 要在启动类或配置类加 `@EnableCache`

1. 常用注解
    1. `@Cacheable`
        - 表示方法的结果可以被缓存
        - 调用该方法时，首先会查缓存，若存在则直接返回，否则调用方法，将方法的返回值放入缓存再返回
        - 默认不缓存 null，可能导致缓存穿透（大量访问不存在数据的请求直达数据库）
    2. `@CachePut`
        - 表示将方法的返回值放入缓存
    3. `@CacheEvict`
        - 表示清理一条或多条缓存数据
        - 默认在方法执行成功后执行

2. 常用属性
    1. `cacheNames`：表示缓存的名称，实际表示一类数据，如 user 的缓存，dish 的缓存
    2. `key`：键名，支持 SpEL 表达式，可动态指定键名
    > 以上两个拼接组成实际缓存的键名：`cacheNames::key`
    > 在可视化工具里会构成一个树形结构，cacheNames 为根，key 为叶节点
    3. `condition`：满足条件时执行操作
    4. `unless`：满足条件时不执行操作
    5. `allEntries`：`@CacheEvict` 的属性，为 `true` 时删除所有缓存
    6. `beforeInvocation`：`@CacheEvict` 的属性，为 `true` 时一定执行删除操作，不论方法是否成功
    7. `keyGenerator`：自定义键生成器

    
    