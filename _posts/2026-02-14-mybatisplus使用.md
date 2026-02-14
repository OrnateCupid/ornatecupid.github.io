---
layout: post
title: Mybatis Plus 避免踩坑
tags: [Mybatis Plus, 雪花 ID, 后端开发技巧]
---

### 雪花 ID 主键导致前端精度溢出

1. 问题
    - 使用 `BaseMapper.insert()` 插入数据后：
    - 数据库中的主键：`2022574648097251329`
    - 前端返回并再次提交的主键：`2022574648097251300`
    - 导致查不到记录

2. 原因
    1. MyBatis Plus 的默认主键策略
        - 如果未显式指定，MP 默认使用 **雪花算法** 生成 19 位 long
    2. JS 精度溢出
        - js 的最大安全整数为 `2^53 - 1 ≈ 9.0e15`
        - 而雪花 ID 大约为 `2.0e18`
        - 前端使用 `Number("2022574648097251329")` 造成精度丢失

3. 解决办法
    1. 配置 MP 的默认主键生成方式
        - application.yaml
            ```yaml
                mybatis-plus:
                  global-config:
                    db-config:
                      id-type: auto
            ```
        - 或者使用配置类
            ```java
                @Configuration
                public class MybatisPlusConfig {

                    @Bean
                    public MybatisPlusPropertiesCustomizer idCustomizer() {
                        return properties -> properties.getGlobalConfig()
                            .getDbConfig()
                            .setIdType(IdType.AUTO);
                    }
                }

            ```
    2. 如果表已经插入过了雪花 ID 主键
        - 此时表的**自增起点已经被拉高**到了雪花 ID 的值，即使自增，也会使前端溢出
        - 只能在数据库层解决：重建表
        