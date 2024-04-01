# 一、MyBatis-Plus概述

## 一、简介

- [MyBatis-Plus (opens new window)](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个[MyBatis (opens new window)](https://www.mybatis.org/mybatis-3/)的增强工具，在MyBatis的基础上只做增强不做改变，为简化开发、提高效率而生

## 二、特性

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本CURD，性能基本无损耗，直接面向对象操作
- **强大的CRUD操作**：内置通用Mapper、通用Service，仅仅通过少量配置即可实现单表大部分CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持Lambda形式调用**：通过Lambda表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达4种主键策略（内含分布式唯一ID生成器 - Sequence），可自由配置，完美解决主键问题
- **支持ActiveRecord模式**：支持ActiveRecord形式调用，实体类只需继承Model类即可进行强大的CRUD操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者Maven插件可快速生成Mapper、Model、Service、 Controller层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于MyBatis物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通List查询
- **分页插件支持多种数据库**：支持MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer等多种数据库
- **内置性能分析插件**：可输出SQL语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表delete 、 update操作智能分析阻断，也可自定义拦截规则，预防误操作

## 三、支持数据库

- MySQL，Oracle，DB2，H2，HSQL，SQLite，PostgreSQL，SQLServer，Phoenix，Gauss ，ClickHouse，Sybase，OceanBase，Firebird，Cubrid，Goldilocks，csiidb
- 达梦数据库，虚谷数据库，人大金仓数据库，南大通用(华库)数据库，南大通用数据库，神通数据库，瀚高数据库

## 四、框架结构

<img src="https://baomidou.com/img/mybatis-plus-framework.jpg" alt="framework" style="zoom:50%;" />

## 五、官网

- [官方网站](https://baomidou.com/)
- [官方文档](https://baomidou.com/pages/24112f/)

# 二、入门案例

## 一、创建工程

1. 使用`Spring Initializer`快速初始化一个Spring Boot工程

   <img src="https://image-bed-vz.oss-cn-hangzhou.aliyuncs.com/MyBatis-Plus/image-20220519140839640.png" alt="image-20220519140839640" style="zoom:80%;" />

   <img src="https://image-bed-vz.oss-cn-hangzhou.aliyuncs.com/MyBatis-Plus/image-20220519141335981.png" alt="image-20220519141335981" style="zoom:80%;" />

   <img src="https://image-bed-vz.oss-cn-hangzhou.aliyuncs.com/MyBatis-Plus/image-20220519141737405.png" alt="image-20220519141737405" style="zoom:80%;" />

   <img src="https://image-bed-vz.oss-cn-hangzhou.aliyuncs.com/MyBatis-Plus/image-20220519141849937.png" alt="image-20220519141849937" style="zoom:80%;" />

2. 引入`MyBatis-Plus`的依赖

   ```xml
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-boot-starter</artifactId>
       <version>3.5.1</version>
   </dependency>
   ```

3. 安装`Lombok`插件

   <img src="https://image-bed-vz.oss-cn-hangzhou.aliyuncs.com/MyBatis-Plus/image-20220519143257305.png" alt="image-20220519143257305" style="zoom:80%;" />

## 二、配置编码

1. 配置`application.yml`文件

   ```yml
   #配置端口
   server:
     port: 80
   
   spring:
     #配置数据源
     datasource:
       #配置数据源类型
       type: com.zaxxer.hikari.HikariDataSource
       #配置连接数据库的信息
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/mybatis_plus?characterEncoding=utf-8&useSSL=false
       username: {username}
       password: {password}
   
   #MyBatis-Plus相关配置
   mybatis-plus:
     configuration:
       #配置日志
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   ```

2. 在Spring Boot启动类中添加 `@MapperScan` 注解，扫描Mapper文件夹

   ```java
   @SpringBootApplication
   @MapperScan("指定Mapper接口所在的包")
   public class MybatisPlusDemoApplication {
   	public static void main(String[] args) {
   		SpringApplication.run(MybatisPlusDemoApplication.class, args);
   	}
   }
   ```

3. 编写实体类 `User.java`

   ```java
   @Data
   public class User {
       private Long id;
       private String name;
       private Integer age;
       private String email;
   }
   ```

4. 编写Mapper包下的 `UserMapper`接口

   ```java
   public interface UserMapper extends BaseMapper<User> {}
   ```

# 三、CRUD

## 一、BaseMapper&lt;T>

- 通用CRUD封装BaseMapper接口，为 `Mybatis-Plus` 启动时自动解析实体表关系映射转换为`Mybatis` 内部对象注入容器
- 泛型 `T` 为任意实体对象
- 参数 `Serializable` 为任意类型主键 `Mybatis-Plus` 不推荐使用复合主键约定每一张表都有自己的唯一 `id` 主键
- 对象 `Wrapper` 为条件构造器