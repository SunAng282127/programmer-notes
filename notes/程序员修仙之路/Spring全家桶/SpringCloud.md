# 一、SpringCloud简介

## 一、SpringCloud版本选型

1. Java：Java17+
2. cloud：2023.0.0
3. boot：3.2.0，cloud版本的选择决定了boot的版本的选型
4. cloud：alibaba：2022.0.0.0-RC2
5. Maven：3.9+
6. MySQL：8.2+

## 二、SpringCould各个组件大纲

![](../../../TyporaImage/d9ecd5f5414d407aa9830d951db891dc.png)

1. 服务注册与发现：实现服务治理（服务注册与发现）（核心组件） 
   - Eureka（不推荐使用）
   - Consul
   - Etcd
   - Nacos
2. 服务调用和负载均衡：主要提供客户侧的软件负载均衡算法 （核心组件） 
   - Ribbon（已过时）
   - OpenFeign
   - LoadBalancer
3. 分布式事务
   - Seata
   - LCN
   - Hmily
4. 服务熔断和降级：断路器，保护系统，控制故障范围 （核心组件） 
   - Hystrix（已过时）
   - Circuit Breaker，它仅仅是接口，实现时使用Resikience4J做支撑
   - Sentinel
5. 服务链路追踪
   - Sleuth + Zipkin（已过时）
   - Micrometer Tracing
6. 服务网关：api网关，路由，负载均衡等多种作用 （核心组件） 
   - Zuul（已过时）
   - Gate Way
7. 分布式配置管理：配置管理（核心组件） 
   - Config + Bus（已过时）
   - Consul
   - Naclos

# 二、微服务架构Base工程模块构建

## 一、搭建Maven父工程

1. 创建Maven工程中的新Project，聚合总工程的名字默认为下图中的Name，`GroupId`为`com.atguigu.cloud`

   ![image-20240518105459597](../../../TyporaImage/image-20240518105459597.png)

2. 字符编码

   ![image-20240518110002572](../../../TyporaImage/image-20240518110002572.png)

3. 注解生效激活

   ![image-20240518110128729](../../../TyporaImage/image-20240518110128729.png)

4. Java编译版本选自已的（Java17+）

5. File Type过滤：去掉不想看到的文件

6. 注意Maven版本以及Java版本，先检查一遍

7. 在pom文件中，加上`<packaging>pom</packaging>`则表示此Maven是父类，一般打包方式为pom的是父级项目且不会产生`jar/war`包。如果依赖一致飘红则先把`<dependencyManagement></dependencyManagement>`去掉先下载依赖后再加

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.atguigu.cloud</groupId>
       <artifactId>cloud2024</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>pom</packaging>
       <modules>
           <module>mybatis_generator2024</module>
       </modules>
   
       <properties>
           <maven.compiler.source>21</maven.compiler.source>
           <maven.compiler.target>21</maven.compiler.target>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
           <hutool.version>5.8.22</hutool.version>
           <lombok.version>1.18.26</lombok.version>
           <druid.version>1.1.20</druid.version>
           <mybatis.springboot.version>3.0.2</mybatis.springboot.version>
           <mysql.version>8.0.11</mysql.version>
           <swagger3.version>2.2.0</swagger3.version>
           <mapper.version>4.2.3</mapper.version>
           <fastjson2.version>2.0.40</fastjson2.version>
           <persistence-api.version>1.0.2</persistence-api.version>
           <spring.boot.test.version>3.1.5</spring.boot.test.version>
           <!--<spring.boot.version>3.2.0</spring.boot.version>
           <spring.cloud.version>2023.0.0</spring.cloud.version>-->
   
           <!--仅为了整合openfeign + alibaba sentinel的案例，降低版本处理下-->
           <!--<spring.boot.version>3.0.9</spring.boot.version>
           <spring.cloud.version>2022.0.2</spring.cloud.version>-->
   
           <!--仅为了整合openfeign + alibaba seata的案例，降低版本处理下-->
           <spring.boot.version>3.1.7</spring.boot.version>
           <spring.cloud.version>2022.0.4</spring.cloud.version>
   
           <spring.cloud.alibaba.version>2022.0.0.0</spring.cloud.alibaba.version>
           <micrometer-tracing.version>1.2.0</micrometer-tracing.version>
           <micrometer-observation.version>1.12.0</micrometer-observation.version>
           <feign-micrometer.version>12.5</feign-micrometer.version>
           <zipkin-reporter-brave.version>2.17.0</zipkin-reporter-brave.version>
       </properties>
   
       <dependencyManagement>
           <dependencies>
               <!--springboot 3.2.0-->
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-starter-parent</artifactId>
                   <version>${spring.boot.version}</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--springcloud 2023.0.0-->
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-dependencies</artifactId>
                   <version>${spring.cloud.version}</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--springcloud alibaba 2022.0.0.0-RC2-->
               <dependency>
                   <groupId>com.alibaba.cloud</groupId>
                   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                   <version>${spring.cloud.alibaba.version}</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--SpringBoot集成mybatis-->
               <dependency>
                   <groupId>org.mybatis.spring.boot</groupId>
                   <artifactId>mybatis-spring-boot-starter</artifactId>
                   <version>${mybatis.springboot.version}</version>
               </dependency>
               <!--Mysql数据库驱动8 -->
               <dependency>
                   <groupId>mysql</groupId>
                   <artifactId>mysql-connector-java</artifactId>
                   <version>${mysql.version}</version>
               </dependency>
               <!--SpringBoot集成druid连接池-->
               <dependency>
                   <groupId>com.alibaba</groupId>
                   <artifactId>druid-spring-boot-starter</artifactId>
                   <version>${druid.version}</version>
               </dependency>
               <!--通用Mapper4之tk.mybatis-->
               <dependency>
                   <groupId>tk.mybatis</groupId>
                   <artifactId>mapper</artifactId>
                   <version>${mapper.version}</version>
               </dependency>
               <!--persistence-->
               <dependency>
                   <groupId>javax.persistence</groupId>
                   <artifactId>persistence-api</artifactId>
                   <version>${persistence-api.version}</version>
               </dependency>
               <!-- fastjson2 -->
               <dependency>
                   <groupId>com.alibaba.fastjson2</groupId>
                   <artifactId>fastjson2</artifactId>
                   <version>${fastjson2.version}</version>
               </dependency>
               <!-- swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
               <dependency>
                   <groupId>org.springdoc</groupId>
                   <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
                   <version>${swagger3.version}</version>
               </dependency>
               <!--hutool-->
               <dependency>
                   <groupId>cn.hutool</groupId>
                   <artifactId>hutool-all</artifactId>
                   <version>${hutool.version}</version>
               </dependency>
               <!--lombok-->
               <dependency>
                   <groupId>org.projectlombok</groupId>
                   <artifactId>lombok</artifactId>
                   <version>${lombok.version}</version>
                   <optional>true</optional>
               </dependency>
               <!-- spring-boot-starter-test -->
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-starter-test</artifactId>
                   <version>${spring.boot.test.version}</version>
                   <scope>test</scope>
               </dependency>
           </dependencies>
       </dependencyManagement>
   
   </project>
   ```

## 二、搭建mybatis_generator工程

1. 搭建一个普通的Maven工程

2. 引入pom文件依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.atguigu.cloud</groupId>
           <artifactId>cloud2024</artifactId>
           <version>1.0-SNAPSHOT</version>
           <relativePath>../pom.xml</relativePath>
       </parent>
   
       <artifactId>mybatis_generator2024</artifactId>
   
       <properties>
           <maven.compiler.source>21</maven.compiler.source>
           <maven.compiler.target>21</maven.compiler.target>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       </properties>
   
       <dependencies>
           <!--Mybatis 通用mapper tk单独使用，自己独有+自带版本号-->
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.13</version>
           </dependency>
           <!-- Mybatis Generator 自己独有+自带版本号-->
           <dependency>
               <groupId>org.mybatis.generator</groupId>
               <artifactId>mybatis-generator-core</artifactId>
               <version>1.4.2</version>
           </dependency>
           <!--通用Mapper-->
           <dependency>
               <groupId>tk.mybatis</groupId>
               <artifactId>mapper</artifactId>
           </dependency>
           <!--mysql8.0-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <!--persistence-->
           <dependency>
               <groupId>javax.persistence</groupId>
               <artifactId>persistence-api</artifactId>
           </dependency>
           <!--hutool-->
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
           </dependency>
           <!--lombok-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
               <exclusions>
                   <exclusion>
                       <groupId>org.junit.vintage</groupId>
                       <artifactId>junit-vintage-engine</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
       </dependencies>
   
       <build>
           <resources>
               <resource>
                   <directory>${basedir}/src/main/java</directory>
                   <includes>
                       <include>**/*.xml</include>
                   </includes>
               </resource>
               <resource>
                   <directory>${basedir}/src/main/resources</directory>
               </resource>
           </resources>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
                   <configuration>
                       <excludes>
                           <exclude>
                               <groupId>org.projectlombok</groupId>
                               <artifactId>lombok</artifactId>
                           </exclude>
                       </excludes>
                   </configuration>
               </plugin>
               <plugin>
                   <groupId>org.mybatis.generator</groupId>
                   <artifactId>mybatis-generator-maven-plugin</artifactId>
                   <version>1.4.2</version>
                   <configuration>
                       <configurationFile>${basedir}/src/main/resources/generatorConfig.xml</configurationFile>
                       <overwrite>true</overwrite>
                       <verbose>true</verbose>
                   </configuration>
                   <dependencies>
                       <dependency>
                           <groupId>mysql</groupId>
                           <artifactId>mysql-connector-java</artifactId>
                           <version>8.0.33</version>
                       </dependency>
                       <dependency>
                           <groupId>tk.mybatis</groupId>
                           <artifactId>mapper</artifactId>
                           <version>4.2.3</version>
                       </dependency>
                   </dependencies>
               </plugin>
           </plugins>
       </build>
   
   </project>
   ```

3. 配置JDBC文件

   ```properties
   #t_pay表包名
   package.name=com.atguigu.cloud
   
   # mysql8.0
   jdbc.driverClass = com.mysql.cj.jdbc.Driver
   jdbc.url= jdbc:mysql://localhost:13306/db2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
   jdbc.user = root
   jdbc.password = 282127
   ```

4. 配置mybatis插件生成

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE generatorConfiguration
           PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
           "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   
   <generatorConfiguration>
       <properties resource="config.properties"/>
   
       <context id="Mysql" targetRuntime="MyBatis3Simple" defaultModelType="flat">
           <property name="beginningDelimiter" value="`"/>
           <property name="endingDelimiter" value="`"/>
   
           <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
               <property name="mappers" value="tk.mybatis.mapper.common.Mapper"/>
               <property name="caseSensitive" value="true"/>
           </plugin>
   
           <!-- 数据库配置 -->
           <jdbcConnection driverClass="${jdbc.driverClass}"
                           connectionURL="${jdbc.url}"
                           userId="${jdbc.user}"
                           password="${jdbc.password}">
           </jdbcConnection>
   
           <!-- 配置config.properties中文件的生成路径 -->
           <javaModelGenerator targetPackage="${package.name}.entities" targetProject="src/main/java"/>
   
           <sqlMapGenerator targetPackage="${package.name}.mapper" targetProject="src/main/java"/>
   
           <javaClientGenerator targetPackage="${package.name}.mapper" targetProject="src/main/java" type="XMLMAPPER"/>
   
           <!--配置想要生成的表名 -->
           <table tableName="t_pay" domainObjectName="Pay">
               <generatedKey column="id" sqlStatement="JDBC"/>
           </table>>
   
       </context>
   </generatorConfiguration>
   
   
   ```

5. 生成文件时，点击maven -> mybatis_generator2024 -> mybatis_generator -> mybatis_generator:generat即可自动生成entitiy和Mapper层的代码文件

## 三、微服务提供者支付工程

1. 构建完整的微服务步骤小口诀

   - 建module
   - 改pom
   - 写yml
   - 主启动
   - 业务类

2. 搭建cloud-provider-payment8001工程

3. 引入pom文件依赖

   ```xml
   <dependencies>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-bootstrap</artifactId>
           </dependency>
           <!--SpringCloud consul discovery -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-consul-discovery</artifactId>
               <exclusions>
                   <exclusion>
                       <groupId>commons-logging</groupId>
                       <artifactId>commons-logging</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
           <!--SpringBoot通用依赖模块-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--SpringBoot集成druid连接池-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid-spring-boot-starter</artifactId>
           </dependency>
           <!-- Swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
           <dependency>
               <groupId>org.springdoc</groupId>
               <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
           </dependency>
           <!--mybatis和springboot整合-->
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
           </dependency>
           <!--Mysql数据库驱动8 -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <!--persistence-->
           <dependency>
               <groupId>javax.persistence</groupId>
               <artifactId>persistence-api</artifactId>
           </dependency>
           <!--通用Mapper4-->
           <dependency>
               <groupId>tk.mybatis</groupId>
               <artifactId>mapper</artifactId>
           </dependency>
           <!--hutool-->
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
           </dependency>
           <!-- fastjson2 -->
           <dependency>
               <groupId>com.alibaba.fastjson2</groupId>
               <artifactId>fastjson2</artifactId>
           </dependency>
           <!--lombok-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.28</version>
               <scope>provided</scope>
           </dependency>
           <!--test-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   ```

4. 编写yml文件`application.yml`

   ```yaml
   server:
     port: 8001
   
   # ==========applicationName + druid-mysql8 driver===================
   spring:
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/db2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
       username: root
       password: 123456
     profiles:
       active: dev # 多环境配置加载内容dev/prod,不写就是默认default配置
   
   # ========================mybatis===================
   mybatis:
     mapper-locations: classpath:mapper/*.xml
     type-aliases-package: com.atguigu.cloud.entities
     configuration:
       map-underscore-to-camel-case: true
   ```

5. 在resources文件夹下添加mapper文件目录，放入mapper文件

6. 编写启动类Main8001，启动类的名称可以随便取，但是也要见名知意，但是一定要放在其他层级代码的顶级，如放在`com.atguigu.cloud`包下，这是SpringBoot的规则

   ```java
   @SpringBootApplication
   @MapperScan("com.atguigu.cloud.mapper")
   //tk.mybatis.spring.annotation.MapperScan可以省略在Mapper层写@Mapper
   public class Main8001 {
   
       public static void main(String[] args) {
           SpringApplication.run(Main8001.class,args);
       }
   }
   ```

7. 将一键生成的代码放入新工程下指定的目录下即可

8. 详谈目录结构

   - entities包：主实体类Pay（必须和数据库字段一一对应）、传递数值PayDTO（用于和页面进行交互，数据传输）
   - mapper包：java目录下的mapper包放mapper接口，resources/mapper下放mapper.xml文件
   - service包：包括service接口以及service接口的实现类，service的实现类一定要加`@Service`，在引入mapper接口时推荐使用`@Resource`
   - controller包：用于接口访问

9. CURD整套输出

   - controller

     ```java
     @RestController
     @RequestMapping("/pay")
     @Slf4j
     public class PayController {
     
         @Resource
         private IPayService payService;
     
         @PostMapping(value = "/addPay")
         public String addPay(@RequestBody Pay pay) {
             int i = payService.add(pay);
             return i > 0 ? "支付成功" : "支付失败";
         }
     
         @DeleteMapping(value = "/deletePay/{id}")
         public String delete(@PathVariable("id") Integer id) {
             int i = payService.delete(id);
             return i > 0 ? "支付删除成功" : "支付删除失败";
         }
     
         @PutMapping(value = "/updatePay")
         public String update(@RequestBody PayDTO payDTO) {
             Pay pay = new Pay();
             BeanUtil.copyProperties(payDTO, pay);
             int i = payService.update(pay);
             return i > 0 ? "支付修改成功" : "支付修改失败";
         }
     
         @GetMapping(value = "/getInfoById/{id}")
         public Map<String, Object> getInfoById(@PathVariable("id") Integer id) {
             Pay pay = payService.getInfoById(id);
             PayDTO payDTO = new PayDTO();
             BeanUtil.copyProperties(pay, payDTO);
             Map<String, Object> map = new HashMap<>();
             map.put("msg", "支付详情");
             map.put("data", payDTO);
             return map;
         }
     
         @GetMapping(value = "/getList")
         public Map<String, Object> getList() {
             List<Pay> payList = payService.getList();
             List<PayDTO> payDTOList = new ArrayList<>();
             for (Pay pay : payList) {
                 PayDTO payDTO = new PayDTO();
                 BeanUtil.copyProperties(pay,payDTO);
                 payDTOList.add(payDTO);
             }
             Map<String, Object> map = new HashMap<>();
             map.put("msg", "支付列表");
             map.put("data", payDTOList);
             return map;
         }
     }
     ```

   - service

     ```java
     public interface IPayService {
     
         public int add(Pay pay);
     
         public int delete(Integer id);
     
         public int update(Pay pay);
     
         public Pay getInfoById(Integer id);
     
         public List<Pay> getList();
     }
     ```

     ```java
     @Service
     public class PayServiceImpl implements IPayService {
     
         @Resource
         private PayMapper payMapper;
     
         @Override
         public int add(Pay pay) {
             return payMapper.insertSelective(pay);
         }
     
         @Override
         public int delete(Integer id) {
             return payMapper.deleteByPrimaryKey(id);
         }
     
         @Override
         public int update(Pay pay) {
             return payMapper.updateByPrimaryKeySelective(pay);
         }
     
         @Override
         public Pay getInfoById(Integer id) {
             return payMapper.selectByPrimaryKey(id);
         }
     
         @Override
         public List<Pay> getList() {
             return payMapper.selectAll();
         }
     }
     ```

   - entities

     ```java
     @Table(name = "t_pay")
     public class Pay {
         /**
          * id
          */
         @Id
         @GeneratedValue(generator = "JDBC")
         private Integer id;
     
         /**
          * 支付流水号
          */
         @Column(name = "pay_no")
         private String payNo;
     
         /**
          * 订单流水号
          */
         @Column(name = "order_no")
         private String orderNo;
     
         /**
          * 用户账号id
          */
         @Column(name = "user_id")
         private Integer userId;
     
         /**
          * 交易金额
          */
         private BigDecimal amount;
     
         /**
          * 删除标识（0不删除；1删除）
          */
         private Byte deleted;
     
         /**
          * 创建时间
          */
         @Column(name = "create_time")
         private Date createTime;
     
         /**
          * 更新时间
          */
         @Column(name = "update_time")
         private Date updateTime;
     
         /**
          * 获取id
          *
          * @return id - id
          */
         public Integer getId() {
             return id;
         }
     
         /**
          * 设置id
          *
          * @param id id
          */
         public void setId(Integer id) {
             this.id = id;
         }
     
         /**
          * 获取支付流水号
          *
          * @return payNo - 支付流水号
          */
         public String getPayNo() {
             return payNo;
         }
     
         /**
          * 设置支付流水号
          *
          * @param payNo 支付流水号
          */
         public void setPayNo(String payNo) {
             this.payNo = payNo;
         }
     
         /**
          * 获取订单流水号
          *
          * @return orderNo - 订单流水号
          */
         public String getOrderNo() {
             return orderNo;
         }
     
         /**
          * 设置订单流水号
          *
          * @param orderNo 订单流水号
          */
         public void setOrderNo(String orderNo) {
             this.orderNo = orderNo;
         }
     
         /**
          * 获取用户账号id
          *
          * @return userId - 用户账号id
          */
         public Integer getUserId() {
             return userId;
         }
     
         /**
          * 设置用户账号id
          *
          * @param userId 用户账号id
          */
         public void setUserId(Integer userId) {
             this.userId = userId;
         }
     
         /**
          * 获取交易金额
          *
          * @return amount - 交易金额
          */
         public BigDecimal getAmount() {
             return amount;
         }
     
         /**
          * 设置交易金额
          *
          * @param amount 交易金额
          */
         public void setAmount(BigDecimal amount) {
             this.amount = amount;
         }
     
         /**
          * 获取删除标识（0不删除；1删除）
          *
          * @return deleted - 删除标识（0不删除；1删除）
          */
         public Byte getDeleted() {
             return deleted;
         }
     
         /**
          * 设置删除标识（0不删除；1删除）
          *
          * @param deleted 删除标识（0不删除；1删除）
          */
         public void setDeleted(Byte deleted) {
             this.deleted = deleted;
         }
     
         /**
          * 获取创建时间
          *
          * @return createTime - 创建时间
          */
         public Date getCreateTime() {
             return createTime;
         }
     
         /**
          * 设置创建时间
          *
          * @param createTime 创建时间
          */
         public void setCreateTime(Date createTime) {
             this.createTime = createTime;
         }
     
         /**
          * 获取更新时间
          *
          * @return updateTime - 更新时间
          */
         public Date getUpdateTime() {
             return updateTime;
         }
     
         /**
          * 设置更新时间
          *
          * @param updateTime 更新时间
          */
         public void setUpdateTime(Date updateTime) {
             this.updateTime = updateTime;
         }
     }
     ```

     ```java
     @Data
     @AllArgsConstructor
     @NoArgsConstructor
     public class PayDTO implements Serializable {
         /**
          * id
          */
         private Integer id;
     
         /**
          * 支付流水号
          */
         private String payNo;
     
         /**
          * 订单流水号
          */
         private String orderNo;
     
         /**
          * 用户账号id
          */
         private Integer userId;
     
         /**
          * 交易金额
          */
         private BigDecimal amount;
     
         /**
          * 删除标识（0不删除；1删除）
          */
         private Byte deleted;
     
         /**
          * 创建时间
          */
         private Date createTime;
     
         /**
          * 更新时间
          */
         private Date updateTime;
     
     }
     ```

   - mapper

     ```java
     public interface PayMapper extends Mapper<Pay> {
     }
     ```

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.atguigu.cloud.mapper.PayMapper">
       <resultMap id="BaseResultMap" type="com.atguigu.cloud.entities.Pay">
         <!--
           WARNING - @mbg.generated
         -->
         <id column="id" jdbcType="INTEGER" property="id" />
         <result column="pay_no" jdbcType="VARCHAR" property="payNo" />
         <result column="order_no" jdbcType="VARCHAR" property="orderNo" />
         <result column="user_id" jdbcType="INTEGER" property="userId" />
         <result column="amount" jdbcType="DECIMAL" property="amount" />
         <result column="deleted" jdbcType="TINYINT" property="deleted" />
         <result column="create_time" jdbcType="TIMESTAMP" property="createTime" />
         <result column="update_time" jdbcType="TIMESTAMP" property="updateTime" />
       </resultMap>
     </mapper>
     ```

10. swagger配置

    - 所需依赖

      ```xml
      <!-- swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
      <dependency>
          <groupId>org.springdoc</groupId>
          <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
          <version>${swagger3.version}</version>
      </dependency>
      ```

    - swagger常用注解

      | 注解           | 标注位置          | 作用                   |
      | -------------- | ----------------- | ---------------------- |
      | `@Tag`         | controller类      | 标识controller作用     |
      | `@Parameter`   | 参数              | 标识参数作用           |
      | `@Parameters`  | 参数              | 参数多重说明           |
      | `@Schema`      | model层的JavaBean | 描述模型作用及每个属性 |
      | `Operation`    | 方法              | 描述方法作用           |
      | `@ApiResponse` | 方法              | 描述响应状态码等       |

    - 常用注解添加

      ```java
      @RestController
      @RequestMapping("/pay")
      @Slf4j
      @Tag(name = "支付微服务模块", description = "支付CRUD")
      public class PayController {
      
          @Resource
          private IPayService payService;
      
          @PostMapping(value = "/addPay")
          @Operation(summary = "新增", description = "新增支付流水方法，json做参数")
          public String addPay(@RequestBody @Parameter(name = "支付实体类", description = "封装用于支付的实体类") Pay pay) {
              int i = payService.add(pay);
              return i > 0 ? "支付成功" : "支付失败";
          }
      }
      ```

      ```java
      @Table(name = "t_pay")
      @Schema(name = "支付流水号实体类")
      public class Pay {
          /**
           * id
           */
          @Id
          @GeneratedValue(generator = "JDBC")
          private Integer id;
      
          /**
           * 支付流水号
           */
          @Column(name = "pay_no")
          @Schema(name = "支付流水号")
          private String payNo;
      
          /**
           * 订单流水号
           */
          @Column(name = "order_no")
          @Schema(name = "订单流水号")
          private String orderNo;
      
          /**
           * 用户账号id
           */
          @Column(name = "user_id")
          @Schema(name = "用户账号id")
          private Integer userId;
      
          /**
           * 交易金额
           */
          @Schema(name = "交易金额")
          private BigDecimal amount;
      
          /**
           * 删除标识（0不删除；1删除）
           */
          private Byte deleted;
      
          /**
           * 创建时间
           */
          @Column(name = "create_time")
          private Date createTime;
      
          /**
           * 更新时间
           */
          @Column(name = "update_time")
          private Date updateTime;
      
          /**
           * 获取id
           *
           * @return id - id
           */
          public Integer getId() {
              return id;
          }
      
          /**
           * 设置id
           *
           * @param id id
           */
          public void setId(Integer id) {
              this.id = id;
          }
      
          /**
           * 获取支付流水号
           *
           * @return payNo - 支付流水号
           */
          public String getPayNo() {
              return payNo;
          }
      
          /**
           * 设置支付流水号
           *
           * @param payNo 支付流水号
           */
          public void setPayNo(String payNo) {
              this.payNo = payNo;
          }
      
          /**
           * 获取订单流水号
           *
           * @return orderNo - 订单流水号
           */
          public String getOrderNo() {
              return orderNo;
          }
      
          /**
           * 设置订单流水号
           *
           * @param orderNo 订单流水号
           */
          public void setOrderNo(String orderNo) {
              this.orderNo = orderNo;
          }
      
          /**
           * 获取用户账号id
           *
           * @return userId - 用户账号id
           */
          public Integer getUserId() {
              return userId;
          }
      
          /**
           * 设置用户账号id
           *
           * @param userId 用户账号id
           */
          public void setUserId(Integer userId) {
              this.userId = userId;
          }
      
          /**
           * 获取交易金额
           *
           * @return amount - 交易金额
           */
          public BigDecimal getAmount() {
              return amount;
          }
      
          /**
           * 设置交易金额
           *
           * @param amount 交易金额
           */
          public void setAmount(BigDecimal amount) {
              this.amount = amount;
          }
      
          /**
           * 获取删除标识（0不删除；1删除）
           *
           * @return deleted - 删除标识（0不删除；1删除）
           */
          public Byte getDeleted() {
              return deleted;
          }
      
          /**
           * 设置删除标识（0不删除；1删除）
           *
           * @param deleted 删除标识（0不删除；1删除）
           */
          public void setDeleted(Byte deleted) {
              this.deleted = deleted;
          }
      
          /**
           * 获取创建时间
           *
           * @return createTime - 创建时间
           */
          public Date getCreateTime() {
              return createTime;
          }
      
          /**
           * 设置创建时间
           *
           * @param createTime 创建时间
           */
          public void setCreateTime(Date createTime) {
              this.createTime = createTime;
          }
      
          /**
           * 获取更新时间
           *
           * @return updateTime - 更新时间
           */
          public Date getUpdateTime() {
              return updateTime;
          }
      
          /**
           * 设置更新时间
           *
           * @param updateTime 更新时间
           */
          public void setUpdateTime(Date updateTime) {
              this.updateTime = updateTime;
          }
      }
      ```

    - swagger配置类，新建config/Swagger3Config

      ```java
      @Configuration
      public class Swagger3Config
      {
          @Bean
          public GroupedOpenApi PayApi()
          {
              return GroupedOpenApi.builder().group("支付微服务模块").pathsToMatch("/pay/**").build();
          }
          @Bean
          public GroupedOpenApi OtherApi()
          {
              return GroupedOpenApi.builder().group("其它微服务模块").pathsToMatch("/other/**", "/others").build();
          }
          /*@Bean
          public GroupedOpenApi CustomerApi()
          {
              return GroupedOpenApi.builder().group("客户微服务模块").pathsToMatch("/customer/**", "/customers").build();
          }*/
      
          @Bean
          public OpenAPI docsOpenApi()
          {
              return new OpenAPI()
                      .info(new Info().title("cloud2024")
                              .description("通用设计rest")
                              .version("v1.0"))
                      .externalDocs(new ExternalDocumentation()
                              .description("www.atguigu.com")
                              .url("https://yiyan.baidu.com/"));
          }
      }
      ```

    - 访问地址：`http://主机IP地址:模块端口号/swagger-ui/index.html`

11. 完善时间格式

    - 方法一：使用`@JsonFormat`注解

      ```java
      @JsonFormat(pattern = "yyyy-MM-dd",timezone = "GMT+8")
      @DateTimeFormat(pattern = "yyyy-MM-dd")
      // 两者选择一个即可
      private Date createTime;
      ```

    - 方法二：在application文件中指定配置

      ```yaml
      spring:
        jackson:
          date-format: yyyy-MM-dd
          time-zone: GMT+8
      ```

12. 统一返回值

    - 定义返回标准格式，三大标配

      - code状态码：由后端统一定义各种返回结果的状态码
      - message描述：本次接口调用的结果描述
      - data数据：本次返回的数据

    - 扩展，接口调用时间：timestamp

    - HTTP请求返回的状态码

      | 分类 | 区间      | 分类描述                                       |
      | ---- | --------- | ---------------------------------------------- |
      | 1**  | 100 - 199 | 信息，服务器收到请求，需要请求者继续执行操作   |
      | 2**  | 200 - 299 | 成功，操作被成功接收并处理                     |
      | 3**  | 300 - 399 | 重定向，需要进一步的操作以完成请求             |
      | 4**  | 400 - 499 | 客户端错误，请求包含语法错误或无法完成请求     |
      | 5**  | 500 - 599 | 服务器错误，服务器在处理请求的过程中发生了错误 |

    - 状态码枚举类

      ```java
      @Getter
      public enum ReturnCodeEnum {
      
          // 定义枚举类小口诀
          // 举值 - 构造 - 遍历
      
          // 举值
          RC999("999", "操作xxx失败"),
      
          RC200("200", "操作成功"),
      
          RC201("201", "服务开启降级保护，请稍后再试！"),
      
          RC202("202", "热点参数限流，请稍后再试！"),
      
          RC203("203", "系统规则不满足要求，请稍后再试！"),
      
          RC204("204", "授权规则不通过，请稍后再试！"),
      
          RC401("401", "匿名用户访问无权限资源时的异常"),
      
          RC403("403", "无访问权限，请联系管理员授予权限"),
      
          RC404("404", "404页面找不到的异常"),
      
          RC500("500", "系统异常，请稍后重试"),
      
          RC375("375", "数学运算异常，请稍后重试"),
      
          INVALID_TOKEN("2001", "访问令牌不合法"),
      
          ACCESS_DENIED("2003", "没有权限访问该资源");
      
          // 构造
          // 自定义状态码
          private final String code;
      
          // 自定义描述信息
          private final String msg;
      
          ReturnCodeEnum(String code, String msg) {
              this.code = code;
              this.msg = msg;
          }
      
          // 遍历
          // 传统版本
          public static ReturnCodeEnum getReturnCodeEnumByFor(String code) {
              for (ReturnCodeEnum element : ReturnCodeEnum.values()) {
                  if (element.getCode().equalsIgnoreCase(code)) {
                      return element;
                  }
              }
              return null;
          }
      
          // stream流式计算版本
          public static ReturnCodeEnum getReturnCodeEnumByStream(String code) {
              return Arrays.stream(ReturnCodeEnum.values()).filter(element -> element.getCode().equalsIgnoreCase(code)).findFirst().orElse(null);
          }
      }
      ```

    - 创建统一定义返回对象ResultData

      ```java
      @Data
      @Accessors(chain = true)
      // 开启链式编程   设置chain=true时，setter方法返回的是this（也就是对象自己），
      // 代替了默认的返回值void，直接链式操作对象
      public class ResultData<T> {
      
          private String code;
      
          private String msg;
      
          private T data;
      
          private long timestamp;
      
          public ResultData() {
              this.timestamp = System.currentTimeMillis();
          }
      
          public static <T> ResultData<T> success(T data){
              ResultData resultData = new ResultData();
         resultData.setCode(ReturnCodeEnum.RC200.getCode()).setMsg(ReturnCodeEnum.RC200.getMsg()).setData(data);
              return resultData;
          }
      
          public static <T> ResultData<T> fail(String code,String msg){
              ResultData resultData = new ResultData();
              resultData.setCode(code).setMsg(msg).setData(null);
              return resultData;
          }
      }
      
      ```

    - 完善过后的controller类

      ```java
      @RestController
      @RequestMapping("/pay")
      @Slf4j
      @Tag(name = "支付微服务模块", description = "支付CRUD")
      public class PayController {
      
          @Resource
          private IPayService payService;
      
          @PostMapping(value = "/addPay")
          @Operation(summary = "新增", description = "新增支付流水方法，json做参数")
          public ResultData<String> addPay(@RequestBody @Parameter(name = "支付实体类", description = "封装用于支付的实体类") Pay pay) {
              int i = payService.add(pay);
              String msg = i > 0 ? "支付成功" : "支付失败";
              return ResultData.success(msg);
          }
      
          @DeleteMapping(value = "/deletePay/{id}")
          public ResultData<String> delete(@PathVariable("id") Integer id) {
              int i = payService.delete(id);
              String msg = i > 0 ? "支付删除成功" : "支付删除失败";
              return ResultData.success(msg);
          }
      
          @PutMapping(value = "/updatePay")
          public ResultData<String> update(@RequestBody PayDTO payDTO) {
              Pay pay = new Pay();
              BeanUtil.copyProperties(payDTO, pay);
              int i = payService.update(pay);
              String msg = i > 0 ? "支付修改成功" : "支付修改失败";
              return ResultData.success(msg);
          }
      
          @GetMapping(value = "/getInfoById/{id}")
          public ResultData<PayDTO> getInfoById(@PathVariable("id") Integer id) {
              Pay pay = payService.getInfoById(id);
              PayDTO payDTO = new PayDTO();
              BeanUtil.copyProperties(pay, payDTO);
              return ResultData.success(payDTO);
          }
      
          @GetMapping(value = "/getList")
          public ResultData<List<PayDTO>> getList() {
              List<Pay> payList = payService.getList();
              List<PayDTO> payDTOList = new ArrayList<>();
              for (Pay pay : payList) {
                  PayDTO payDTO = new PayDTO();
                  BeanUtil.copyProperties(pay,payDTO);
                  payDTOList.add(payDTO);
              }
              return ResultData.success(payDTOList);
          }
      }
      ```

13. 全局异常接入返回的标准格式

    - 全局异常处理器的好处：不用再手写`try...catch`，当前二者并无冲突

    - 创建全局异常类

      ```java
      @Slf4j
      @RestControllerAdvice
      public class GlobalExceptionHandler {
      
          @ExceptionHandler(RuntimeException.class)
          @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
          public ResultData<String> exception(Exception e){
              log.info("全局异常信息：{}",e.getMessage(),e);
              return ResultData.fail(ReturnCodeEnum.RC500.getCode(), e.getMessage());
          }
      }
      ```

    - `@RestControllerAdvice`的作用

      - 当我们在类上使用`@RestControllerAdvice`注解时，它相当于同时使用了`@ControllerAdvice`和`@ResponseBody`。这意味着被`@RestControllerAdvice`注解标记的类将被视为全局异常处理器，并且异常处理方法的返回值将以JSON格式直接写入响应体中
      - 通过在`@RestControllerAdvice`类中定义异常处理方法，我们可以捕获和处理控制器中抛出的异常，提供自定义的异常处理逻辑，以及返回适当的响应给客户端。这样可以统一处理应用程序中的异常情况，提高代码的可维护性和可读性

## 四、微服务消费者消费工程

1. 搭建cloud-consumer-order80工程

2. 引入pom文件依赖

   ```xml
   <dependencies>
           <!--web + actuator-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--lombok-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <!--hutool-all-->
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
           </dependency>
           <!--fastjson2-->
           <dependency>
               <groupId>com.alibaba.fastjson2</groupId>
               <artifactId>fastjson2</artifactId>
           </dependency>
           <!-- swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
           <dependency>
               <groupId>org.springdoc</groupId>
               <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   ```

3. 编写yml文件`application.yml`

   ```yaml
   server:
     port: 80
   ```

4. 配置RestTemplate

   - RestTemplate的作用：RestTemplate提供了多种便捷访问远程http服务的方法；RestTemplate是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的客户端模板工具类

   - RestTemplate的使用时常用的三个参数

     - url：REST请求地址
     - requestMap：请求参数
     - ResponseBean.class：http响应转换被转换成的对象类型

   - RestTemplate常用的方法

     - *xxxForObject`：返回对象为响应体中数据转化成的对象，基本上可以理解为JSON
     - `xxxForEntity`：返回对象为ResponseEntity对象，包含了响应中的一些重要信息，比如响应头、响应状态码、响应体等

   - 创建RestTemplate的配置类

     ```java
     @Configuration
     public class RstTemplateConfig {
     
         @Bean
         public RestTemplate restTemplate(){
             return new RestTemplate();
         }
     }
     ```

   - 消费者CRUD

     ```javascript
     @RestController
     @RequestMapping("/consumer")
     public class OrderController {
     
         public static final String PAY_SERVICE_URL = "http://localhost:8001/pay";
     
         @Resource
         private RestTemplate restTemplate;
     
         @PostMapping(value = "/addPay")
         public ResultData addOrder(@RequestBody PayDTO payDTO) {
             return restTemplate.postForObject(PAY_SERVICE_URL + "/addPay", payDTO, ResultData.class);
         }
     
         @PutMapping(value = "/updatePay")
         public ResultData updateOrder(@RequestBody PayDTO payDTO) {
             restTemplate.put(PAY_SERVICE_URL + "/updatePay", payDTO);
             return ResultData.success("修改成功");
         }
     
         @DeleteMapping(value = "/deletePay/{id}")
         public ResultData deletePay(@PathVariable("id") Integer id) {
             restTemplate.delete(PAY_SERVICE_URL + "/deletePay/" + id);
             return ResultData.success("删除成功");
         }
     
         @GetMapping(value = "/getInfoById/{id}")
         public ResultData getInfoById(@PathVariable("id") Integer id) {
             return restTemplate.getForObject(PAY_SERVICE_URL + "/getInfoById/" + id, ResultData.class, id);
         }
     
         @GetMapping(value = "/getList")
         public ResultData getList() {
             return restTemplate.getForObject(PAY_SERVICE_URL + "/getList", ResultData.class);
         }
     }
     
     ```

## 五、搭建公共工程

1. 搭建普通的cloud-api-commons工程，并不需要启动类

2. 引入依赖

   ```xml
   <dependencies>
           <!--web + actuator-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <!--hutool-->
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   ```

3. 提取共用类`PayDTO`、`GlobalExceptionHandler`、`ResultData`、`ReturnCodeEnum`，并将原来共同的包删除掉

4. 使用maven命令`mvn clean install`将cloud-api-commons工程打包

5. 在其他模块引入cloud-api-commons的jar包

   ```xml
   <!-- 引入自己定义的api通用包 -->
   <dependency>
       <groupId>com.atguigu.cloud</groupId>
       <artifactId>cloud-api-commons</artifactId>
       <version>1.0-SNAPSHOT</version>
   </dependency>
   ```

# 三、Consul服务注册与发现

## 一、Consul简介

1. [Consul官网](https://www.consul.io/)
2. Consul的定位：Consul是一个服务网络解决方案，它使团队能够管理服务之间以及跨本地和多云环境和运行时的安全网络连接。Consul提供服务发现、服务网格、流量管理和对网络基础设施设备的自动更新。可以单独使用这些功能，也可以在单个Consul部署中一起使用。Consul是一套开源的分布式服务发现和配置管理系统，由Go语言开发
3. Consul的作用
   - 服务发现：提供HTTP和DNS两种发现方式
   - 健康监测：支持多种方式，HTPP、TCP、Docker、Shell脚本定制等
   - KV存储：key、value的存储方式
   - 多数据中心：支持多数据中心
   - 可视化Web界面

## 二、安装并运行Consul

1. [Consul官网下载路径](https://developer.hashicorp.com/consul/install?product_intent=consul)
2. 解压后文件中仅有`consul.exe`文件，在`cmd`的切换到解压后的路径执行`consul -version`显示版本号即下载安装成功
3. 启动命令为`consul agent -dev`
4. 访问地址为`http://localhost:8500/`。在页面Services目录中显示consul为绿标即为启动正常

## 三、微服务服务注册Consul

1. 消费者和支付提供者两个工程中引入依赖

   ```xml
   <!--SpringCloud consul discovery -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-consul-discovery</artifactId>
       <exclusions>
           <exclusion>
               <groupId>commons-logging</groupId>
               <artifactId>commons-logging</artifactId>
           </exclusion>
       </exclusions>
   </dependency>
   ```

2. 消费者和支付提供者两个工程中更改`application.yaml`文件中的配置信息

   ```yaml
   spring:
     application:
       name: cloud-consumer-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
           
           
   spring:
     application:
       name: cloud-payment-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
   ```

3. 消费者和支付提供者两个工程的主启动类中添加`@EnableDiscoveryClient`即可开启服务发现

4. 在Consul页面中显示两个微服务绿标即为服务发现成功

5. 修改`OrderController`中主机调用地址

   ```java
   //    public static final String PAY_SERVICE_URL = "http://localhost:8001/pay";
   public static final String PAY_SERVICE_URL = "http://cloud-payment-service/pay";
   ```

6. 由于Consul默认支持负载均衡，微服务是集群的，所以需要在RestTemplate的配置类中添加`@LoadBalanced`以支持负载均衡

7. 正常访问消费支付等url地址即可

## 四、CAP理论

1. CAP概念

   - C，Consistency，即强一致性
   - A，Availability，即可用性
   - P，Partition tolerance，即分区容错性

2. 经典CAP

   - 最多只能同时满足较好的两个
   - CAP理论的核心是，一个分布式系统不可能同时很好的满足一致性、可用性和分区容错性。因此，根据CAP原理将NoSQL数据库分成了满足CA原则、满足CP原则和满足AP原则三大类
     - CA，即单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。当网络分区出现后，为了保证一致性，就必须
     - CP，即满足一致性，分区容错性的系统，通常性能不是特别高。当网络分区出现后，为了保证系统A与系统B 一致性，就必须拒绝请求，否则无法保证一致性，此时系统会返回错误码或错误信息。Consul遵循CP原则，保证了强一致性和分区容错性，且使用的是Raft算法，比zookeeper使用的Paxos算法更加简单。虽然保证了强一致性，但是可用性就相应下降了，例如服务注册的时间会稍微长一点，例如服务注册的时间会稍微长一些，因为Consul的Raft协议要求必须过半数的节点都写入成功才认为注册成功；在leader挂掉之后，重新选举出leader之前会导致Consul服务不可用
     - AP，即满足可用性，分区容错性的系统，通常可能对一致性要求低一些。如Eureka组件有系统A和系统B，当网络分区出现后，为了保证可用性，系统B可以返回旧值，保证系统的可用性。当数据出现不一致时，虽然A、B上的注册信息不完全相同，但每个Eureka节点依然能够正常对外提供服务，这会出现查询服务信息时如果请求A查不到，但请求B就能查到，即返回旧值。如此保证了可用性但牺牲了一致性结论，违背了一致性C的要求

3. 主流注册中心异同点

   | 组件名    | 语言 | CAP  | 服务健康检查 | 对外暴露接口 | SpringCloud集成与否 |
   | --------- | ---- | ---- | ------------ | ------------ | ------------------- |
   | Eureka    | Java | AP   | 可配支持     | HTTP         | 是                  |
   | Consul    | Go   | CP   | 支持         | HTTP/DNS     | 是                  |
   | Zookeeper | Java | CP   | 支持         | 客户端       | 是                  |

## 五、微服务服务配置Consul

1. 需求：通用全局配置信息，直接注册进Consul服务器

2. 消费者和支付提供者两个工程中引入依赖

   ```xml
   <!--SpringCloud consul config -->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-consul-config</artifactId>
   </dependency>
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-bootstrap</artifactId>
   </dependency>
   ```

3. `bootstrap.yaml`和`application.yaml`的联系与区别：

   - `application.yaml`是用户级别的资源配置项
   - `bootstrap.yaml`是系统级的，优先级更高
   - `Bootstrap`属性有高优先级，默认情况下不会被本地配置覆盖。`Bootstrap context`和`Application context`有些不同的约定，所以新增一个`bootstrap.yaml`文件，保证`Bootstrap context`和`Application context`配置的分离
   - 一般情况下两种配置文件共存即可

4. 更改后`bootstrap.yaml`和`application.yaml`的内容

   - `application.yaml`

   ```yaml
   #cloud-payment-service
   server:
     port: 8001
   
   spring:
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:13306/db2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
       username: root
       password: 282127
     # 多环境配置加载内容dev/prod,不写就是默认default配置
     profiles:
       active: default
     jackson:
       date-format: yyyy-MM-dd
       time-zone: GMT+8
   
   mybatis:
     mapper-locations: classpath:mapper/*.xml
     type-aliases-package: com.atguigu.cloud.entities
     configuration:
       map-underscore-to-camel-case: true
   
   
   # cloud-consumer-service
   server:
     port: 80
   ```

   - `bootstrap.yaml`

   ```yaml
   spring:
     application:
       name: cloud-payment-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
         config:
           profile-separator: '-'
           format: yaml
           
           
   spring:
     application:
       name: cloud-consumer-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
         config:
           profile-separator: '-'
           format: yaml
   ```

5. Consul服务器key/value配置填写

   - 在Consul页面key/value目录下创建`config/`文件夹，必须是这个名称

   - 在`config/`文件夹下创建`cloud-payment-service/`、`cloud-payment-service-dev/`、`cloud-payment-service-prod/`

   - `cloud-payment-service/`、`cloud-payment-service-dev/`、`cloud-payment-service-prod/`三个文件夹下分别创建data文件，data不再是文件夹

   - data的内容暂时写入以下内容

     ```yaml
     atguigu:
      info: welcome to my prod world
     # 此处不能使用Tab，使用一个空格即可
     ```

   - 获取Consul内容

     ```java
      @GetMapping(value = "/getInfoByConsul")
      public String getInfoByConsul(@Value("${atguigu.info}") String info) {
      	return port + "：info" + info;
      }
     ```

   - 当改变`application.yaml`中的`profiles.active`环境时，会自动根据`profiles.active`读取Consul中`cloud-payment-service/`、`cloud-payment-service-dev/`、`cloud-payment-service-prod/`相匹配的文件夹下的data内容

## 六、微服务动态刷新Consul

1. 需求：在Consul的key/value目录中修改了内容，能让各个服务都能立马刷新得到新的内容

2. 在主启动类中添加`@RefreshScope`实现动态刷新，如果不添加此注解，在修改完key/value的内容之后也会默认在55秒后进行刷新

   - `@RefreshScope`将这个注解加在需要动态刷新的Bean上，当一个Bean被`@RefreshScope`注解后，Spring容器会在监测到配置变动时重新实例化该Bean，并且注入最新的配置信息，这样就实现了配置的热更新 
   - 通常情况下，这个Bean还有个属性被`@Value`注解了，这样，当配置发生变化时，`@Value`注解会重新注入最新的配置值
   - 对于使用了`@Value`注解的类，不需要显式添加`@RefreshScope`注解，因为被`@Value`注解的属性值会在配置发生变化时自动更新，而对于其它没有使用`@Value`注解的类，比如只使用了`@Autowired`或者`@Resource`，那么就需要添加`@RefreshScope`注解 

3. 在`application.yaml`配置类中添加`wait-time`，最好不修改

   ```yaml
   spring:
     application:
       name: cloud-payment-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
         config:
           profile-separator: '-'
           format: yaml
           watch:
             wait-time: 10
   ```

## 七、Consul持久化

1. 在Consul解压文件下创建mydata文件夹

2. 在Consul解压文件下创建consul_start.bat，后缀名为.bat

   ```shell
   @echo.服务启动......  
   @echo off  
   @sc create Consul binpath= "D:\Program Files\consul-1.17\consul.exe agent -server -ui -bind=127.0.0.1 -client=0.0.0.0 -bootstrap-expect  1  -data-dir D:\Program Files\consul-1.17\mydata"
   @net start Consul
   @sc config Consul start= AUTO  
   @echo.Consul start is OK......success
   @pause
   ```

3. 右击选择管理员运行此consul_start.bat文件即可实现自启动

# 四、LoadBalancer负载均衡服务调用

## 一、负载均衡概述

1. LB，即负载均衡（Load Balancer），是高并发、高可用系统必不可少的关键组件，其目标是尽力将网络流量平均分发到多个服务器上，以提高系统整体的响应速度和可用性 
2. SpringCloud Balancer是由SpringCloud官方提供的一个开源的、简单易用的客户端负载均衡，它包含在SpringCloud-commons中用它来替代以前的Ribbon。相对于Ribbon，SpringCloud Balancer不仅能够支持RestTemplate，还支持WebClient（WeClient是Spring Web Flux中提供的功能，可以实现响应式异步请求）
3. 客户端负载均衡VS服务端负载均衡
   - Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求，即负载均衡是由服务端实现的。服务器充当指挥员，在请求过来的时候进行分配
   - loadbalancer本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用技术。客户端自己有眼力价，自己看着哪个服务器不忙就去找哪个
4. 负载均衡算法分类
   - 轮询：将请求按顺序轮流地分配到每个节点上，不关心每个节点实际的连接数和当前的系统负载。优点是简单高效，易于水平扩展，每个节点满足字面意义上的均衡；缺点是没有考虑机器的性能问题，集群性能瓶颈更多的会受性能差的服务器影响。轮询的算法公式为rest接口第几次请求数%服务器集群总数量 = 实际调用服务器位置下标，每次服务重启后rest接口计数从1开始
   - 随机：将请求随机分配到各个节点。由概率统计理论得知，随着客户端调用服务端的次数增多，其实际效果越来越接近于平均分配，也就是轮询的结果 
   - 最小连接数法：根据每个节点当前的连接情况，动态地选取其中当前积压连接数最少的一个节点处理当前请求，尽可能地提高后端服务的利用效率，将请求合理地分流到每一台服务器。优点是动态，根据节点状况实时变化； 缺点是 提高了复杂度，每次连接断开需要进行计数
   - 最快响应速度法：根据请求的响应时间，来动态调整每个节点的权重，将响应速度快的服务节点分配更多的请求，响应速度慢的服务节点分配更少的请求，俗称能者多劳，扶贫救弱。优点是动态，实时变化，控制的粒度更细，更灵敏；缺点是复杂度更高，每次需要计算请求的响应速度  
   - 观察模式法：观察者模式是综合了最小连接数和最快响应度，同时考量这两个指标数，进行一个权重的分配  
   - 源地址哈希：根据客户端的IP地址，通过哈希计算得到一个数值，用该数值对服务器节点数进行取模，得到的结果便是要访问节点序号。采用源地址哈希法进行负载均衡，同一IP地址的客户端，当后端服务器列表不变时，它每次都会落到到同一台服务器进行访问。优点是相同的IP每次落在同一个节点，可以人为干预客户端请求方向；缺点是如果某个节点出现故障，会导致这个节点上的客户端无法使用，无法保证高可用。当某一用户成为热点用户，那么会有巨大的流量涌向这个节点，导致冷热分布不均衡，无法有效利用起集群的性能。所以当热点事件出现时，一般会将源地址哈希法切换成轮询法   
   - 一致性哈希：主要的特点就是Hash环，我们的请求可以构建成一个Hash环，按照顺时针记录hash和请求。当我们的服务挂了A时，我们只需要将A的请求交给A后面的B处理；当我们需要增加服务器C时，我们只需要在Hash环上划一块范围，然后交给C；这样就可以实现动态的扩容和缩容。一致性哈希用于解决分布式缓存系统中的节点选择和在增删服务器后，节点减少带来的数据缓存的消失与重新分配问题

## 二、LoadBalancer工作流程

1. 先选择ConsulServer从服务端查询并拉取服务列表，知道了它有多少个服务（每个服务的实现是完全一样的），默认轮询调用谁都可以正常执行。类似生活中的求医挂号，某个科室今日出诊的全部医生，客户端自己选一个
2. 按照指定的负载均衡策略从server取到的服务注册列表中由客户端自己选择一个地址。所以LoadBalancer是一个客户端的负载均衡器

## 三、LoadBalancer的实现

1. LoadBalancer只是相当于一个接口，实现它的方式有多种
   - RestTemplate
   - RestClient
   - WebClient
   - WebFlux WebClient with ReactorLoadBalancerExchangeFilterFunction
   
2. 完全将cloud-provider-payment8001复制一遍，新搭建一个cloud-provider-payment8002工程

3. 在消费者客户端工程中引入依赖

   ```xml
   <dependency>
   	<groupId>org.springframework.cloud</groupId>
   	<artifactId>spring-cloud-starter-loadbalancer</artifactId>
   </dependency>
   ```

4. 在消费者客户端工程`RestTemplateConfig`配置类中添加`@LoadBalanced`注解

   ```java
   @Configuration
   public class RestTemplateConfig {
   
       @Bean
       @LoadBalanced
       public RestTemplate restTemplate(){
           return new RestTemplate();
       }
   }
   ```

5. 在消费者客户端工程的controller包中添加如下方法

   ```java
   @GetMapping(value = "/getInfoByConsul")
   public String getInfoByConsul() {
   	return restTemplate.getForObject(PAY_SERVICE_URL + "/getInfoByConsul", String.class);
   }
   ```

6. 访问此地址的时间即可在采用轮询的方式进行服务的调用

7. 修改LoadBalancer的负载均衡算法的实现方式

   ```java
   @Configuration
   @LoadBalancerClient(value = "cloud-payment-service", configuration = RestTemplateConfig.class)
   // value指出具体的微服务采用非默认的负载均衡算法
   public class RestTemplateConfig {
   
       @Bean
       @LoadBalanced
       public RestTemplate restTemplate() {
           return new RestTemplate();
       }
   
       @Bean
       public ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(Environment environment,
        LoadBalancerClientFactory loadBalancerClientFactory) {
           String name = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);
           return new RandomLoadBalancer(loadBalancerClientFactory.getLazyProvider(name, ServiceInstanceListSupplier.class), name);
       }
   }
   ```

# 五、OpenFeign服务接口调用

## 一、OpenFeign简介

1. Feign是一个声明性Web服务客户端，它使用编写Web服务客户端变得更容易。使用Feign创建一个接口并对其进行注释。在使用Feign时提供负载均衡的http客户端。简而言之，使用时只需要创建一个Rest接口并在该接口上添加`@FeignClient`即可。OpenFeign基本上就是当前微服务之间调用的事实标准
2. OpenFeign的作用
   - 可插拔的注解支持，包括Feign注解和JAX-RS注解
   - 支持可插拔的HTTP编码器和解码器
   - 支持Sentinel和它的Fallback，用于降级
   - 支持SpringCloud LoadBalancer的负载均衡
   - 支持HTTP请求和相应的压缩
   - OpenFeign是一种声明式、模板化的HTTP客户端，它使得调用RESTful网络服务变得简单。在Spring Cloud中使用OpenFeign，可以做到像调用本地方法一样使用HTTP请求访问远程服务，开发者无需关注远程HTTP请求的细节 
   - OpenFeign的Spring应用架构一般分为三部分，分别是注册中心、服务提供者和服务消费者。服务提供者向服务注册中心注册自己，然后服务消费者通过OpenFeign发送请求时，OpenFeign会向服务注册中心获取关于服务提供者的信息，然后再向服务提供者发送网络请求 

## 二、OpenFeign的使用

![image-20240520064333407](../../../TyporaImage/image-20240520064333407.png)

1. 搭建cloud-consumer-feign-order80工程

2. 在cloud-consumer-feign-order80工程中引入依赖

   ```xml
   <dependencies>
           <!--web + actuator-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--lombok-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <!--hutool-all-->
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
           </dependency>
           <!--fastjson2-->
           <dependency>
               <groupId>com.alibaba.fastjson2</groupId>
               <artifactId>fastjson2</artifactId>
           </dependency>
           <!-- swagger3 调用方式 http://你的主机IP地址:5555/swagger-ui/index.html -->
           <dependency>
               <groupId>org.springdoc</groupId>
               <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
           </dependency>
           <!-- 引入自己定义的api通用包 -->
           <dependency>
               <groupId>com.atguigu.cloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <!--SpringCloud consul discovery -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-consul-discovery</artifactId>
               <exclusions>
                   <exclusion>
                       <groupId>commons-logging</groupId>
                       <artifactId>commons-logging</artifactId>
                   </exclusion>
               </exclusions>
           </dependency>
           <!--SpringCloud consul config -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-consul-config</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-bootstrap</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-openfeign</artifactId>
           </dependency>
       </dependencies>
   
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   ```

3. cloud-consumer-feign-order80工程中`bootstrap.yml`文件中配置如下

   ```yaml
   spring:
     application:
       name: cloud-consumer-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
           prefer-ip-address: true #优先使用服务ip进行注册
         config:
           profile-separator: '-'
           format: yaml
     # 如果启动报错，则加上以下代码解决The bean ‘xxxx.FeignClientSpecification‘ could not be registered   # 的问题。allow-bean-definition-overriding用于允许覆盖已经定义的bean定义
     main:
       allow-bean-definition-overriding: true
   ```

4. cloud-consumer-feign-order80工程启动类中添加注解并将cloud-consumer-order80的代码复制到cloud-consumer-feign-order80中并将RestTemplate配置类删除

   ```java
   @SpringBootApplication
   @EnableFeignClients
   // 启动feign客户端，定义服务+绑定接口，以声明式的方法优雅而简单的实现服务调用
   @EnableDiscoveryClient
   // 该注解用于向使用consul为注册中心时注册服务
   public class OpenFeignMain80 {
   
       public static void main(String[] args) {
           SpringApplication.run(OpenFeignMain80.class,args);
       }
   }
   ```

5. cloud-consumer-feign-order80工程中修改OrderController

   ```java
   @RestController
   @RequestMapping("/feign/pay")
   public class OrderController {
   
       @Resource
       private PayFeignApi payFeignApi;
   
       @PostMapping(value = "/addPay")
       public Result<Integer> addPay(@RequestBody PayDTO payDTO) {
           return payFeignApi.addPay(payDTO);
       }
   
       @DeleteMapping(value = "/deletePay/{id}")
       public Result<Integer> deletePay(@PathVariable("id") Integer id) {
           return payFeignApi.deletePay(id);
       }
   
       @PutMapping(value = "/updatePay")
       public Result<Integer> updatePay(@RequestBody PayDTO payDTO) {
           return payFeignApi.updatePay(payDTO);
       }
   
       @GetMapping(value = "/getInfoById/{id}")
       public Result<PayDTO> getInfoById(@PathVariable("id") Integer id) {
           return payFeignApi.getById(id);
       }
   
       @GetMapping(value = "/getList")
       public Result<List<PayDTO>> getList() {
           return payFeignApi.getAll();
       }
   
       @GetMapping(value = "/getInfoByConsul")
       public Result<String> getInfoByConsul() {
           return payFeignApi.getInfoByConsul();
       }
   }
   ```

6. 修改cloud-api-commons工程，将Feign接口定义在此。引入OpenFeign依赖

   ```xml
   <dependency>
   	<groupId>org.springframework.cloud</groupId>
   	<artifactId>spring-cloud-starter-openfeign</artifactId>
   </dependency>
   ```

7. cloud-api-commons工程中新建apis文件夹存放各个微服务的api接口，以支付接口为例

   ```java
   @FeignClient(value = "cloud-payment-service")
   // @RequestMapping("/pay")
   // 千万不要在interface上写@RequestMapping
   public interface PayFeignApi {
   
       @PostMapping(value = "/pay/addPay")
       public ResultData<String> addPay(@RequestBody PayDTO pay);
   
       @DeleteMapping(value = "/pay/deletePay/{id}")
       public ResultData<String> deletePay(@PathVariable("id") Integer id);
   
       @PutMapping(value = "/pay/updatePay")
       public ResultData<String> updatePay(@RequestBody PayDTO payDTO);
   
       @GetMapping(value = "/pay/getInfoById/{id}")
       public ResultData<PayDTO> getInfoById(@PathVariable("id") Integer id);
   
       @GetMapping(value = "/pay/getList")
       public ResultData<List<PayDTO>> getList();
   
       @GetMapping(value = "/pay/getInfoByConsul")
       public String getInfoByConsul();
   }
   ```

9. 测试时不需要启动原来的cloud-consumer-order80工程。即使不启动cloud-api-commons工程也会调用到接口，因为 类似于以前Dao接口上标注mapper注解，现在是一个微服务接口上面标注一个Feign注解即可对服务提供方的接口绑定， 如果对某个接口定义了`@FeignClient`注解，Feign就会针对这个接口创建一个动态代理， 要是调用那个接口，本质就是会调用Feign创建的动态代理，Feign的动态代理会根据你在接口上的`@RequestMapping`等注解，来动态构造出你要请求的服务的地址，然后构建请求，构造参数，发送请求，接受请求，解析返回值等

## 三、OpenFeign的高级特性

### 一、OpenFeign之超时控制

1. OpenFeign默认等待时间是60秒钟，超过后会报错

2. ymal文件中开启配置：`connectTimeout`即连接超时时间和`readTimeout`即请求处理超时时间

   - 全局配置

     ```yaml
     spring:
       application:
         name: cloud-consumer-feign-service
       cloud:
         consul:
           host: localhost
           port: 8500
           discovery:
             service-name: ${spring.application.name}
             prefer-ip-address: true #优先使用服务ip进行注册
           config:
             profile-separator: '-'
             format: yaml
         openfeign:
           client:
             config:
               #设置的全局超时时间，指定服务名称可以设置单个服务的超时时间
               #为cloud-payment-service设置全局变量，则把default改为cloud-payment-service即可
               default:
               	#连接超时时间
                 connect-timeout: 20000
                 #读取超时时间
                 read-timeout: 20000
       main:
         allow-bean-definition-overriding: true
     ```

   - 指定配置

     ```yaml
     spring:
       application:
         name: cloud-consumer-feign-service
       cloud:
         consul:
           host: localhost
           port: 8500
           discovery:
             service-name: ${spring.application.name}
             prefer-ip-address: true #优先使用服务ip进行注册
           config:
             profile-separator: '-'
             format: yaml
         openfeign:
           client:
             config:
               #设置的全局超时时间，指定服务名称可以设置单个服务的超时时间
               #为cloud-payment-service设置全局变量，则把default改为cloud-payment-service即可
               cloud-payment-service:
               	#连接超时时间
                 connect-timeout: 20000
                 #读取超时时间
                 read-timeout: 20000
       main:
         allow-bean-definition-overriding: true
     ```

   - 当全局配置和指定配置都存在时，当调用指定配置的服务时，指定配置启作用

### 二、OpenFeign之重试机制

1. 默认情况下会创建Retryer.NEVER_RETRY类型为Retryer的Bean，这将金庸重试。注意点是这种重试行为与Feign默认行为不同，它会自动重试IOExceptions，将它们视为与网络相关的瞬态异常，以及从ErrorDecoder抛出的任何RetryableException

2. cloud-consumer-feign-order80工程中新增FeignConfig配置类

   ```java
   @Configuration
   public class FeignConfig {
   
       @Bean
       public Retryer myRetryer(){
           // return new Retryer.NEVER_RETRY则是默认配置不走重试机制的
           // 最大请求次数为3(1+2) -> 初始一次，重试两次，初始间隔时间为100ms，重试间最大间隔时间为1s
           return new Retryer.Default(100,1,3);
       }
   }
   ```

3. 重试机制和超时控制机制配置使用时，如超时控制设置的时间为4s，重试机制的最大请求次数为3次，那么如果报错会等12秒之后出现报错信息（因为初始间隔时间为100ms，可以忽略不算，如果初始间隔时间设置的大则也要计算进去）

### 三、OpenFeign之HttpClient

1. OpenFeign如果不做特殊配置，OpenFeign默认使用的是JDK自带的HttpURLConnection发送HTTP请求。由于默认HttpURLConnection没有连接池，性能和效率比较低，如果采用默认，性能上不是最好的

2. 使用Apache HttpClient5替换OpenFeign默认的HttpURLConnection，**一定要替换，提供系统效率**

3. 在cloud-consumer-feign-order80工程中引入依赖

   ```xml
   <!-- httpclient5 -->
   <dependency>
   	<groupId>org.apache.httpcomponents.client5</groupId>
   	<artifactId>httpclient5</artifactId>
   	<version>5.3</version>
   </dependency>
   <!-- httpclient5 -->
   <dependency>
   	<groupId>io.github.openfeign</groupId>
   	<artifactId>feign-hc5</artifactId>
   	<version>13.1</version>
   </dependency>
   ```

4. ymal文件中开启配置：

   ```yaml
   spring:
     application:
       name: cloud-consumer-feign-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
           prefer-ip-address: true #优先使用服务ip进行注册
         config:
           profile-separator: '-'
           format: yaml
       openfeign:
         client:
           config:
             default:
               connect-timeout: 20000
               read-timeout: 20000
         httpclient: 
         #httpclient的超时时间配置用client中的connect-timeout和read-timeout即可
           hc5:
             enabled: true
     main:
       allow-bean-definition-overriding: true
   ```

### 四、OpenFeign之请求/相应压缩

1. OpenFeign之请求/相应压缩介绍：Spring Cloud OpenFeign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗

2. 配置文件中添如下配置，就能开启请求与相应的压缩功能

   ```properties
   spring.cloud.openfeign.compression.request.enabled = true
   spring.cloud.openfeign.compression.response.enabled = true
   ```

3. 细粒化设置：对请求压缩做一些更细致的设置，比如指定压缩的请求参数类型并设置请求压缩的大小下线，只有超过这个大小的请求才会进行压缩

   ```properties
   spring.cloud.openfeign.compression.request.enabled = true
   # 最小触发压缩的大小
   spring.cloud.openfeign.compression.response.min-request-size = 2028
   # 触发压缩数据类型
   spring.cloud.openfeign.compression.response.mime-types = text/xml,application/xml,application/json
   ```


### 五、OpenFeign之日志

1. OpenFeign日志可以了解Feign中Http请求的细节，可以通过配置来调整日志等级，简而言之就是对Feign接口的调用情况进行监控和输出

2. OpenFeign日志分类

	- NONE：默认的，不显示任何日志
	- BASIC：仅记录请求方法、URL、响应状态码以及执行时间
	- HEADERS：记录请求方法、URL、响应状态码、执行时间以及请求和响应的头信息
	- FULL：除了记录请求方法、URL、响应状态码、执行时间以及请求和响应的头信息，还记录请求和响应的正文及元数据

3. cloud-consumer-feign-order80工程FeignConfig配置类中配置日志Bean

	```java
	@Configuration
	public class FeignConfig {
	
	    // 重试机制
	    @Bean
	    public Retryer myRetryer() {
	        return new Retryer.Default();
	        // 初次间隔 最大间隔 最大请求次数(1+2) = 3
	//        return new Retryer.Default(100, 1, 3);
	    }
	
	    // 日志记录级别
	    @Bean
	    public Logger.Level feignLoggerLevel() {
	        return Logger.Level.FULL;
	    }
	}
	```

4. cloud-consumer-feign-order80工程yaml文件中添加Feign等级日志

	- 公式为：logging.level + 含有@FeignClient注解的完整带包名的接口名 + debug
	- 如果没有在配置文件中配置日志，则只会打印简单的日志，例如调用接口开始时间和结束时间

	```yaml
	#和Spring是等级的
	logging:
	  level:
	    com:
	      atguigu:
	      	cloud:
	        	apis:
	          		PayFeignApi: debug
	```

# 六、CircuitBreaker断路器

## 一、CircuitBreaker断路器介绍

1. CircuitBreaker断路器用于服务熔断和降级。Spring Cloud Circuit Breaker提供了跨不同断路器实现的抽象，它提供了在应用程序中使用的一致API，允许开发人员选择最合适的应用程序需求的断路器实现。CircuitBreaker支持实现的实现组件有Resilience4J（重点、核心）和Spring Retry，简而言之CircuitBreaker只是一套规范和接口，落地实现者是Resilience4J

2. CircuitBreaker的状态：CLOSED、OPEN和HALF_OPEN三个常用状态以及DISABLED和FORCED_OPEN两个特殊状态

   ![](../../../TyporaImage/9b0a20fd2dae4885a8861b32d2b61d12.png)

   ![](../../../TyporaImage/15e640ad6dfc495eb9eb6d5a129c1120.png)

3. CircuitBreaker的目的是保护分布式系统免受**故障和异常**，提高系统的可用性和健壮性。当一个组件或服务出现故障时，CircuitBreaker会迅速切换到开放OPEN状态（保险丝跳闸断电），阻止请求发送到该组件或服务从而避免更多的请求发送到该组件或请求。这可以减少对该组件或服务的负载，防止该组件或服务进一步崩溃，并使整个系统能够继续正常运行。同时，CircuitBreaker还可以提高系统的可用性和健壮性，因为它可以在分布式系统的各个组件之间自动切换，从而避免单点故障的问题。**服务熔断不是针对的某个接口出现故障时挂掉这个接口，而是挂掉整个服务，在半开状态下也不是某个挂掉的接口进行试探服务是否恢复，而是整个系统中某些接口去请求服务是否恢复，是否达到服务容错阈值也是根据某些接口请求服务去计算的，而不是针对某个已知挂掉的接口去多次请求而计算的**

4. CircuitBreaker断路器的前辈Hystrix（豪猪）：Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统中，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。但是Hystrix已不再维护，不推荐使用了

5. 断路器本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险），向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩

6. 服务雪崩概念：微服务调用链路中（A调用B，B调用C等链路）的某个服务故障，引起整个链路中的所有微服务都不可用，这就是雪崩

   - 避免因瞬时高并发流量而导致的服务故障，则采用流量控制
   - 避免因服务故障而引起的雪崩问题的解决方式，需采用超时处理、线程隔断、降级熔断等方式

7. 服务熔断概念：类似于保险丝，保险丝闭合状态可以正常使用，当达到最大服务访问时，直接拒绝访问跳闸限电，此时调用方会接受服务降级的处理并返回友好兜底提示。服务熔断和服务降级不是一个概念

8. 服务降级概念：比如提示服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback

9. 服务限流概念：秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒N个即可，有序进行

10. 服务限时概念：在程序允许的时间范围内访问，其他的时间不可访问

11. 服务预热概念：刚开始允许访问的请求少，一级一级的往上增加允许访问的条数

## 二、Resilience4J介绍

![](../../../TyporaImage/27ef0d9078b47cd142758f40e6a8c3a0.png)

## 三、Resilience4J实现

### 一、服务熔断/降级（CircuitBreaker）

1. [官方文档](https://github.com/lmhmhl/Resilience4j-Guides-Chinese/blob/main/core-modules/CircuitBreaker.md)

2. 主要使用的的参数

   | 配置属性                              | 默认值      | 描述                                                         |
   | ------------------------------------- | ----------- | ------------------------------------------------------------ |
   | failureRateThreshold                  | 50          | 以百分比配置失败率阈值。当失败率等于或大于阈值时，断路器状态并关闭变为开启，并进行服务降级。当坏的调用达到50%时，那么整个服务都会被坏的请求拖垮，之前好的请求也不能用了 |
   | slidingWindowType                     | COUNT_BASED | 配置滑动窗口的类型，当断路器关闭时，将调用的结果记录在滑动窗口中。滑动窗口的类型可以是count-based（基于次数）或time-based（基于时间）。如果滑动窗口类型是COUNT_BASED，将会统计记录最近`slidingWindowSize`次调用的结果。如果是TIME_BASED，将会统计记录最近`slidingWindowSize`秒的调用结果 |
   | slidingWindowSize                     | 100         | 配置滑动窗口的大小。如果滑动窗口类型是COUNT_BASED，则10次调用中有50%失败（即5次）打开熔断断路器。如果是TIME_BASED，此时还需要额外设置两个属性。含义为在N秒内（sliding-window-size）100%（slow-call-rate-threshold）的请求超过N秒（slow-call-duration-threshold）打开断路器 |
   | slowCallRateThreshold                 | 100         | 以百分比的方式配置，断路器把调用时间大于`slowCallDurationThreshold`的调用视为满调用，当慢调用比例大于等于阈值时，断路器开启，并进行服务降级 |
   | slowCallDurationThreshold             | 60000 [ms]  | 配置调用时间的阈值，高于该阈值的呼叫视为慢调用，并增加慢调用比例 |
   | permittedNumberOfCallsInHalfOpenState | 10          | 断路器在半开状态下允许通过的调用次数                         |
   | minimumNumberOfCalls                  | 100         | 断路器计算失败率或慢调用率之前所需的最小调用数（每个滑动窗口周期）。例如，如果minimumNumberOfCalls为10，则必须至少记录10个调用，然后才能计算失败率。如果只记录了9次调用，即使所有9次调用都失败，断路器也不会开启 |
   | waitDurationInOpenState               | 60000 [ms]  | 断路器从开启过渡到半开应等待的时间                           |

3. 实战案例需求说明：6次访问中当执行方法的失败率达到50%时CircuitBreaker将进入OPEN状态（保险丝跳闸断电）拒绝所有请求，等待5秒后，CircuitBreaker将自动从开启OPEN状态过渡到半开HALF_OPEN状态，允许一些请求通过以测试服务是否恢复正常。如果还是异常，CircuitBreaker则重新进入开启OPEN状态；如正常将进入关闭CLOSED开合状态，恢复正常处理请求

4. 在cloud-provider-payment8001提供者工程中添加新的controller

   ```java
   @RestController
   @RequestMapping("/pay")
   @Slf4j
   public class PayCircuitController {
   
       @Resource
       private IPayService payService;
   
       /**
        * @return
        * Resilience4J CircuitBreaker的案例
        **/
       @GetMapping(value = "/getCircuitById/{id}")
       public String getInfoById(@PathVariable("id") Integer id) {
           if(id < 0){
               throw new RuntimeException("id不能为负数");
           }
   
           if(id > 999){
               try {
                   TimeUnit.SECONDS.sleep(5);
               }catch (InterruptedException e){
                   e.printStackTrace();
               }
           }
           return "Hello, CircuitBreaker inputId："+ id +" \t" + IdUtil.simpleUUID();
       }
   
   }
   ```

5. 在cloud-api-commons公共工程的PayFeignApi接口中添加新的接口

   ```javascript
   @GetMapping(value = "/pay/getCircuitById/{id}")
   public String getCircuitById(@PathVariable("id") Integer id);
   ```

6. 在cloud-consumer-feign-order80消费者工程添加CircuitBreaker配置，也就是在接口请求方添加保险丝

   - 添加依赖

   ```xml
   <!-- resilience4J circuitbreaker -->
   <dependency>
   	<groupId>org.springframework.cloud</groupId>
   	<artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
   </dependency>
   <!-- 由于断路保护等需要AOP实现，所以必须导入AOP包 -->
   <dependency>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-starter-aop</artifactId>
   </dependency>
   ```

   - yaml文件中添加配置

   ```yaml
   spring:
     application:
       name: cloud-consumer-feign-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
           prefer-ip-address: true #优先使用服务ip进行注册
         config:
           profile-separator: '-'
           format: yaml
       openfeign:
         client:
           config:
             default:
               connect-timeout: 20000
               read-timeout: 20000
         httpclient:
           hc5:
             enabled: true
         compression:
           request:
             enabled: true
             # 最小触发压缩的大小
             min-request-size: 2028
             # 触发压缩数据类型
             mime-types: text/xml,application/xml,application/json
           response:
             enabled: true
         # 开启circuitbreaker和分组激活spring.cloud.openfeign.circuitbreaker.enabled
         circuitbreaker:
           enabled: true
           group:
             # 没开分组永远不用分组的配置。
             # 精确优先，分组次之（如果开了分组之后，分组也是在精确之后），默认行为最后
             enabled: true
     main:
       allow-bean-definition-overriding: true
   
   logging:
     level:
       com:
         atguigu:
           cloud:
             apis:
               PayFeignApi: debug
   
   # 基于次数的降级
   resilience4j:
     timelimiter:
       configs:
         default:
           timeout-duration: 10s # 默认1s 超过1s直接降级 (坑)
     circuitbreaker: # 降级熔断
       configs:
         default:
           failure-rate-threshold: 50 
           # 调用失败达到50%后打开断路器，就跳闸。当坏的调用达到50%时，那么整个服务都会被坏的请求拖垮，
           # 之前好的请求也不能用了
           sliding-window-type: count_based 
           # 滑动窗口类型
           sliding-window-size: 6 
           # 滑动窗口大小 若count_based则表示6个请求 若time_base则表示6秒
           minimum-number-of-calls: 6 
           # 每个滑动窗口的周期，也就是在半开状态下探路先锋的最小数量，
           # 如果未达到指定数量，即使探路先锋全成功或全失败也不会改变此时的状态
           automatic-transition-from-open-to-half-open-enabled: true 
           # 开启过渡到半开状态，默认为true
           wait-duration-in-open-state: 5s 
           # 从开启到半开启需要5s
           permitted-number-of-calls-in-half-open-state: 2 
           # 半开状态允许通过的最大请求数，在半开状态下circuitBreaker将允许最多
           # permitted-number-of-calls-in-half-open-state个请求通过，
           # 如果其中有任何一个请求失败，circuitBreaker将重新进入开启状态
           record-exceptions:
             - java.lang.Exception
       instances:
         cloud-payment-service:
           base-config: default
   ```

   - 新建Controller来完成CircuitBreaker调用测试，并提供服务降级提示友好

   ```java
   @RestController
   @RequestMapping("/feign/pay")
   public class OrderCircuitController {
   
       @Resource
       private PayFeignApi payFeignApi;
   
       @GetMapping(value = "/getCircuitById/{id}")
       @CircuitBreaker(name = "cloud-payment-service",fallbackMethod = "myCircuitFallback")
       public String getCircuitById(@PathVariable("id") Integer id) {
           return payFeignApi.getCircuitById(id);
       }
   
       public String myCircuitFallback(Integer id, Throwable t){
           return "系统繁忙，请稍后再试";
       }
   }
   ```

7. 以上是基于次数的滑动窗口计算方式，基于时间的滑动窗口的计算方式为：是通过有N个桶的环形数组实现，如果滑动窗口的大小为10秒，这个环形数组总是有10个桶，每个桶统计了在这一秒钟发生的所有调用结果（部分统计结果），数组中的第一个桶存储了当前这一秒内的所有调用的结果，其他的桶存储了之前每秒调用的结果。滑动窗口不会单独存储所有的调用结果，而是对每个桶内的统计结果和总的统计值进行增量的更新，当新的调用结果被记录时，总的统计值会进行增量更新。每个桶在进行部分统计时存储了三个整形，分别为失败调用时间、慢调用数、总调用数，还有一个long类型变量，存储所有调用的响应时间

8. 在cloud-consumer-feign-order80消费者工程添加基于时间窗口滑动的CircuitBreaker配置

   ```yaml
   spring:
     application:
       name: cloud-consumer-feign-service
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           service-name: ${spring.application.name}
           prefer-ip-address: true #优先使用服务ip进行注册
         config:
           profile-separator: '-'
           format: yaml
       openfeign:
         client:
           config:
             default:
               connect-timeout: 20000
               read-timeout: 20000
         httpclient:
           hc5:
             enabled: true
         compression:
           request:
             enabled: true
             # 最小触发压缩的大小
             min-request-size: 2028
             # 触发压缩数据类型
             mime-types: text/xml,application/xml,application/json
           response:
             enabled: true
         # 开启circuitbreaker和分组激活spring.cloud.openfeign.circuitbreaker.enabled
         circuitbreaker:
           enabled: true
           group:
             # 没开分组永远不用分组的配置。
             # 精确优先，分组次之（如果开了分组之后，分组也是在精确之后），默认行为最后
             enabled: true
     main:
       allow-bean-definition-overriding: true
   
   logging:
     level:
       com:
         atguigu:
           cloud:
             apis:
               PayFeignApi: debug
   
   # 基于时间的降级
   resilience4j:
     timelimiter:
       configs:
         default:
           timeout-duration: 10s 
           # 默认1s 超过1s直接降级 (坑)
     circuitbreaker:
       configs:
         default:
           failure-rate-threshold: 50 
           # 调用失败达到50%后打开断路器
           slow-call-duration-threshold: 2s 
           # 慢调用时间阈值，高于这个阈值的则视为慢调用并增加慢调用比例
           slow-call-rate-threshold: 30 
           # 慢调用百分比峰值，断路器把调用时间大于slow-call-duration-threshold视为慢调用，
           # 当慢调用的比例高于阈值，断路器打开，并开启服务降级
           sliding-window-type: time_based 
           # 滑动窗口类型
           sliding-window-size: 6 
           # 滑动窗口大小 若count_based则表示6个请求 若time_base则表示6秒
           minimum-number-of-calls: 2 
           # 每个滑动窗口的周期
           automatic-transition-from-open-to-half-open-enabled: true 
           # 开始过度到半开状态
           wait-duration-in-open-state: 5s 
           # 从开启到半开启需要5s
           permitted-number-of-calls-in-half-open-state: 2 
           #半开状态允许通过的最大请求数
           record-exceptions:
             - java.lang.Exception
       instances:
         cloud-payment-service:
           base-config: default
   ```

### 二、隔离（BulkHead）

1. BulkHead的概念：舱壁隔离，类似于鸳鸯锅，两者锅底互不影响。主要用于限并发

2. BulkHead的作用：依赖隔离和负载保护，用来限制对于下游服务的最大并发数量的限制

3. Resilience4J提供了两种隔离的实现方式，可以限制并发执行的数量

   - SemaphoreBulkhead使用了信号量，可以在各种线程和I/O模型上正常工作，与Hystrix不同，它不提供基于shadow的thread选型，由客户端来确保正确的线程池大小与隔离配置
   - FixedThreadPoolBulkhead：使用了有界队列和固定大小线程池

4. 实现SemaphoreBulkhead（信号量舱壁）

   - SemaphoreBulkhea原理是当信号量有空闲时，进入系统的请求会直接获取信号量并开始业务处理。当信号量有空闲时，进入系统的请求会直接获取信号量并开始业务处理。当信号量全被占用时，接下来的请求将会进入阻塞状态，SemaphoreBulkhead提供了一个阻塞计时器；如果阻塞状态的请求在阻塞计时内无法获取到信号量则系统会拒绝这些请求；如果请求在阻塞计时内获取到了信号量，那将直接获取信号量并执行相应的业务处理

   - 在cloud-provider-payment8001提供者工程中的PayCircuitController新增接口

     ```java
     @RestController
     @RequestMapping("/pay")
     @Slf4j
     public class PayCircuitController {
     
         @Resource
         private IPayService payService;
     
         /**
          * @return
          * Resilience4J CircuitBreaker的案例
          **/
         @GetMapping(value = "/getBulkheadById/{id}")
         public String getBulkheadById(@PathVariable("id") Integer id) {
             if(id < 0){
                 throw new RuntimeException("id不能为负数");
             }
     
             if(id > 999){
                 try {
                     TimeUnit.SECONDS.sleep(5);
                 }catch (InterruptedException e){
                     e.printStackTrace();
                 }
             }
             return "Hello, CircuitBreaker inputId："+ id +" \t" + IdUtil.simpleUUID();
         }
     
     }
     ```

   - 在cloud-api-commons公共工程的PayFeignApi接口中添加新的接口

     ```java
     @GetMapping(value = "/pay/getBulkheadById/{id}")
     public String getBulkheadById(@PathVariable("id") Integer id);
     ```

   - 在cloud-consumer-feign-order80消费者工程添加Bulkhead依赖

     ```xml
     <!-- resilience4J bulkhead -->
     <dependency>
       <groupId>io.github.resilience4j</groupId>
       <artifactId>resilience4j-bulkhead</artifactId>
     </dependency>
     ```

   - 在cloud-consumer-feign-order80消费者工程添加Bulkhead的yaml配置

     ```yaml
     spring:
       application:
         name: cloud-consumer-feign-service
       cloud:
         consul:
           host: localhost
           port: 8500
           discovery:
             service-name: ${spring.application.name}
             prefer-ip-address: true #优先使用服务ip进行注册
           config:
             profile-separator: '-'
             format: yaml
         openfeign:
           client:
             config:
               default:
                 connect-timeout: 20000
                 read-timeout: 20000
           httpclient:
             hc5:
               enabled: true
           compression:
             request:
               enabled: true
               # 最小触发压缩的大小
               min-request-size: 2028
               # 触发压缩数据类型
               mime-types: text/xml,application/xml,application/json
             response:
               enabled: true
           # 开启circuitbreaker和分组激活spring.cloud.openfeign.circuitbreaker.enabled
           circuitbreaker:
             enabled: true
             group:
               # 没开分组永远不用分组的配置。
               # 精确优先，分组次之（如果开了分组之后，分组也是在精确之后），默认行为最后
               enabled: true
       main:
         allow-bean-definition-overriding: true
     
     logging:
       level:
         com:
           atguigu:
             cloud:
               apis:
                 PayFeignApi: debug
     
     # 基于次数的降级
     resilience4j:
       timelimiter:
         configs:
           default:
             timeout-duration: 10s # 默认1s 超过1s直接降级 (坑)
       circuitbreaker: # 降级熔断
         configs:
           default:
             failure-rate-threshold: 50 
             # 调用失败达到50%后打开断路器，就跳闸。当坏的调用达到50%时，那么整个服务都会被坏的请求拖垮，
             # 之前好的请求也不能用了
             sliding-window-type: count_based 
             # 滑动窗口类型
             sliding-window-size: 6 
             # 滑动窗口大小 若count_based则表示6个请求 若time_base则表示6秒
             minimum-number-of-calls: 6 
             # 每个滑动窗口的周期，也就是在半开状态下探路先锋的最小数量，
             # 如果未达到指定数量，即使探路先锋全成功或全失败也不会改变此时的状态
             automatic-transition-from-open-to-half-open-enabled: true 
             # 开启过渡到半开状态，默认为true
             wait-duration-in-open-state: 5s 
             # 从开启到半开启需要5s
             permitted-number-of-calls-in-half-open-state: 2 
             # 半开状态允许通过的最大请求数，在半开状态下circuitBreaker将允许最多
             # permitted-number-of-calls-in-half-open-state个请求通过，
             # 如果其中有任何一个请求失败，circuitBreaker将重新进入开启状态
             record-exceptions:
               - java.lang.Exception
         instances:
           cloud-payment-service:
             base-config: default
        
       # 信号量舱壁
       bulkhead:
           configs:
             default:
               max-concurrent-calls: 2
               # 舱壁允许的最大并发执行量，默认为25
               max-wait-duration: 1s
               # 尝试进入饱和舱壁时，应阻塞线程的最长时间，默认为0。
               # 此处设置为1s，过时不候进舱壁兜底fallback
           instances:
             cloud-payment-service:
               base-config: default     
     ```

   - 在cloud-consumer-feign-order80消费者工程中的OrderCircuitController新增接口

     ```java
     @GetMapping(value = "/getBulkheadById/{id}")
     @Bulkhead(name = "cloud-payment-service",fallbackMethod = "myBulkheadFallback",type = Bulkhead.Type.SEMAPHORE)
     public String getBulkheadById(@PathVariable("id") Integer id) {
     	return payFeignApi.getBulkheadById(id);
     }
     
     public String myBulkheadFallback(Integer id, Throwable t){
     	return "系统繁忙，请稍后再试";
     }
     ```

5. 实现FixedThreadPoolBulkhead（固定线程池舱壁）

   - FixedThreadPoolBulkhead原理的功能与SemaphoreBulkhead一样也是用于限制并发执行次数的，但是二者的实现原理存在差异而且表现效果也存在细微的差别。FixedThreadPoolBulkhead使用一个固定线程池和一个等待队列来实现舱壁。当线程池中存在空闲时，则此时进入系统的请求将直接进入线程池开启新线程或使用空闲线程来处理请求；当线程池无空闲时，接下来的请求将进入等待队列，若等待队列仍然无剩余空间时接下来的请求将直接拒绝，若队列中的请求等待线程池出现空闲时将进入线程池进行业务处理。FixedThreadPoolBulkhead只对CompletableFuture方法有效，所以必须创建返回CompletableFuture类型的方法来实现FixedThreadPoolBulkhead
   
   - 在cloud-consumer-feign-order80消费者工程添加Bulkhead的yaml配置
   
     ```yaml
     spring:
       application:
         name: cloud-consumer-feign-service
       cloud:
         consul:
           host: localhost
           port: 8500
           discovery:
             service-name: ${spring.application.name}
             prefer-ip-address: true #优先使用服务ip进行注册
           config:
             profile-separator: '-'
             format: yaml
         openfeign:
           client:
             config:
               default:
                 connect-timeout: 20000
                 read-timeout: 20000
           httpclient:
             hc5:
               enabled: true
           compression:
             request:
               enabled: true
               # 最小触发压缩的大小
               min-request-size: 2028
               # 触发压缩数据类型
               mime-types: text/xml,application/xml,application/json
             response:
               enabled: true
           # 开启circuitbreaker和分组激活spring.cloud.openfeign.circuitbreaker.enabled
           circuitbreaker:
             enabled: true
             group:
               # 没开分组永远不用分组的配置。
               # 精确优先，分组次之（如果开了分组之后，分组也是在精确之后），默认行为最后
               enabled: true
       main:
         allow-bean-definition-overriding: true
     
     logging:
       level:
         com:
           atguigu:
             cloud:
               apis:
                 PayFeignApi: debug
     
     # 基于次数的降级
     resilience4j:
       timelimiter:
         configs:
           default:
             timeout-duration: 10s # 默认1s 超过1s直接降级 (坑)
       circuitbreaker: # 降级熔断
         configs:
           default:
             failure-rate-threshold: 50 
             # 调用失败达到50%后打开断路器，就跳闸。当坏的调用达到50%时，
             # 那么整个服务都会被坏的请求拖垮，
             # 之前好的请求也不能用了
             sliding-window-type: count_based 
             # 滑动窗口类型
             sliding-window-size: 6 
             # 滑动窗口大小 若count_based则表示6个请求 若time_base则表示6秒
             minimum-number-of-calls: 6 
             # 每个滑动窗口的周期，也就是在半开状态下探路先锋的最小数量，
             # 如果未达到指定数量，即使探路先锋全成功或全失败也不会改变此时的状态
             automatic-transition-from-open-to-half-open-enabled: true 
             # 开启过渡到半开状态，默认为true
             wait-duration-in-open-state: 5s 
             # 从开启到半开启需要5s
             permitted-number-of-calls-in-half-open-state: 2 
             # 半开状态允许通过的最大请求数，在半开状态下circuitBreaker将允许最多
             # permitted-number-of-calls-in-half-open-state个请求通过，
             # 如果其中有任何一个请求失败，circuitBreaker将重新进入开启状态
             record-exceptions:
               - java.lang.Exception
         instances:
           cloud-payment-service:
             base-config: default
        
       # 信号量舱壁（基于线程池方式实现）
       bulkhead:
           configs:
             default:
               max-thread-pool-sezi: 4
               # 配置最大线程池大小，
               # 默认为Runtime.getRuntime().availableProcessors()代表数量
               core-thread-pool-size: 2
               # 配置核心线程池大小
               # 默认为Runtime.getRuntime().availableProcessors()-1
               queue-capacity: 100
               # 配置队列的容量，默认为100
               keep-alive-duration: 20ms
               # 当线程数大于核心数时，这是多余空闲线程在终止前等待新任务的最长时间
               # 默认20ms
           instances:
             cloud-payment-service:
               base-config: default    
           # spring.cloud.openfeign.circuitbreaker.group.enabled
           # 需要设置为false，新启线程需要和原来的主线程脱离
     ```
   
   - 在cloud-consumer-feign-order80消费者工程中的OrderCircuitController修改接口
   
   	```java
   	@GetMapping(value = "/getBulkheadById/{id}")
   	@Bulkhead(name = "cloud-payment-service",fallbackMethod = "myBulkheadThreadPoolFallback",type = Bulkhead.Type.THREADPOOL)
   	public CompletableFuture<String> getBulkheadThreadPoolById(@PathVariable("id") Integer id) {
   		return CompletableFuture.supplyAsync(() -> payFeignApi.getBulkheadById(id) + "\t" + "Bulkhead.Type.THREADPOOL");
   	}
   	
   	public CompletableFuture<String> myBulkheadThreadPoolFallback(Integer id, Throwable t){
   	    return CompletableFuture.supplyAsync(() -> "系统繁忙，请稍后再试");
   	}
   	```

### 三、限流（RateLimiter）

1. 限流的概念：限流就是限制最大访问流量，系统能提供的最大并发是有限的，同时来的请求又太多，就需要限流，限流是频率控制，比如商城秒杀业务，瞬时大量请求涌入，服务器忙不过来就只能排队限流。简而言之，限流就是通过对并发访问/请求进行**限速**，或者对一个时间窗口内的请求进行**限速**，以保护应用系统，一旦达到限制速率则可以拒绝服务、排队或等待、降级等处理

2. 限流的常见算法

  - 漏斗算法（Leaky Bucket）：使用一个固定容量的漏桶，按照设定常量固定速率流出水滴，也就是漏斗，类似于医院打吊针，不管源头流量多大，可以设定匀速流出。如果流入水滴超过了桶的容量，则流入的水滴将会溢出（被丢弃），而漏桶容量是不变的。但是漏桶算法对于存在突发特性的流量来说缺乏效率，因为漏斗的效率是匀速的，所以不会提高效率

  - 令牌桶算法（Token Bucket）：Spring Cloud默认使用该算法

  	![image-20240523111805164](../../../TyporaImage/image-20240523111805164.png)

  - 滚动时间窗口（tumbling time window）：允许**固定数量**的请求进入（比如1秒取4个请求数据相加，4个请求数据加和次数超过25值就over）超过数量就拒绝或者排队，等下一个时间段进入。由于是在一个时间间隔内进行限制，如果用户在上个时间间隔结束前请求（但没有超过限制），同时在当前时间间隔刚开始请求（同样没抽过限制）。在各自的时间间隔内，这些请求都是正常的。但是间隔临界的一段时间内的请求就会超过系统限制，可能导致系统被压垮

  - 滑动时间窗口（sliding time window）：顾名思义该窗口是滑动的。窗口即需要定义窗口的大小；滑动即需要定义在窗口中滑动的大小，但理论上讲滑动的大小不能超过窗口的大小。滑动窗口算法是把固定时间片进行划分并且随着时间移动，移动方式为开始时间点变为时间列表中的第2个时间点，结束时间点增加一个时间点，通过这种方式可以巧妙的避开计数器的临界点的问题。如下

  	![image-20240523133322584](../../../TyporaImage/image-20240523133322584.png)

3. 限流的实操

	- 在cloud-provider-payment8001提供者工程中的PayCircuitController新增接口

		```java
		@GetMapping("/pay/getRateLimitById/{id}")
		public Result<String> getRateLimitById(@PathVariable("id") Integer id) {
			return Result.success("Hello" + id + IdUtil.simpleUUID());
		}
		```

	- 在cloud-api-commons公共工程的PayFeignApi接口中添加新的接口

		```java
		@GetMapping("/pay/getRateLimitById/{id}")
		Result<String> getRateLimitById(@PathVariable("id") Integer id);
		```

	- 在cloud-consumer-feign-order80消费者工程添加Ratelimiter依赖

		```xml
		<!-- resilience4J bulkhead -->
		<dependency>
		  <groupId>io.github.resilience4j</groupId>
		  <artifactId>resilience4j-ratelimiter</artifactId>
		</dependency>
		```

	- 在cloud-consumer-feign-order80消费者工程配置yaml

		```yaml
		resilience4j:
		  ratelimiter:
		    configs:
		      default:
		        limit-for-period: 2 
		        # 一次刷新周期内允许最大的请求数，默认为50
		        limit-refresh-period: 1s 
		        # 限流器每隔limit-refresh-period刷新一次，
		        # 将允许处理的最大请求数量重置为limit-refresh-period
		        # 默认为500纳秒
		        timeout-duration: 1 
		        # 线程等待权限的默认等待时间，默认为5秒
		    instances:
		      cloud-payment-service:
		        base-config: default
		```

	- 在cloud-consumer-feign-order80消费者工程中的OrderCircuitController新增接口

		```java
		@GetMapping(value = "/getBulkheadById/{id}")
		@RateLimiter(name = "cloud-payment-service",fallbackMethod = "myRateLimiterFallback",type = Bulkhead.Type.SEMAPHORE)
		public String getBulkheadById(@PathVariable("id") Integer id) {
			return payFeignApi.getBulkheadById(id);
		}
		
		public String myRateLimiterFallback(Integer id, Throwable t){
			return "系统繁忙，请稍后再试";
		}
		```


# 七、Sleuth（Micrometer）+ ZipKin分布式链路追踪

## 一、分布式链路追踪概述

- 分布式链路追踪（Distributed Tracing），就是将一次分布式请求还原成调用链路，进行日志记录，性能监控并将一次分布请求的调用情况集中展示。比如各个服务节点上的耗时、请求具体到达哪台机器上、每个服务节点的请求状态等等
- Sleuth已经被Micrometer替代
- Spring Cloud Sleuth（Micrometer）是将一次分布式请求还原成调用链路，进行日志记录和性能监控，并将一次分布式请求的调用情况集中发送给zipkin，zipkin负责在web展示
- zipkin是一种分布式链路跟踪系统图形化工具，zipkin是Twitter开源的分布式跟踪系统，能够手机微服务运行过程中的实时调用链路信息，并能够将这些调用链路信息展示到Web图形化界面上供开发人员分析，开发人员能够从zipkin中分析除调用链路中的性能瓶颈，识别除存在问题的应用程序，进而定位问题和解决问题

## 二、分布式链路追踪原理

1. 假定微服务调用的链路为Service1调用Service2，Service2调用Service3和Service4

2. 一条链路追踪会在每个服务调用的时候加上TraceID（全局ID，也就是整个请求链路的ID）和SpanID（每次请求发送的ID）。链路是通过TraceID唯一标识；Span标识发起的请求信息，各Span通过parentId（也就是当前请求的上一个服务请求id）关联起来（Span：表示调用链路来源，通俗的理解span就是一次请求信息）

3. 相关概念

	- CS：Client Sent即客户端发送这个请求的时间
	- SR：Server Received即服务器接收到这个请求的时间
	- CR：Client Received即客户端接收到数据的时间
	- SS：Server Sent即服务端发送响应的时间
	- SR - CS = 网络传输时间（请求过去）
	- SS - SR = 业务处理时间
	- CR - CS = 远程调用耗时
	- CR - SS = 网络传输时间（响应过来）

4. 链路调用示例图

	- 通过parentId就可以找到父节点，整个链路即可以进行跟踪追溯了
	- 这整个过程每个节点的TraceID是相同的

	![image-20240523152623502](../../../TyporaImage/image-20240523152623502.png)

## 三、zipkin下载及运行

1. [zipkin官网下载](https://zipkin.io/pages/quickstart.html)

2. zipkin运行命令

	```shell
	java -jar zipkin的jar名称.jar
	```

3. 打开网站地址`localhost:9411/zipkin`即可

## 四、分布式链路追踪案例

1. 在顶级pom文件中添加如下配置

	```xml
	<micrometer-tracing.version>1.2.0</micrometer-tracing.version>
	<micrometer-observation.version>1.12.0</micrometer-observation.version>
	<feign-micrometer.version>12.5</feign-micrometer.version>
	<zipkin-reporter-brave.version>2.17.0</zipkin-reporter-brave.version>
	
	
	<!--micrometer-tracing一系列包  -->
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-tracing-bom</artifactId>
	    <version>${micrometer-tracing.version}</version>
	    <type>pom</type>
	    <scope>import</scope>
	</dependency>
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-tracing</artifactId>
	    <version>${micrometer-tracing.version}</version>
	</dependency>
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-tracing-bridge-brave</artifactId>
	    <version>${micrometer-tracing.version}</version>
	</dependency>
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-observation</artifactId>
	    <version>${micrometer-observation.version}</version>
	</dependency>
	<dependency>
	    <groupId>io.github.openfeign</groupId>
	    <artifactId>feign-micrometer</artifactId>
	    <version>${feign-micrometer.version}</version>
	</dependency>
	<dependency>
	    <groupId>io.zipkin.reporter2</groupId>
	    <artifactId>zipkin-reporter-brave</artifactId>
	    <version>${zipkin-reporter-brave.version}</version>
	</dependency>
	```

2. 添加的依赖包的作用

	| 依赖包                          | 说明                                                         |
	| ------------------------------- | ------------------------------------------------------------ |
	| micrometer-tracing-bom          | 导入链路追踪版本中心，体系化说明                             |
	| micrometer-tracing              | 指标追踪                                                     |
	| micrometer-tracing-bridge-brave | 一个micrometer模块，用于与分布式跟踪工具brave集成，以收集应用程序的分布式跟踪数据，brave是一个开源的分布式跟踪工具，它可以帮助用户在分布式系统中跟踪请求的流转，它使用一种称为跟踪上下文的机制，将请求的跟踪信息存储在请求的头部，然后将请求传递给下一个服务，在整个请求链中，brave会将每个服务处理请求的时间和其他信息存储到跟踪数据中，以使用户可以了解整个请求的路径和性能 |
	| micrometer-observation          | 一个基于度量库micrometer的观测模块，用于收集应用程序的度量数据 |
	| feign-micrometer                | 一个Feign HTTP客户端的micrometer模块，用于收集客户端请求的度量数据 |
	| zipkin-reporter-brave           | 一个用于将brave跟踪数据报告到zipkin跟踪系统的库              |
	| spring-boot-starter-actuator    | SpringBoot框架的一个模块用于监视和管理应用程序。只是个补充包 |

3. 在cloud-provider-payment8001提供者工程和cloud-consumer-feign-order80消费者工程中配置依赖（最好牵扯到服务的pom都引入此依赖，公共工程不需要引入此依赖）

	```xml
	<!--micrometer-tracing一系列包  -->
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-tracing</artifactId>
	</dependency>
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-tracing-bridge-brave</artifactId>
	</dependency>
	<dependency>
	    <groupId>io.micrometer</groupId>
	    <artifactId>micrometer-observation</artifactId>
	</dependency>
	<dependency>
	    <groupId>io.github.openfeign</groupId>
	    <artifactId>feign-micrometer</artifactId>
	</dependency>
	<dependency>
	    <groupId>io.zipkin.reporter2</groupId>
	    <artifactId>zipkin-reporter-brave</artifactId>
	</dependency>
	```

4. 在cloud-provider-payment8001提供者工程和cloud-consumer-feign-order80消费者工程中配置yaml

	```yaml
	management:
	  zipkin:
	    tracing:
	      endpoint: http://localhost:9411/api/v2/spans
	  tracing:
	    sampling:
	      probability: 1.0 # 采样率百分比，默认值0.1(就是10%的链路会被记录下来)
	```

5. 在cloud-provider-payment8001提供者工程新建Controller

	```java
	@RestController
	@RequestMapping("/pay")
	public class PayMicrometerController {
	
	    @GetMapping("/getMicrometerById/{id}")
	    public String getMicrometerById(@PathVariable("id") Integer id) {
	        return "这是链路追踪, Id:" + IdUtil.simpleUUID();
	    }
	}
	```

6. 在cloud-api-commons公共工程的PayFeignApi接口中添加新的接口

	```java
	@GetMapping("/pay/getMicrometerById/{id}")
	Result<String> getMicrometerById(@PathVariable("id") Integer id);
	```

7. cloud-consumer-feign-order80消费者工程中新建Controller

	```java
	@RestController
	@RequestMapping("/feign/pay")
	public class OrderMicrometerController {
	
	    @Resource
	    private PayFeignApi payFeignApi;
	
	    @GetMapping("/getMicrometerById/{id}")
	    public String getMicrometerById(@PathVariable("id") Integer id) {
	        return payFeignApi.getMicrometerById(id);
	    }
	
	}
	```

8. 访问`http://localhost:9411`进行筛选即可看到服务的链路情况

