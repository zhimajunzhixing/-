---
title: maven
date: 2022-11-02T10:51:01Z
lastmod: 2022-11-02T11:08:13Z
---

# maven

* [maven高级.pdf](assets/maven高级-20221102105614-x445bcn.pdf)
* 如何使用自研模块的资源

  * 导入依赖
  * 将自研模块打包到本地仓库（maven：install）或私服。
* 依赖管理

  * 直接依赖**`在当前项目中通过依赖`****[配置]()``****`建立的依赖关系`**
  * 间接依赖**`被资源的资源如果依赖其他资源，当前项目间接依赖其他资源`**
  * 依赖优先级

    * 路径优先

      * ```yaml
        * 当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
        * 一个项目Demo依赖了两个jar包，其中A-B-C-X(1.0) ， A-D-X(2.0)。由于X(2.0)路径最短，所以项目使用的是X(2.0)。
        ```
    * 声明优先

      * ```yaml
        * 当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
        * pom文件中申明顺序优先
        ```
    * 特殊优先

      * ```yaml
        当同级配置了相同资源的不同版本，后配置的覆盖先配置的
        ```
  * 可选依赖

    * what

      * ```yaml
        可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性。
        ```
    * how to

      * ```xml
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>maven_03_pojo</artifactId>
            <version>1.0-SNAPSHOT</version>
            <!--可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性-->
            <optional>true</optional>
        </dependency>
        ```
  * 排除依赖

    * what**`排除依赖指主动断开依赖的资源`**
    * how to

      * ```xml
        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>maven_03_pojo</artifactId>
            <version>1.0-SNAPSHOT</version>
            <!--可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递性-->
            <optional>true</optional>
        </dependency>
        ```
* 聚合

  * what**`将多个模块组织成一个整体，同时进行项目构建的过程称为聚合`**
  * 聚合工程

    * what`将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建`
    * how（[maven高级.pdf - p5 - 聚合工程开发](assets/maven高级-20221102105614-x445bcn.pdf?p=5)）
* 继承

  * what`子工程可以继承父工程中的配置信息，常见于依赖关系的继承`
  * how（[maven高级.pdf - p6 - 继承关系开发](assets/maven高级-20221102105614-x445bcn.pdf?p=6)）
  * 属性管理
* 工程版本

  * SNAPSHOT（快照版本）

    * ```yaml
      * 项目开发过程中临时输出的版本，称为快照版本
      * 快照版本会随着开发的进展不断更新
      ```
  * RELEASE（发布版本）

    * ```yaml
      * 项目开发到进入阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构件文件是稳定的
      * 即便进行功能的后续开发，也不会改变当前发布版本内容，这种版本称为发布版本
      ```
* [maven高级.pdf - p10 - 多环境配置与应用](assets/maven高级-20221102105614-x445bcn.pdf?p=10)
* [maven高级.pdf - p12 - 跳过测试](assets/maven高级-20221102105614-x445bcn.pdf?p=12)
* [maven高级.pdf - p12 - 私服](assets/maven高级-20221102105614-x445bcn.pdf?p=12)

  * [maven高级.pdf - p13 - Nexus安装与启动](assets/maven高级-20221102105614-x445bcn.pdf?p=13)
  * [maven高级.pdf - p14 - 私服仓库分类](assets/maven高级-20221102105614-x445bcn.pdf?p=14)
  * [maven高级.pdf - p14 - 从私服中下载依赖](assets/maven高级-20221102105614-x445bcn.pdf?p=14)
  * [maven高级.pdf - p15 - 上传依赖到私服中](assets/maven高级-20221102105614-x445bcn.pdf?p=15)
