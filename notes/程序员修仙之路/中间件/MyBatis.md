# 一、Mybatis简介

## 一、Mybatis历史

- MyBatis最初是Apache的一个开源项目iBatis, 2010年6月这个项目由Apache Software Foundation迁移到了Google Code。随着开发团队转投Google Code旗下，iBatis3.x正式更名为MyBatis。代码于2013年11月迁移到Github
- iBatis一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。iBatis提供的持久层框架包括SQL Maps和Data Access Objects（DAO）

## 二、Mybatis特性

1. MyBatis是支持定制化SQL、存储过程以及高级映射的优秀的持久层框架
2. MyBatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集
3. MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录
4. MyBatis是一个半自动的ORM（Object Relation Mapping）框架

## 三、下载地址

- [MyBatis下载地址](https://github.com/mybatis/mybatis-3)

## 四、对比其他持久化层技术

1. JDBC  

   - SQL夹杂在Java代码中耦合度高，导致硬编码内伤  

   - 维护不易且实际开发需求中SQL有变化，频繁修改的情况多见  
   - 代码冗长，开发效率低

2. Hibernate和JPA

   - 操作简便，开发效率高  

   - 程序中的长难复杂SQL需要绕过框架  
   - 内部自动生产的SQL，不容易做特殊优化  
   - 基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难  
   - 反射操作太多，导致数据库性能下降

3. MyBatis

   - 轻量级，性能出色  

   - SQL和Java编码分开，功能边界清晰。Java代码专注业务、SQL语句专注数据  
   - 开发效率稍逊于HIbernate，但是完全能够接受

# 二、Mybatis搭建

## 一、创建maven工程

- 打包方式：jar

- 引入依赖

  ```java
  <dependencies>
  	<!-- Mybatis核心 -->
  	<dependency>
  		<groupId>org.mybatis</groupId>
  		<artifactId>mybatis</artifactId>
  		<version>3.5.7</version>
  	</dependency>
  	<!-- junit测试 -->
  	<dependency>
  		<groupId>junit</groupId>
  		<artifactId>junit</artifactId>
  		<version>4.12</version>
  		<scope>test</scope>
  	</dependency>
  	<!-- MySQL驱动 -->
  	<dependency>
  		<groupId>mysql</groupId>
  		<artifactId>mysql-connector-java</artifactId>
  		<version>5.1.3</version>
  		</dependency>
  </dependencies>
  ```

## 二、创建Mybatis核心配置文件

- 核心配置文件习惯上命名为`mybatis-config.xml`，这个文件名仅仅只是建议，并非强制要求。将来整合Spring之后，这个配置文件可以省略

- 核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息

- 核心配置文件存放的位置是src/main/resources目录下

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>  
  <!DOCTYPE configuration  
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
  "http://mybatis.org/dtd/mybatis-3-config.dtd">  
  <configuration>  
  	<!--设置连接数据库的环境-->  
  	<environments default="development">  
  		<environment id="development">  
  			<transactionManager type="JDBC"/>  
  			<dataSource type="POOLED">  
  				<property name="driver" value="com.mysql.cj.jdbc.Driver"/>  
  				<property name="url" value="jdbc:mysql://localhost:3306/MyBatis"/>  
  				<property name="username" value="root"/>  
  				<property name="password" value="123456"/>  
  			</dataSource>  
  		</environment>  
  	</environments>  
  	<!--引入映射文件-->  
  	<mappers>  
  		<mapper resource="mappers/UserMapper.xml"/>  
  	</mappers>  
  </configuration>
  ```

## 三、创建mapper接口

- MyBatis中的mapper接口相当于以前的dao。但是区别在于，mapper仅仅是接口，我们不需要提供实现类

  ```java
  public interface UserMapper {  
  	/**  
  	* 添加用户信息  
  	*/  
  	int insertUser();  
  }
  ```

## 四、创建Mybatis映射文件

1. 相关概念：ORM（Object Relationship Mapping）对象关系映射

2. 对象：Java的实体类对象

   - 关系：关系型数据库
   - 映射：二者之间的对应关系

   | Java概念 | 数据库概念 |
   | :------: | :--------: |
   |    类    |     表     |
   |   属性   |  字段/列   |
   |   对象   |  记录/行   |

3. 映射文件的命名规范

   - 表所对应的实体类的类名+Mapper.xml
   - 一个映射文件对应一个实体类，对应一张表的操作
   - MyBatis映射文件用于编写SQL，访问以及操作表中的数据
   - MyBatis映射文件存放的位置是src/main/resources/mappers目录下

4. MyBatis中可以面向接口操作数据，要保证两个一致

 5. mapper接口的全类名和映射文件的命名空间（namespace）保持一致

    - mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>  
    <!DOCTYPE mapper  
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
    <mapper namespace="com.atguigu.mybatis.mapper.UserMapper">  
    	<!--int insertUser();-->  
    	<insert id="insertUser">  
    	</insert>  
    </mapper>
    ```

## 五、通过junit测试功能

1. SqlSession：代表Java程序和数据库之间的会话（HttpSession是Java程序和浏览器之间的会话）

2. SqlSessionFactory：是“生产”SqlSession的“工厂”

3. 工厂模式：如果创建某一个对象，使用的过程基本固定，那么我们就可以把创建这个对象的相关代码封装到一个“工厂类”中，以后都使用这个工厂类来“生产”我们需要的对象

   ```java
   public class UserMapperTest {
       @Test
       public void testInsertUser() throws IOException {
           //读取MyBatis的核心配置文件
           InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
           //获取SqlSessionFactoryBuilder对象
           SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
           //通过核心配置文件所对应的字节输入流创建工厂类SqlSessionFactory，生产SqlSession对象
           SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
           //获取sqlSession，此时通过SqlSession对象所操作的sql都必须手动提交或回滚事务
           //SqlSession sqlSession = sqlSessionFactory.openSession();
   	    //创建SqlSession对象，此时通过SqlSession对象所操作的sql都会自动提交  
   		SqlSession sqlSession = sqlSessionFactory.openSession(true);
           //通过代理模式创建UserMapper接口的代理实现类对象
           UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
           //调用UserMapper接口中的方法，就可以根据UserMapper的全类名匹配元素文件，通过调用的方法名匹配映射文件中的SQL标签，并执行标签中的SQL语句
           int result = userMapper.insertUser();
           //提交事务
           //sqlSession.commit();
           System.out.println("result:" + result);
       }
   }
   ```

## 六、加入log4j日志功能

1. 加入依赖

   ```xml
   <!-- log4j日志 -->
   <dependency>
   <groupId>log4j</groupId>
   <artifactId>log4j</artifactId>
   <version>1.2.17</version>
   </dependency>
   ```

2. 加入log4j配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
   <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
       <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
           <param name="Encoding" value="UTF-8" />
           <layout class="org.apache.log4j.PatternLayout">
   			<param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m (%F:%L) \n" />
           </layout>
       </appender>
       <logger name="java.sql">
           <level value="debug" />
       </logger>
       <logger name="org.apache.ibatis">
           <level value="info" />
       </logger>
       <root>
           <level value="debug" />
           <appender-ref ref="STDOUT" />
       </root>
   </log4j:configuration>
   ```

   - log4j的配置文件名为log4j.xml，存放的位置是src/main/resources目录下
   - 日志的级别：FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试) 从左到右打印的内容越来越详细

# 三、Mybatis核心配置文件介绍

核心配置文件中的标签必须按照固定的顺序(有的标签可以不写，但顺序一定不能乱)：

1. properties
2. settings
3. typeAliases
4. typeHandlers
5. objectFactory
6. objectWrapperFactory
7. reflectorFactory
8. plugins
9. environments
10. databaseIdProvider
11. mappers

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//MyBatis.org//DTD Config 3.0//EN"
        "http://MyBatis.org/dtd/MyBatis-3-config.dtd">
<configuration>
    <!--引入properties文件，此时就可以${属性名}的方式访问属性值-->
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!--将表中字段的下划线自动转换为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
    <typeAliases>
        <!--
        typeAlias：设置某个具体的类型的别名
        属性：
        type：需要设置别名的类型的全类名
        alias：设置此类型的别名，且别名不区分大小写。若不设置此属性，该类型拥有默认的别名，即类名
        -->
        <!--<typeAlias type="com.sunny.mybatis.bean.User"></typeAlias>-->
        <!--<typeAlias type="com.sunny.mybatis.bean.User" alias="user">
        </typeAlias>-->
        <!--以包为单位，设置该包下所有的类型都拥有默认的别名，即类名且不区分大小写-->
        <package name="com.sunny.mybatis.bean"/>
    </typeAliases>
    <!--
    environments：设置多个连接数据库的环境
    属性：
	    default：设置默认使用的环境的id
    -->
    <environments default="mysql_test">
        <!--
        environment：设置具体的连接数据库的环境信息
        属性：
	        id：设置环境的唯一标识，可通过environments标签中的default设置某一个环境的id，表示默认使用的环境
        -->
        <environment id="mysql_test">
            <!--
            transactionManager：设置事务管理方式
            属性：
	            type：设置事务管理方式，type="JDBC|MANAGED"
	            type="JDBC"：设置当前环境的事务管理都必须手动处理
	            type="MANAGED"：设置事务被管理，例如spring中的AOP
            -->
            <transactionManager type="JDBC"/>
            <!--
            dataSource：设置数据源
            属性：
	            type：设置数据源的类型，type="POOLED|UNPOOLED|JNDI"
	            type="POOLED"：使用数据库连接池，即会将创建的连接进行缓存，
							下次使用可以从缓存中直接获取，不需要重新创建
	            type="UNPOOLED"：不使用数据库连接池，即每次使用连接都需要重新创建
	            type="JNDI"：调用上下文中的数据源
            -->
            <dataSource type="POOLED">
                <!--设置驱动类的全类名-->
                <property name="driver" value="${jdbc.driver}"/>
                <!--设置连接数据库的连接地址-->
                <property name="url" value="${jdbc.url}"/>
                <!--设置连接数据库的用户名-->
                <property name="username" value="${jdbc.username}"/>
                <!--设置连接数据库的密码-->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <!-- <mapper resource="UserMapper.xml"/> -->
        <!--
        以包为单位，将包下所有的映射文件引入核心配置文件
        注意：
			1. 此方式必须保证mapper接口和mapper映射文件必须在相同的包下
			2. mapper接口要和mapper映射文件的名字一致
        -->
        <package name="com.sunny.mybatis.mapper"/>
    </mappers>
</configuration>
```

# 四、Mybatis默认的类型别名

![默认的类型别名](../../../TyporaImage/mybatis/默认的类型别名1.png)

![默认的类型别名](../../../TyporaImage/%E9%BB%98%E8%AE%A4%E7%9A%84%E7%B1%BB%E5%9E%8B%E5%88%AB%E5%90%8D2.png)

# 五、Mybatis查询注意点

1. 查询的标签select必须设置属性resultType或resultMap，用于设置实体类和数据库表的映射关系  
   - resultType：自动映射，用于属性名和表中字段名一致的情况  
   - resultMap：自定义映射，用于一对多或多对一或字段名和属性名不一致的情况  
2. 当查询的数据为多条时，不能使用实体类作为返回值，只能使用集合，否则会抛出异常TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值

# 六、MyBatis获取参数值的两种方式