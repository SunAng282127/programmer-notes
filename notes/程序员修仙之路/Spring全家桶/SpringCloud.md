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
   - 轮询：将请求按顺序轮流地分配到每个节点上，不关心每个节点实际的连接数和当前的系统负载。优点是简单高效，易于水平扩展，每个节点满足字面意义上的均衡；缺点是没有考虑机器的性能问题，集群性能瓶颈更多的会受性能差的服务器影响 
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



# 五、OpenFeign服务接口调用

