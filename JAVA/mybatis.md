---
title: mybatis
date: 2022-10-31T22:19:48Z
lastmod: 2022-11-02T16:47:45Z
---

# mybatis

## 配置pom.xml建立maven依赖、

```xml
<dependencies>
  <!-- mybatis -->
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
  </dependency>
  <!-- mysql 驱动 -->
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.32</version>
  </dependency>
</dependencies>
```

## 配置mybatis配置文件

文件名：mybatis-config.xml

文件位置：\src\main\resources

### 配置数据库相关内容

```xml

    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/> 
        <!-- 指定数据库驱动 -->
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
         <!-- 指定数据连接地址、端口、数据库名 -->
        <property name="username" value="root"/> 
        <!-- 指定数据库用户名 -->
        <property name="password" value="1234"/>   
        <!-- 指定数据库密码 -->
      </dataSource>
    </environment>
```

### 配置默认环境

```xml

    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/> 
        <!-- 指定数据库驱动 -->
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
         <!-- 指定数据连接地址、端口、数据库名 -->
        <property name="username" value="root"/> 
        <!-- 指定数据库用户名 -->
        <property name="password" value="1234"/>   
        <!-- 指定数据库密码 -->
      </dataSource>
    </environment>
```

### 配置别名

设置别名，该包下所有类、接口在使用可直省略包名

```xml
<typeAliases>
    <package name="com.itheima.proj"/> 
    <!-- 设置别名，该包下所有类、接口在使用可直省略包名 -->
</typeAliases>
```

### 配置sql映射文件路径（逐个配置）

```xml
<mappers>
  <package resource="UserMapper.xml"/>    <!-- 指定sql映射xml所在目录 -->
</mappers>
```

### 配置映射文件目录

```xml
<mappers>
  <package name="com.itheima.mapper"/>  
  <!-- 指定sql映射xml所在目录 -->
</mappers>
```

## 配置SQL映射文件

### 文件名规范

一般→实体类名+Mapper.xml，如UserMapper.xml

mapper开发→同对应的接口保持一致

### 文件位置

一般→同mybatis同一目录即可，如\src\main\resources

mapper开发→与对应接口目录一致，如接口为com.itheima.mapper.UserMapper，则sql映射路径应为com/itheima/mapper/UserMapper.xml

### 文件结构

```xml
<mapper namespace="com.itheima.mapper.BrandMapper">

	<resultMap id="brandResultMap" type="brand">
		<result column="brand_name" property="brandName"/>
		<result column="company_name" property="companyName"/>
	</resultMap>
	<select id="selectAll" resultMap="brandResultMap">

		select * from tb_brand;
	</select>

</mapper>
```

标签<mapper>

属性namesapce的值

## 配置sql语句标签

### 查询标签<select>

#### 返回值接收

一般：通过属性resultType设置，值为全限定类名，将根据数据库字段名与类的属性名进行封闭。

别名生效→当设置包别名时，可以省略包名只写类名，不区大小写

#### 解决方案：类的属性名与数据表字段名不一致导致属性封装失败

使用as对数据表字段名起别名

```xml
<select id="selectAll" resultType="brand">
	select company_name as companyName from brand;
</select>
```

<resultmap>统一设置数据表别名

2.1通过设置别名规则

```yaml
- 属性id：本套别名规则的唯一标识，可供<select>引用  
- 标识别名涉及的类：type属性 
- <result>标签标识数据表字段名与对应的类属性名
    - 属性column：标识字段名  
    - 属性property：标识类属性名  
```

```xml
<resultMap id="brandResultMap" type="brand">
  <result column="brand_name" property="brandName"/>
  <result column="company_name" property="companyName"/>
</resultMap>
```

2.2删除的resultType属性，增加resultMap属性，其值为对应<resultmap>的的id

```xml
<select id="selectAll" resultMap="brandResultMap">
    select *
    from tb_brand;
</select>
```

### 更新标签<update>

返回值：影响的行数（int）

### 插入标签<insert>

#### 主键返回

what

插入成功后获取自动增长的主键值，封装至传递的对象

how

```yaml
- 在 insert 标签上添加如下属性
    - useGeneratedKeys↔是够获取自动增长的主键值。true表示获取
    - keyProperty↔指定将获取到的主键值封装到哪儿个属性里
    - <insert id="add" useGeneratedKeys="true" keyProperty="id">
```

```xml
<insert id="add" useGeneratedKeys="true" keyProperty="id">
    insert into tb_brand (brand_name, company_name, ordered, description, status)
    values (#{brandName}, #{companyName}, #{ordered}, #{description}, #{status});
</insert>
```

### 删除标签<delete>

### 占位符

what

将参数传入sql中的方式

how

#{参数名}，如 #{id}

### 特殊字符解决方案

转义字符

CDATA区，快捷键：CD+回车

### 接口方法形参数与sql语句形参映射方案

1.Param注解，格式为@Param(“sql形参名”)

```java
List<Brand> selectByCondition(@Param("status") int status);
```

2.传递类的对象作为参数，要求{{051755716816966224::类属性名与sql语句形参名保持一致}}。

3.传递Map对象作为参数，要求{{2947328294404763::key的值与sql语句形参名保持一致。 }}

### 动态SQL标签

* 功能：当传递参数要求不满足sql语法时，对sql语句进行修正。

#### if标签

```yaml
test属性：逻辑表达式设定
```

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    where
        <if test="status != null">
            and status = #{status}
        </if>
        <if test="companyName != null and companyName != '' ">
            and company_name like #{companyName}
        </if>
        <if test="brandName != null and brandName != '' ">
            and brand_name like #{brandName}
        </if>
</select>
```

#### where标签

```yaml
- 作用
    - 替换where关键字
    - 会动态的去掉第一个条件前的 and
    - 如果所有的参数没有值则不加where关键字
```

```xml
<select id="selectByCondition" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <if test="status != null">
            and status = #{status}
        </if>
        <if test="companyName != null and companyName != '' ">
            and company_name like #{companyName}
        </if>
        <if test="brandName != null and brandName != '' ">
            and brand_name like #{brandName}
        </if>
    </where>
</select>
```

#### choose标签

```xml
<select id="selectByConditionSingle" resultMap="brandResultMap">
    select *
    from tb_brand
    <where>
        <choose><!--相当于switch-->
            <when test="status != null"><!--相当于case-->
                status = #{status}
            </when>
            <when test="companyName != null and companyName != '' ">
            <!--相当于case-->
                company_name like #{companyName}
            </when>
            <when test="brandName != null and brandName != ''">
            <!--相当于case-->
                brand_name like #{brandName}
            </when>
        </choose>
    </where>
</select>
```

#### set标签

```yaml
功能:用于update 语句,替代set关键字，当接收参数后sql不符合语法规定时，自动进行纠正 
```

```xml
<update id="update">
    update tb_brand
    <set>
        <if test="brandName != null and brandName != ''">
            brand_name = #{brandName},
        </if>
        <if test="companyName != null and companyName != ''">
            company_name = #{companyName},
        </if>
        <if test="ordered != null">
            ordered = #{ordered},
        </if>
        <if test="description != null and description != ''">
            description = #{description},
        </if>
        <if test="status != null">
            status = #{status}
        </if>
    </set>
    where id = #{id};
</update>
```

### 传递数组方案for each 标签，将数组分割为sql要求的字符串

```yaml
sql位置：关键字in后
* collection：标识要遍历的数组}}，值为array接口方法的形参名（前提是@Param注解标识）
* item：标识集合中的元素，值要求与sql占位符中的形参保持一致
* separator：分隔符号，一般为“,”
* open：该属性值是在拼接SQL语句之前拼接的语句，只会拼接一次
* close：该属性值是在拼接SQL语句拼接后拼接的语句，只会拼接一次
```

```xml
<delete id="deleteByIds">
    delete from tb_brand where id
    in
    <foreach collection="array" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
    ;
</delete>
```

## 基本开发流程

```yaml
* 编写简单的sql影射语句
* 获取SqlSession对象
* 调用SqlSession对象的selectList()等方法，参数为sql语句映射的唯一识别符mapper标签的名称空间+ “.” + sql标签的id，如test . selectAll
* 释放资源
```

```java
//1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
//2. 获取SqlSession对象，用它来执行sql
SqlSession sqlSession = sqlSessionFactory.openSession();
//3. 执行sql
List<User> users = sqlSession.selectList("test.selectAll");
System.out.println(users);
//4. 释放资源
sqlSession.close();
```

## mapper开发流程

### 定义mapper接口

```yaml
* 接口名要求：与对应的sql映射文件同名
* 接口路径要求：一般存放于指定的mapper包，如com.itheima.mapper.UserMapper
* 接口方法要求：方法名与对应的sql语句的id保持一致，并确定返回值
```

### 定义mapper映射文件

### 代码

```yaml
* 获取获取SqlSession对象.
* 通过SqlSession对象的getMapper()方法获取UserMapper接口的UserMapper对象，参数为接口名.class，如UserMapper.class
* 调用UserMapper 对象的方法，即接口的方法
* 释放资源
```

```java
//1. 加载mybatis的核心配置文件，获取 SqlSessionFactory
String resource = "mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

//2. 获取SqlSession对象，用它来执行sql
SqlSession sqlSession = sqlSessionFactory.openSession();
//3. 执行sql
//List<User> users = sqlSession.selectList("test.selectAll");

//3.1 获取UserMapper接口的代理对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
List<User> users = userMapper.selectAll();
System.out.println(users);
//4. 释放资源
sqlSession.close();
```

### 注解开发

```yaml
* 功能：在mapper中，替代sql映射文件
* 应用场景：只适合一些简单的sql
```

```yaml
给数据表字段起别名
* sql映射文件编写标签
* 在接口的方法上添加注解@ResultMap("标签的id")，如@ @ResultMap("brandResultMap")
```

```java
@Select(value = "select * from tb_user where id = #{id}")
public User select(int id);
```

## 开启事务方法

```java
//获取SqlSession对象时设置自动提交事务 
SqlSession sqlSession = sqlSessionFactory.openSession(true);
//调用SqlSession对象commit()方法
sqlSession.commit();
```

## resultMap封装复杂类

**被封装的类结构**

```java
public class SpuItemAttrGroupVo {
    private String groupName;
    private List<Attr> attrs;
}
public class Attr {
    private Long attrId;
    private String attrName;
    private String attrValue;
} 
```

**xml配置**

```xml
<resultMap id="SpuItemAttrGroupVo" type="com.zhixing.gulimall.product.vo.SpuItemAttrGroupVo">
<!--使用了resultMap，则所以成员都要在这里声明，不然会报错-->
<result property="groupName" column="groupName"></result>
<!--这里封装集合成员-->
<collection property="attrs" ofType="com.zhixing.gulimall.product.vo.Attr">
<result property="attrId" column="attrId"></result>
<result property="attrName" column="attrName"></result>
<result property="attrValue" column="attrValue"></result>
</collection>
</resultMap>
```

‍
