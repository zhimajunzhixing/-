---
title: mybatis-plus
date: 2022-11-11T16:11:53Z
lastmod: 2022-11-11T16:15:32Z
---

# mybatis-plus

* 环境准备

  * 添加依赖，注释掉mybatis依赖

    * ```xml
      <dependency>
          <groupId>com.baomidou</groupId>
          <artifactId>mybatis-plus-boot-starter</artifactId>
          <version>3.4.1</version>
      </dependency>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.16</version>
      </dependency>
      ```
  * 定义数据接口，继承**BaseMapper**

    * ```java
      package com.itheima.dao;

      import com.baomidou.mybatisplus.core.mapper.BaseMapper;
      import com.itheima.domain.User;
      import org.apache.ibatis.annotations.Mapper;

      @Mapper
      public interface UserDao extends BaseMapper<User> {
      }
      ```
* 条件构造器

  * 实现类

    * QueryWrapper
    * LambdaQueryWrapper
  * 添加条件格式

    * 方式种类

      * QueryWrapper直接添加，对象名.方法（参数）;

        ```java
        QueryWrapper<User> qw=new QueryWrapper<>();
        qw.lt("age", 18);
        ```
      * QueryWrapper lambda格式添加，对象名.lambda().方法（参数）

        ```java
        QueryWrapper<User> qw = new QueryWrapper<User>();
        qw.lambda().lt(User::getAge, 10)
        ```
      * LambdaQueryWrapper，对象名.方法（参数）

        ```java

        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 10);
        ```
    * 变量名标识方式

      * 字符串
      * lambda：类名::get方法名
  * 运算符API

    * 等于=
    * 不等于 <>
    * 大于 >
    * 大于等于 >=
    * 小于 <
    * 小于等于 <=
    * BETWEEN 值1 AND 值2
    * NOT BETWEEN 值1 AND 值2
    * LIKE '%值%'
    * NOT LIKE '%值%'
    * LIKE '%值'
    * LIKE '值%'
    * 字段 IS NULL
    * 字段 IS NOT NULL
    * 字段 IN (value.get(0), value.get(1), ...)
    * 字段 NOT IN (value.get(0), value.get(1), ...)
    * 字段 IN ( sql语句 )
    * 字段 NOT IN ( sql语句 )
    * 分组：GROUP BY 字段
    * 排序：ORDER BY 字段, ... ASC
    * 排序：ORDER BY 字段, ...
  * 组合条件

    * 并且关系

      * ```java
        lqw.lt(User::getAge, 30).gt(User::getAge, 10);
        ```
    * 或者关系

      * ```

        lqw.lt(User::getAge, 10).or().gt(User::getAge, 30);
        ```
