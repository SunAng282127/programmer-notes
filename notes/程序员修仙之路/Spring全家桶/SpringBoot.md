# 一、SpringBoot介绍

## 一、SpringBoot官网

- [Spring Boot官网](https://spring.io/projects/spring-boot)
- [Spring Boot官方文档](https://docs.spring.io/spring-boot/docs/)

## 二、Spring生态圈

1. [Spring官网](https://spring.io/)

2. Spring的能力

   ![Spring](C:/Users/MC/AppData/TyporaImage/springboot/20210205004146543.png)

3. Spring的生态

   - web开发
   - 数据访问
   - 安全控制
   - 分布式
   - 消息服务
   - 移动开发
   - 批处理

4. Spring5重大升级

   - 响应式编程

     ![springboot](C:/Users/MC/AppData/TyporaImage/20210205004250581.png)

## 三、SpringBoot的优缺点

### 一、SpringBoot优点

1. 创建独立Spring应用
2. 内嵌web服务器
3. 自动starter依赖，简化构建配置
4. 自动配置Spring以及第三方功能
5. 提供生产级别的监控、健康检查及外部化配置
6. 无代码生成、无需编写XML
7. SpringBoot是整合Spring技术栈的一站式框架
8. SpringBoot是简化Spring技术栈的快速开发脚手架

### 二、SpringBoot缺点

1. 人称版本帝，迭代快，需要时刻关注变化
2. 封装太深，内部原理复杂，不容易精通

## 四、微服务与分布式

### 一、微服务概述

1. 微服务是一种架构风格
2. 一个应用拆分为一组小型服务
3. 每个服务运行在自己的进程内，也就是可独立部署和升级
4. 服务之间使用轻量级HTTP交互
5. 服务围绕业务功能拆分
6. 可以由全自动部署机制独立部署
7. 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术

### 二、分布式的困难

1. 远程调用
2. 服务发现
3. 负载均衡
4. 服务容错
5. 配置管理
6. 服务监控
7. 链路追踪
8. 日志管理
9. 任务调度

### 三、分布式的解决

- SpringBoot + SpringCloud

### 四、云原生

- 原生应用如何上云。Cloud Native

### 五、上云的困难

1. 服务自愈
2. 弹性伸缩
3. 服务隔离
4. 自动化部署
5. 灰度发布
6. 流量治理

### 六、上云的解决

![在这里插入图片描述](C:/Users/MC/AppData/TyporaImage/springboot/20210205004621290.png)

## 五、微服务与分布式的联系

1.  分布式系统是一种架构范式 
    - 分布式系统是一种软件架构的设计方式，用于解决多台计算机协同工作的问题。微服务架构是在这种分布式系统的背景下提出的一种服务组织方式 
2.  微服务是一种实现方式 
    - 微服务是一种软件架构风格，它可以在分布式系统中应用。微服务架构将一个大型应用划分为一组小型、自治的服务，每个服务都有自己的业务逻辑，可以独立开发、部署和运行。微服务通常运行在分布式环境中，利用分布式系统的特性 
    - 微服务是分布式系统的一种形式： 微服务是分布式系统的一种实现形式，它采用了分布式的原则和理念。微服务通过将应用划分为小型服务并通过网络进行通信，体现了分布式系统的核心概念 
3.  总体而言，微服务架构可以视为是在分布式系统的基础上演进而来的一种服务架构，它更强调服务的独立性、自治性和松耦合，适用于构建灵活、可维护的大型应用系统。分布式系统作为一种更广泛的概念，包括了各种架构和设计方式，而微服务是其中一种具体的实践方式 

# 二、SpringBoot入门案例

1.  可以使用Spring Initializr来初始化项目

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
        <groupId>com.sunny</groupId>
        <artifactId>demo</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <name>demo</name>
        <description>demo</description>
        <properties>
            <java.version>1.8</java.version>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
            <spring-boot.version>2.6.13</spring-boot.version>
        </properties>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
    
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>
        </dependencies>
        <dependencyManagement>
            <dependencies>
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-dependencies</artifactId>
                    <version>${spring-boot.version}</version>
                    <type>pom</type>
                    <scope>import</scope>
                </dependency>
            </dependencies>
        </dependencyManagement>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                        <encoding>UTF-8</encoding>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <version>${spring-boot.version}</version>
                    <configuration>
                        <mainClass>com.sunny.bootdemo.DemoApplication</mainClass>
                        <skip>true</skip>
                    </configuration>
                    <executions>
                        <execution>
                            <id>repackage</id>
                            <goals>
                                <goal>repackage</goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    </project>
    
    ```

2. 创建主程序

   ```java
   @SpringBootApplication
   public class MainApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(MainApplication.class, args);
       }
   }
   ```

3. 编写业务

   ```java
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String handle01(){
           return "Hello, Spring Boot 2!";
       }
   }
   ```

4. 运行并测试

   - 运行`MainApplication`类
   - 浏览器输入`http://localhost:8888/hello`，将会输出`Hello, Spring Boot 2!`

5. 设置配置

   - maven工程的resource文件夹中创建application.properties文件

     ```properties
     # 设置端口号
     server.port=8888
     ```

   - [更多配置信息](https://docs.spring.io/spring-boot/docs/2.3.7.RELEASE/reference/html/appendix-application-properties.html#common-application-properties-server)

6. 打包配置

   - 在pom.xml添加

     ```xml
     <build>
     	<plugins>
     		<plugin>
     			<groupId>org.springframework.boot</groupId>
     			<artifactId>spring-boot-maven-plugin</artifactId>
     		</plugin>
     	</plugins>
     </build>
     ```

   - 在IDEA的Maven插件上点击运行clean 、package，把helloworld工程项目的打包成jar包

   - 打包好的jar包被生成在helloworld工程项目的target文件夹内

   - 用cmd运行`java -jar boot-01-helloworld-1.0-SNAPSHOT.jar`，既可以运行helloworld工程项目

   - 将jar包直接在目标服务器执行即可

# 三、SpringBoot特性

## 一、依赖管理特性

1. 父项目做依赖管理

   ```xml
   <!-- 依赖管理：在项目中引入spring-boot-starter-parent即可 -->
   <parent>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-starter-parent</artifactId>
   	<version>2.3.4.RELEASE</version>
   </parent>
   
   <!-- spring-boot-starter-parent的父依赖为spring-boot-dependencies，在项目中可以不引入。
        spring-boot-dependencies几乎声明了所有开发中常用的依赖的版本号，自动版本仲裁机制，
        即不需要在<dependency>标签中在声明<version>标签，使用默认的版本号 -->
   <parent>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-dependencies</artifactId>
   	<version>2.3.4.RELEASE</version>
   </parent>
   ```

2. 开发导入starter场景启动器

   - 见到很多`spring-boot-starter-*` ： `*`就某种场景，这种写法都是官方提供的依赖包

   - 只要引入`starter`，这个场景的所有常规需要的依赖我们都自动引入

   - [更多SpringBoot所有支持的场景](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)

   - 见到的  `*-spring-boot-starter`： 第三方为我们提供的简化开发的场景启动器，这种写法都是非官方或者自定义的依赖包

   - 所有场景启动器最底层的依赖

     ```xml
     <dependency>
     	<groupId>org.springframework.boot</groupId>
     	<artifactId>spring-boot-starter</artifactId>
     	<version>2.3.4.RELEASE</version>
     	<scope>compile</scope>
     </dependency>
     ```

3. 无需关注版本号，自动版本仲裁

   - 引入依赖默认都可以不写版本
   - 引入非版本仲裁的jar，要写版本号，可以先点进去看具体的版本是什么，然后需要修改再修改

4. 可以修改默认版本号

   - 查看spring-boot-dependencies里面规定当前依赖的版本用的key，如mysql驱动依赖包

     ```xml
     <!-- mysql驱动依赖包的key -->
     <mysql.version>8.0.33</mysql.version>
     
     <dependency>
             <groupId>mysql</groupId>
             <artifactId>mysql-connector-java</artifactId>
             <version>${mysql.version}</version>
             <exclusions>
               <exclusion>
                 <groupId>com.google.protobuf</groupId>
                 <artifactId>protobuf-java</artifactId>
               </exclusion>
             </exclusions>
     </dependency>
     ```

   - 在当前项目里面重写配置，只需要重写`<properties>`标签中的版本号即可，就可代表重新加载了指定版本的依赖包，如下面的代码

     ```xml
     <properties>
     	<mysql.version>5.1.43</mysql.version>
     </properties>
     
     ```

## 二、自动配置特性

1. 自动配好Tomcat

   - 引入Tomcat依赖
   - 配置Tomcat

   ```xml
   <dependency>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-starter-tomcat</artifactId>
   	<version>2.3.4.RELEASE</version>
   	<scope>compile</scope>
   </dependency>
   
   ```

2. 自动配好SpringMVC

   - 引入SpringMVC全套组件
   - 自动配好SpringMVC常用组件（功能）

3. 自动配好Web常见功能，如：字符编码问题、文件上传等

   - SpringBoot帮我们配置好了所有web开发的常见场景

   ```java
   public static void main(String[] args) {
       //1、返回我们IOC容器
       ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
   
       //2、查看容器里面的组件
       String[] names = run.getBeanDefinitionNames();
       for (String name : names) {
           System.out.println(name);
       }
   }
   
   ```

4. 默认的包结构

   - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
   - 无需以前的包扫描配置，想要改变扫描路径则使用
     - @SpringBootApplication(scanBasePackages="com.sunny")
     - @ComponentScan("指定扫描路径")

5. @SpringBootApplication等同于@SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan("com.sunny")这三个注解加一起

6. 各种配置拥有默认值

   - 默认配置最终都是映射到某个类（大部分都是配置类）上，如：`MultipartProperties`
   - 配置文件的值最终会绑定每个类（大部分都是配置类）上，这个类会在容器中创建对象

7. 按需加载所有自动配置项

   - 因为有非常多的starter，在项目中引入了哪些场景则这个场景的自动配置才会开启
   - SpringBoot所有的自动配置功能都在spring-boot-autoconfigure包里面

8. ......

# 四、底层注解

## 一、@Configuration

1. @Configuration简介

   - @Configuration标注的类为配置类，配置类本身也是组件

2. 基本使用

   - Full模式与Lite模式
   - 示例

   ```java
   /**
    * 1、配置类里面使用@Bean标注在方法上给容器注册组件，默认也是单实例的
    * 2、配置类本身也是组件
    * 3、proxyBeanMethods：代理bean的方法
    *      Full(proxyBeanMethods = true)（保证每个@Bean方法被调用多少次返回的组件都是单实例的）（默认）
    *      Lite(proxyBeanMethods = false)（每个@Bean方法被调用多少次返回的组件都是新创建的）
    */
   @Configuration(proxyBeanMethods = false)
   // @Configuration注解告诉SpringBoot这是一个配置类，等同于配置文件
   public class MyConfig {
   
       /**
        * Full：外部无论对配置类中的这个组件注册方法调用多少次获取的都是之前注册容器中的单实例对象，
                外部调用对象时都需要检查一下容器中是否已存在该实例对象
        * Lite：外部对配置类中的这个组件注册方法调用获取的是注册容器中的实例对象，非单例对象，都是全新对象，
                外部调用对象时不需要检查容器中是否已存在该实例对象，直接创建
        * @return
        */
       @Bean 
       // 给容器中添加组件。以方法名作为组件的id，也可以在@bean中指定组件的id，@Bean("usero1")即可
       // 返回类型就是组件类型。
       // 返回的值，就是组件在容器中的实例
       public User user01(){
           User zhangsan = new User("zhangsan", 18);
           //user组件依赖了Pet组件
           zhangsan.setPet(tomcatPet());
           return zhangsan;
       }
   
       @Bean("tom")
       public Pet tomcatPet(){
           return new Pet("tomcat");
       }
   }
   
   ```

3. @Configuration测试代码如下

   ```java
   @SpringBootConfiguration
   @EnableAutoConfiguration
   @ComponentScan("com.sunny.boot")
   public class MainApplication {
   
       public static void main(String[] args) {
       	//1、返回IOC容器
           ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
   
       	//2、查看容器里面的组件
           String[] names = run.getBeanDefinitionNames();
           for (String name : names) {
               System.out.println(name);
           }
   
       	//3、从容器中获取组件
           Pet tom01 = run.getBean("tom", Pet.class);
           Pet tom02 = run.getBean("tom", Pet.class);
           System.out.println("组件："+(tom01 == tom02));
   
      		//4、com.sunny.boot.config.MyConfig$$EnhancerBySpringCGLIB$$51f1e1ca@1654a892
           MyConfig bean = run.getBean(MyConfig.class);
           System.out.println(bean);
   
       	// 如果@Configuration(proxyBeanMethods = true)代理对象调用方法。
           // SpringBoot总会检查这个组件是否在容器中有。
           // 保持组件单实例
           User user = bean.user01();
           User user1 = bean.user01();
           System.out.println(user == user1);
   
           User user01 = run.getBean("user01", User.class);
           Pet tom = run.getBean("tom", Pet.class);
   
           System.out.println("用户的宠物："+(user01.getPet() == tom));
       }
   }
   
   ```

4. 最佳实战

   - 配置类组件之间**无依赖关系**用Lite模式加速容器启动过程，减少判断
   - 配置类组件之间**有依赖关系**，方法会被调用得到之前单实例组件，用Full模式（默认）

## 二、@Import

1. @Bean、@Component、@Controller、@Service、@Repository，它们是Spring的基本标签，在Spring Boot中并未改变它们原来的功能
2. @Import({User.class, DBHelper.class})给容器中自动创建出这两个类型的组件、**默认组件的名字就是全类名**
3. @Import注解一般放在有注解的类上

## 三、@Conditional

1. @Conditional简介

   - 条件装配：满足Conditional指定的条件，则进行组件注入
   - 使用范围：可以使用在类上，也可以使用在方法上，使用位置不同则作用范围不同

2. 派生类

   ![在这里插入图片描述](C:/Users/MC/AppData/TyporaImage/Springboot/20210205005453173.png)

3. 个别派生类使用举例

   ```java
   @Configuration(proxyBeanMethods = false)
   @ConditionalOnMissingBean(name = "tom")
   // @ConditionalOnMissingBean表示没有tom名字的Bean时，MyConfig类的Bean才能生效
   @ConditionalOnBean(name = "tom")
   // @ConditionalOnBean表示有tom名字的Bean时，MyConfig类的Bean才能生效
   public class MyConfig {
   
       @Bean
       public User user01(){
           User zhangsan = new User("zhangsan", 18);
           zhangsan.setPet(tomcatPet());
           return zhangsan;
       }
   
       @Bean("tom22")
       public Pet tomcatPet(){
           return new Pet("tomcat");
       }
   }
   
   ```


## 四、@ImportResource

1. 若之前的项目使用bean.xml文件生成配置bean，然而现在使用配置类来注入Bean，bean.xml还想用，那么就可以使用@ImportResource

2. bean.xml：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans ...">
   
       <bean id="haha" class="com.lun.boot.bean.User">
           <property name="name" value="zhangsan"></property>
           <property name="age" value="18"></property>
       </bean>
   
       <bean id="hehe" class="com.lun.boot.bean.Pet">
           <property name="name" value="tomcat"></property>
       </bean>
   </beans>
   ```

3. 使用方法

   ```java
   @ImportResource("classpath:beans.xml")
   public class MyConfig {
   ...
   }
   ```

## 五、@ConfigurationProperties

1. @ConfigurationProperties的作用是使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用

2. @ConfigurationProperties + @Component配合使用

   - 假设有配置文件application.properties

     ```properties
     mycar.brand=BYD
     mycar.price=100000
     ```

   - 只有在容器中的组件（任何组件都是如此），才会拥有SpringBoot提供的强大功能

     ```java
     @Component
     @ConfigurationProperties(prefix = "mycar")
     public class Car {
         
         private String brand;
         
         private String price;
         
     	...
     }
     ```

3. @EnableConfigurationProperties + @ConfigurationProperties配合使用

   - 开启Car配置绑定功能

   - 把这个Car这个组件自动注册到容器中

     ```java
     @EnableConfigurationProperties(Car.class)
     public class MyConfig {
     ...
     }
     
     
     @ConfigurationProperties(prefix = "mycar")
     public class Car {
         
         private String brand;
         
         private String price;
         
     	...
     }
     ```

# 五、自动配置源码分析

## 一、自动包规则原理

- SpringBoot应用的启动类

  ```java
  @SpringBootApplication
  public class MainApplication {
  
      public static void main(String[] args) {
          SpringApplication.run(MainApplication.class, args);
      }
  
  }
  ```

- 分析`@SpringBootApplication`

  ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @SpringBootConfiguration
  @EnableAutoConfiguration
  @ComponentScan(
      excludeFilters = {@Filter(
      type = FilterType.CUSTOM,
      classes = {TypeExcludeFilter.class}
  ), @Filter(
      type = FilterType.CUSTOM,
      classes = {AutoConfigurationExcludeFilter.class}
  )}
  )
  public @interface SpringBootApplication {
      ...
  }
  ```

- `@SpringBootApplication`重点在于`@SpringBootConfiguration`，`@EnableAutoConfiguration`，`@ComponentScan`

### 一、@SpringBootConfiguration

- `@Configuration`代表当前是一个配置类

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

### 二、@ComponentScan

- 指定扫描Spring注解

### 三、@EnableAutoConfiguration

- 意思为开启自动配置功能的注解
- 重点在于`@AutoConfigurationPackage`，`@Import(AutoConfigurationImportSelector.class)`

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

1. @AutoConfigurationPackage
   - 标签名直译为：自动配置包，指定了默认的包规则
   - 利用Registrar给容器中导入一系列组件
   - 将指定的一个包下的所有组件导入进MainApplication所在包下，也就是导入主程序所在包及子包下的所有组件
2. @Import(AutoConfigurationImportSelector.class)

## 二、初始加载自动配置类

- @Import(AutoConfigurationImportSelector.class)

1. `AutoConfigurationImportSelector.class`类利用`getAutoConfigurationEntry(annotationMetadata);`给容器中批量导入一些组件

2. 调用`List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)`获取到所有需要导入到容器中的配置类（组件）

3. 利用工厂加载 `Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader);`得到所有的组件

4. 从包中`META-INF/spring.factories`位置来加载一个文件

   - 默认扫描我们当前系统里面所有`META-INF/spring.factories`位置的文件
   - `spring-boot-autoconfigure-2.3.4.RELEASE.jar`包里面也有`META-INF/spring.factories`，而且还是主要的
   - 在`spring-boot-autoconfigure-2.3.4.RELEASE.jar/META-INF/spring.factories`虽然有127个场景的所有自动配置启动的时候默认全部加载，但是`xxxxAutoConfiguration`按照条件装配规则（`@Conditional`），最终会按需配置

   ![在这里插入图片描述](../../../TyporaImage/SpringBoot/20210205005536620.png)

   ```properties
   # 文件里面写死了spring-boot一启动就要给容器中加载的所有配置类
   # 如spring-boot-autoconfigure-2.3.4.RELEASE.jar/META-INF/spring.factories
   # Auto Configure
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
   org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
   org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
   ...
   ```

## 三、自动配置流程

- 以`DispatcherServletAutoConfiguration`的内部类`DispatcherServletConfiguration`为例子

  ```java
  @Bean
  @ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
  @ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字multipartResolver的组件，但是用户配置了文件上传的配置类，也会导入此配置类
  public MultipartResolver multipartResolver(MultipartResolver resolver) {
  	//给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找
  	//SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
  	return resolver;//给容器中加入了文件上传解析器；
  }
  ```

- SpringBoot默认会在底层配好所有的组件，但是**如果用户自己配置了以用户的优先**

- 自动配置总结

  - SpringBoot先加载所有的自动配置类`xxxxxAutoConfiguration`
  - 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值（xxxxProperties里面读取，xxxProperties和配置文件进行了绑定）
  - 生效的配置类就会给容器中装配很多组件
  - 只要容器中有这些组件，相当于这些功能就有了
  - 定制化配置
    - 用户直接自己@Bean替换底层的组件
    - 用户去看这个组件是获取的配置文件什么值就去修改配置文件中对应的值
  - **xxxxxAutoConfiguration ---> 组件 ---> xxxxProperties里面拿值  ----> application.properties**

# 六、SpringBoot最佳实践

## 一、SpringBoot应用如何编写

1. 引入场景依赖
   - [官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter)
2. 查看自动配置了哪些（选做）
   - 自己分析，引入场景对应的自动配置一般都生效了
   - 配置文件中debug=true开启自动配置报告
     - Negative（不生效的自动配置类）
     - Positive（生效的自动配置类）
3. 是否需要修改或者自定义配置文件
   - 参照文档修改配置项
     - [官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties)
     - 自己分析。xxxxProperties绑定了配置文件的哪些
   - 自定义加入或者替换组件
     - @Bean、@Component...
   - 自定义器XXXXXCustomizer
   - ......

## 二、Lombok简化开发

1. Lombok用标签方式代替构造器、getter/setter、toString()、hashCode()、equals()等鸡肋代码

2. Lombok的使用

   - 引入依赖

     ```xml
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
     </dependency>
     ```

   - IDEA中File->Settings->Plugins，搜索安装Lombok插件

3. 常用方法替代

   ```java
   @NoArgsConstructor
   @AllArgsConstructor
   // 如果是使用无参-全参之间的构造器则需要重写自己手写构造器
   @Data
   @ToString
   @EqualsAndHashCode
   public class User {
   
       private String name;
       private Integer age;
   
       private Pet pet;
   
       public User(String name,Integer age){
           this.name = name;
           this.age = age;
       }
   }
   ```

4. 简化日志开发：使用`@Slf4j`即可，在代码中使用`log.`即可

   ```java
   @Slf4j
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String handle01(@RequestParam("name") String name){
           log.info("请求进来了....");
           return "Hello, Spring Boot 2!"+"你好："+name;
       }
   }
   ```

## 三、dev-tools热更新

1. dev-tools并不是真正意义上的热更新，只是重启项目而已，但是修改页面后不需要重新启动，Java代码修改后还是要重启

2. 添加依赖

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <optional>true</optional>
       </dependency>
   </dependencies>
   ```

3. 使用方法：在IDEA中，项目或者页面修改以后使用按键Ctrl+F9，也就是rebuild

## 四、Spring Initailizr

- [Spring Initailizr](https://start.spring.io/)是创建SpringBoot工程向导
- 在IDEA中，菜单栏New -> Project -> Spring Initailizr，需要什么依赖选择什么依赖即可

# 七、配置文件

1. YAML的意思其实是："Yet Another Markup Language"（仍是一种标记语言），同以前的properties用法。当properties文件与YAML文件同时存在时，先加载properties文件中的内容，再加载YAML文件中的内容，当properties文件中的内容与YAML文件的内容有重复时，则遵循优先原则，使用properties文件中的内容

2. YAML**非常适合用来做以数据为中心的配置文件**

3. 基本语法

   - key: value；kv之间有空格
   - 大小写敏感
   - 使用缩进表示层级关系
   - 缩进不允许使用tab，只允许空格，在IDEA中允许使用tab
   - 缩进的空格数不重要，只要相同层级的元素左对齐即可
   - '#'表示注释
   - 字符串无需加引号，如果要加，单引号''、双引号""表示字符串内容会被转义、不转义。也就是说单引号会将转义字符`/`再转义成普通字符串，而双引号不会处理转义字符。例如`"张三\n李四"`，则会换行，使用单引号时不会换行

4. 数据类型

   - 字面量：单个的、不可再分的值。date、boolean、string、number、null

     ```yaml
     k: v
     ```

   - 对象：键值对的集合。map、hash、set、object 

     ```yaml
     #行内写法：  
     
     k: {k1:v1,k2:v2,k3:v3}
     
     #或
     
     k: 
       k1: v1
       k2: v2
       k3: v3
     ```

   - 数组：一组按次序排列的值。array、list、queue

     ```yaml
     #行内写法：  
     
     k: [v1,v2,v3]
     
     #或者
     
     k:
      - v1
      - v2
      - v3
     ```

5. 实例

   - Java代码

     ```java
     @Data
     @Component
     @ConfigurationProperties(prefix="person")
     public class Person {
         private String userName;
         private Boolean boss;
         private Date birth;
         private Integer age;
         private Pet pet;
         private String[] interests;
         private List<String> animal;
         private Map<String, Object> score;
         private Set<Double> salarys;
         private Map<String, List<Pet>> allPets;
     }
     
     @Data
     public class Pet {
         private String name;
         private Double weight;
     }
     ```

   - yaml代码

     ```yaml
     person:
       userName: zhangsan
       boss: false
       birth: 2019/12/12 20:12:33
       age: 18
       pet: 
         name: tomcat
         weight: 23.4
       interests: [篮球,游泳]
       animal: 
         - jerry
         - mario
       score:
         english: 
           first: 30
           second: 40
           third: 50
         math: [131,140,148]
         chinese: {first: 128,second: 136}
       salarys: [3999,4999.98,5999.99]
       allPets:
         sick:
           - {name: tom}
           - {name: jerry,weight: 47}
         health: [{name: mario,weight: 47}]
     ```

6. YAML取值

   - @value注解方式： 读取到的数据特别零散 

     ```java
     @RestController
     @RequestMapping("/person")
     public class PersonController {
         
         @Value("${person.userName}")
         private String userName;
      
         @GetMapping("/{id}")
         public String getById(@PathVariable Integer id){
             System.out.println(userName);
             return "hello";
         }
     }
     ```

   - Environment对象：SpringBoot可以使用`@Autowired`注解注入`Environment`对象的方式读取数据。这种方式 SpringBoot会将配置文件中所有的数据封装到`Environment`对象中，如果需要使用哪个数据只需要通过调用 `Environment`对象的`getProperty(String name)`方法获取

     ```java
     @RestController
     @RequestMapping("/person")
     public class PersonController {
         
         @Autowired
         private Environment env;
         
         @GetMapping("/{id}")
         public String getById(@PathVariable Integer id){
             System.out.println(env.getProperty("person.userName"));
             return "hello , spring boot!";
         }
     }
     ```

   - 对自定义数据类型：将实体类bean的创建交给Spring管理，在类上添加@Component注解；使用@ConfigurationProperties注解表示加载配置文件，在该注解中也可以使用prefix属性指定只加载指定前缀的数据 

     ```java
     @RestController
     @RequestMapping("/person")
     public class PersonController {
         
         @Autowired
         private Person person;
         
         @GetMapping("/{id}")
         public String getById(@PathVariable Integer id){
             System.out.println(person.getUserName);
             return "hello , spring boot!";
         }
     }
     ```

7. 配置文件-自定义类绑定的配置提示

   - 自定义的类和配置文件绑定一般没有提示。若要提示，添加如下依赖：

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-configuration-processor</artifactId>
         <optional>true</optional>
     </dependency>
     
     <!-- 下面插件作用是工程打包时，不将spring-boot-configuration-processor打进包内，让其只在编码的时候有用 -->
     <build>
         <plugins>
             <plugin>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-maven-plugin</artifactId>
                 <configuration>
                     <excludes>
                         <exclude>
                             <groupId>org.springframework.boot</groupId>
                             <artifactId>spring-boot-configuration-processor</artifactId>
                         </exclude>
                     </excludes>
                 </configuration>
             </plugin>
         </plugins>
     </build>
     ```

# 八、Web场景开发

## 一、web开发之简介

1. 大多场景我们都无需自定义配置

2. 自动配置的资源有

   - 内容协商视图解析器和BeanName视图解析器
   - 静态资源（包括webjars）
   - 自动注册 `Converter，GenericConverter，Formatter `
   - 支持 `HttpMessageConverters` （配合内容协商理解原理）
   - 自动注册 `MessageCodesResolver` （国际化用）
   - 静态index.html 页支持
   - 自定义 `Favicon`  
   - 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）

   > If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.
   >
   > **不用@EnableWebMvc注解。使用** **`@Configuration`** **+** **`WebMvcConfigurer`** **自定义规则**

   > If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.
   >
   > **声明** **`WebMvcRegistrations`** **改变默认底层组件**

   > If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.
   >
   > **使用** **`@EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC`**

## 二、web开发之静态资源规则与定制化

1. 静态资源目录

   - 只要静态资源放在类路径下： 也就是`resources`目录下`/static` or `/public` or `/resources` or `/META-INF/resources`

   - 访问：当前项目根路径/ + 静态资源名 

   - 原理：静态映射/**

   - 请求进来，先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面

   - 也可以改变默认的静态资源路径，如果改变默认的资源路径，那么`/static`，`/public`,`/resources`, `/META-INF/resources`则会失效

     ```yaml
     resources:
       static-locations: [classpath:/abc/]
     ```

2. 静态资源访问前缀

   - 静态资源默认访问前缀为无，访问路径为：当前项目根路径/ + 静态资源名 

   - 设置前缀后的访问路径为：当前项目 + static-path-pattern + 静态资源名 = 静态资源文件夹下找

     ```yaml
     spring:
       mvc:
         static-path-pattern: /res/**
     ```

3. webjar

   - 可用jar方式添加css，js等资源文件

   - [官网](https://www.webjars.org/)

   - 例如，添加jquery

     ```java
     <dependency>
         <groupId>org.webjars</groupId>
         <artifactId>jquery</artifactId>
         <version>3.5.1</version>
     </dependency>
     ```

   - 访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)  后面地址要按照依赖里面的包路径

## 三、web开发之welcome与favicon功能

- [官方文档](https://docs.spring.io/spring-boot/docs/2.3.8.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-welcome-page)

1. 欢迎页支持

   - 使用controller能处理/index，进行跳转页面

   - 静态资源路径下index.html

     - 可以配置静态资源路径
     - 但是不可以配置静态资源的访问前缀，否则导致index.html不能被默认访问，最新版SpringBoot可能解决了这个bug

     ```yaml
     spring:
     #  mvc:
     #    static-path-pattern: /res/**   这个会导致welcome page功能失效
       resources:
         static-locations: [classpath:/anc/]
     ```

2. 自定义favicon

   - 指网页标签上的小图标
   - favicon.ico放在静态资源目录下即可，必须是favicon.ico这个名字

   ```properties
   spring:
   #  mvc:
   #    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
   ```

## 四、web开发之静态资源原理源码分析

1. 静态资源原理大纲代码

   - SpringBoot启动默认加载自动配置类` xxxAutoConfiguration类`

   - SpringMVC功能的自动配置类`WebMvcAutoConfiguration`，生效

     ```java
     @Configuration(proxyBeanMethods = false)
     @ConditionalOnWebApplication(type = Type.SERVLET)
     @ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
     @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
     @AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
     @AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
     		ValidationAutoConfiguration.class })
     public class WebMvcAutoConfiguration {
         ...
     }
     ```

   - 容器中配置文件的相关属性的绑定：WebMvcProperties（spring.mvc）、ResourceProperties（spring.resources）

     ```java
     @Configuration(proxyBeanMethods = false)
     @Import(EnableWebMvcConfiguration.class)
     @EnableConfigurationProperties({ WebMvcProperties.class, ResourceProperties.class })
     @Order(0)
     public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {
         ...
     }
     ```

2. `WebMvcAutoConfigurationAdapter`配置类只有一个有参构造器

   ```java
   ////有参构造器所有参数的值都会从容器中确定
   public WebMvcAutoConfigurationAdapter(WebProperties webProperties, WebMvcProperties mvcProperties,
   		ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider,
   		ObjectProvider<ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
   		ObjectProvider<DispatcherServletPath> dispatcherServletPath,
   		ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
   	this.mvcProperties = mvcProperties;
   	this.beanFactory = beanFactory;
   	this.messageConvertersProvider = messageConvertersProvider;
   	this.resourceHandlerRegistrationCustomizer = resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
   	this.dispatcherServletPath = dispatcherServletPath;
   	this.servletRegistrations = servletRegistrations;
   	this.mvcProperties.checkConfiguration();
   }
   ```

   - ResourceProperties resourceProperties：获取和spring.resources绑定的所有的值的对象
   - WebMvcProperties mvcProperties：获取和spring.mvc绑定的所有的值的对象
   - ListableBeanFactory beanFactory：Spring的beanFactory
   - HttpMessageConverters：找到所有的HttpMessageConverters
   - ResourceHandlerRegistrationCustomizer：找到资源处理器的自定义器
   - DispatcherServletPath：找到DispatcherServlet的路径
   - ServletRegistrationBean：给应用注册Servlet、Filter....

3. 资源处理的默认规则

   ```java
   ...
   public class WebMvcAutoConfiguration {
       ...
   	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
           ...
   		@Override
   		protected void addResourceHandlers(ResourceHandlerRegistry registry) {
   			super.addResourceHandlers(registry);
   			if (!this.resourceProperties.isAddMappings()) {
   				logger.debug("Default resource handling disabled");
   				return;
   			}
   			ServletContext servletContext = getServletContext();
   			addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
   			addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
   				registration.addResourceLocations(this.resourceProperties.getStaticLocations());
   				if (servletContext != null) {
   					registration.addResourceLocations(new ServletContextResource(servletContext, SERVLET_LOCATION));
   				}
   			});
   		}
           ...
           
       }
       ...
   }
   ```

   - 根据上述代码，我们可以同过配置禁止所有静态资源规则

     ```yaml
     spring:
       resources:
         add-mappings: false   #禁用所有静态资源规则
     ```

   - 静态资源规则：

     ```java
     @ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
     public class ResourceProperties {
     
         private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
                 "classpath:/resources/", "classpath:/static/", "classpath:/public/" };
     
         /**
          * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
          * /resources/, /static/, /public/].
          */
         private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
         ...
     }
     
     ```

   - 欢迎页的处理规则

     ```java
     ...
     public class WebMvcAutoConfiguration {
         ...
     	public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
             ...
     		@Bean
     		public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
     				FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
     			WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(
     					new TemplateAvailabilityProviders(applicationContext), applicationContext, getWelcomePage(),
     					this.mvcProperties.getStaticPathPattern());
     			welcomePageHandlerMapping.setInterceptors(getInterceptors(mvcConversionService, mvcResourceUrlProvider));
     			welcomePageHandlerMapping.setCorsConfigurations(getCorsConfigurations());
     			return welcomePageHandlerMapping;
     		}
         
     ```

   - `WelcomePageHandlerMapping`的构造方法如下：

     ```java
     WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders,
                               ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
         if (welcomePage != null && "/**".equals(staticPathPattern)) {
             //要用欢迎页功能，必须是/**
             logger.info("Adding welcome page: " + welcomePage);
             setRootViewName("forward:index.html");
         }
         else if (welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
             //调用Controller /index
             logger.info("Adding welcome page template: index");
             setRootViewName("index");
         }
         // 这构造方法内的代码也解释了配置`static-path-pattern`，welcome页面和小图标失效的问题
     }
     ```

## 五、请求处理之Rest映射及源码解析

1. 请求映射

   - @xxxMapping：@GetMapping、@PostMapping、@PutMapping、@DeleteMapping

   - Rest风格支持（使用**HTTP**请求方式动词来表示对资源的操作）

     - GET-获取用户
     - DELETE-删除用户
     - PUT-修改用户
     - POST-保存用户

     - 核心Filter：HiddenHttpMethodFilter

2. Rest用法

   - 开启页面表单的Rest功能
   - 页面form的属性method=post，隐藏域 \_method=put、delete等（如果直接get或post，无需隐藏域）
   - 编写请求映射

   ```yaml
   spring:
     mvc:
       hiddenmethod:
         filter:
           enabled: true   #开启页面表单的Rest功能
   ```

   ```html
   <form action="/user" method="get">
       <input value="REST-GET提交" type="submit" />
   </form>
   
   <form action="/user" method="post">
       <input value="REST-POST提交" type="submit" />
   </form>
   
   <form action="/user" method="post">
       <input name="_method" type="hidden" value="DELETE"/>
       <input value="REST-DELETE 提交" type="submit"/>
   </form>
   
   <form action="/user" method="post">
       <input name="_method" type="hidden" value="PUT" />
       <input value="REST-PUT提交"type="submit" />
   <form>
   ```

   ```java
   @GetMapping("/user")
   //@RequestMapping(value = "/user",method = RequestMethod.GET)
   public String getUser(){
       return "GET-张三";
   }
   
   @PostMapping("/user")
   //@RequestMapping(value = "/user",method = RequestMethod.POST)
   public String saveUser(){
       return "POST-张三";
   }
   
   @PutMapping("/user")
   //@RequestMapping(value = "/user",method = RequestMethod.PUT)
   public String putUser(){
       return "PUT-张三";
   }
   
   @DeleteMapping("/user")
   //@RequestMapping(value = "/user",method = RequestMethod.DELETE)
   public String deleteUser(){
       return "DELETE-张三";
   }
   ```

3. Rest原理（表单提交要使用REST的时候）

   - 表单提交会带上`\_method=PUT`
   - 请求过来被`HiddenHttpMethodFilter`拦截，如果请求是正常的，并且是POST请求
     - 获取到`\_method`的值
     - 兼容的请求有：PUT、DELETE、PATCH
     - 原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值
     - 过滤器链放行的时候用wrapper。以后的方法调用getMethod是调用requesWrapper的getMethod

   ```java
   public class HiddenHttpMethodFilter extends OncePerRequestFilter {
   
   	private static final List<String> ALLOWED_METHODS =
   			Collections.unmodifiableList(Arrays.asList(HttpMethod.PUT.name(),
   					HttpMethod.DELETE.name(), HttpMethod.PATCH.name()));
   
   	/** Default method parameter: {@code _method}. */
   	public static final String DEFAULT_METHOD_PARAM = "_method";
   
   	private String methodParam = DEFAULT_METHOD_PARAM;
   
   
   	/**
   	 * Set the parameter name to look for HTTP methods.
   	 * @see #DEFAULT_METHOD_PARAM
   	 */
   	public void setMethodParam(String methodParam) {
   		Assert.hasText(methodParam, "'methodParam' must not be empty");
   		this.methodParam = methodParam;
   	}
   
   	@Override
   	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
   			throws ServletException, IOException {
   
   		HttpServletRequest requestToUse = request;
   
   		if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
   			String paramValue = request.getParameter(this.methodParam);
   			if (StringUtils.hasLength(paramValue)) {
   				String method = paramValue.toUpperCase(Locale.ENGLISH);
   				if (ALLOWED_METHODS.contains(method)) {
   					requestToUse = new HttpMethodRequestWrapper(request, method);
   				}
   			}
   		}
   
   		filterChain.doFilter(requestToUse, response);
   	}
   
   
   	/**
   	 * Simple {@link HttpServletRequest} wrapper that returns the supplied method for
   	 * {@link HttpServletRequest#getMethod()}.
   	 */
   	private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {
   
   		private final String method;
   
   		public HttpMethodRequestWrapper(HttpServletRequest request, String method) {
   			super(request);
   			this.method = method;
   		}
   
   		@Override
   		public String getMethod() {
   			return this.method;
   		}
   	}
   }
   ```

4. Rest使用客户端工具：如PostMan可直接发送put、delete等方式请求

## 六、请求处理之改变默认的\_method

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
		ValidationAutoConfiguration.class })
public class WebMvcAutoConfiguration {

    ...
    
    @Bean
    @ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
    @ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
    public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
        return new OrderedHiddenHttpMethodFilter();
    }
    ...
}
```

- `@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)`意味着在没有`HiddenHttpMethodFilter`时，才执行`hiddenHttpMethodFilter()`。因此，我们可以自定义filter，改变默认的`\_method`。例如：

```java
@Configuration(proxyBeanMethods = false)
public class WebConfig{
    //自定义filter
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
        methodFilter.setMethodParam("_m");
        return methodFilter;
    }    
}
```

- 将`\_method`改成`_m`

```java
<form action="/user" method="post">
    <input name="_m" type="hidden" value="DELETE"/>
    <input value="REST-DELETE 提交" type="submit"/>
</form>
```

## 七、请求处理之请求映射原理

![在这里插入图片描述](../../../TyporaImage/SpringBoot/20210205005703527.png)

1. SpringMVC功能分析都从 `org.springframework.web.servlet.DispatcherServlet` ->  `doDispatch() `

   ```java
   protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
       HttpServletRequest processedRequest = request;
       HandlerExecutionChain mappedHandler = null;
       boolean multipartRequestParsed = false;
   
       WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);
   
       try {
           ModelAndView mv = null;
           Exception dispatchException = null;
   
           try {
               processedRequest = checkMultipart(request);
               multipartRequestParsed = (processedRequest != request);
   
               // 找到当前请求使用哪个Handler（Controller的方法）处理
               mappedHandler = getHandler(processedRequest);
   
               //HandlerMapping：处理器映射。/xxx->>xxxx
       ...
   }
           
   
   // getHandler()方法如下：
   @Nullable
   protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
       if (this.handlerMappings != null) {
           for (HandlerMapping mapping : this.handlerMappings) {
               HandlerExecutionChain handler = mapping.getHandler(request);
               if (handler != null) {
                   return handler;
               }
           }
       }
       return null;
   }
   ```

2. `getHandler()`中`this.handlerMappings`在Debug模式展现的内容如下，其中保存了所有`@RequestMapping` 和`handler`的映射规则

![在这里插入图片描述](../../../TyporaImage/20210205005802305.png)

![在这里插入图片描述](../../../TyporaImage/20210205005926474.png)

3. 所有的请求映射都在HandlerMapping中：
   - SpringBoot自动配置欢迎页的WelcomePageHandlerMapping，访问`/`能访问到index.html
   - SpringBoot自动配置了默认的RequestMappingHandlerMapping
   - 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息

     - 如果有就找到这个请求对应的handler
     - 如果没有就是下一个HandlerMapping
   - 我们需要一些自定义的映射处理，我们也可以自己给容器中放HandlerMapping，自定义HandlerMapping也就是注入`@Controller`

## 八、请求处理之常用参数注解使用

1. 注解

   - `@PathVariable`：路径变量
   - `@RequestHeader`：获取请求头
   - `@RequestParam`：获取请求参数（指问号后的参数，url?a=1&b=2）
   - `@CookieValue`：获取Cookie值
   - `@RequestAttribute`：获取request域属性
   - `@RequestBody`：获取请求体[POST]
   - `@MatrixVariable`：矩阵变量
   - `@ModelAttribute`

2. 使用用例

   ```java
   @RestController
   public class ParameterTestController {
   
   
       //  car/2/owner/zhangsan
       @GetMapping("/car/{id}/owner/{username}")
       public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                        @PathVariable("username") String name,
                                        @PathVariable Map<String,String> pv,
                                        
                                        @RequestHeader("User-Agent") String userAgent,
                                        @RequestHeader Map<String,String> header,
                                        
                                        @RequestParam("age") Integer age,
                                        @RequestParam("inters") List<String> inters,
                                        @RequestParam Map<String,String> params,
                                        
                                        @CookieValue("_ga") String _ga,
                                        @CookieValue("_ga") Cookie cookie){
   
           Map<String,Object> map = new HashMap<>();
   
   //        map.put("id",id);
   //        map.put("name",name);
   //        map.put("pv",pv);
   //        map.put("userAgent",userAgent);
   //        map.put("headers",header);
           map.put("age",age);
           map.put("inters",inters);
           map.put("params",params);
           map.put("_ga",_ga);
           System.out.println(cookie.getName()+"===>"+cookie.getValue());
           return map;
       }
   
   
       @PostMapping("/save")
       public Map postMethod(@RequestBody String content){
           Map<String,Object> map = new HashMap<>();
           map.put("content",content);
           return map;
       }
   }
   ```

## 九、请求处理之@RequestAttribute

```java
@Controller
public class RequestController {

    @GetMapping("/goto")
    public String goToPage(HttpServletRequest request){

        request.setAttribute("msg","成功了...");
        request.setAttribute("code",200);
        return "forward:/success";  //转发到  /success请求
    }

    @GetMapping("/params")
    public String testParam(Map<String,Object> map,
                            Model model,
                            HttpServletRequest request,
                            HttpServletResponse response){
        map.put("hello","world666");
        model.addAttribute("world","hello666");
        request.setAttribute("message","HelloWorld");

        Cookie cookie = new Cookie("c1","v1");
        response.addCookie(cookie);
        return "forward:/success";
    }

    ///<-----------------主角@RequestAttribute在这个方法
    @ResponseBody
    @GetMapping("/success")
    public Map success(@RequestAttribute(value = "msg",required = false) String msg,
                       @RequestAttribute(value = "code",required = false)Integer code,
                       HttpServletRequest request){
        Object msg1 = request.getAttribute("msg");

        Map<String,Object> map = new HashMap<>();
        Object hello = request.getAttribute("hello");
        Object world = request.getAttribute("world");
        Object message = request.getAttribute("message");

        map.put("reqMethod_msg",msg1);
        map.put("annotation_msg",msg);
        map.put("hello",hello);
        map.put("world",world);
        map.put("message",message);

        return map;
    }
}
```

## 十、请求处理之@MatrixVariable

1. `@MatrixVariable`语法：请求路径如`/cars/sell;low=34;brand=byd,audi,yd`。 矩阵变量是在URI路径的片段中以 `path/to;key=value` 形式传递的参数 

2. SpringBoot默认是禁用了矩阵变量的功能

   - 手动开启：原理。对于路径的处理。UrlPathHelper的removeSemicolonContent设置为false，让其支持矩阵变量的。

3. 矩阵变量**必须**有url路径变量才能被解析

4. 手动开启矩阵变量

   - 方式一：实现`WebMvcConfigurer`接口

     ```java
     @Configuration(proxyBeanMethods = false)
     public class WebConfig implements WebMvcConfigurer {
         @Override
         public void configurePathMatch(PathMatchConfigurer configurer) {
     
             UrlPathHelper urlPathHelper = new UrlPathHelper();
             // 不移除；后面的内容。矩阵变量功能就可以生效
             urlPathHelper.setRemoveSemicolonContent(false);
             configurer.setUrlPathHelper(urlPathHelper);
         }
     }
     ```

   - 方式二：创建返回`WebMvcConfigurer`Bean

     ```java
     @Configuration(proxyBeanMethods = false)
     public class WebConfig{
         @Bean
         public WebMvcConfigurer webMvcConfigurer(){
             return new WebMvcConfigurer() {
                             @Override
                 public void configurePathMatch(PathMatchConfigurer configurer) {
                     UrlPathHelper urlPathHelper = new UrlPathHelper();
                     // 不移除；后面的内容。矩阵变量功能就可以生效
                     urlPathHelper.setRemoveSemicolonContent(false);
                     configurer.setUrlPathHelper(urlPathHelper);
                 }
             }
         }
     }
     ```

5. `@MatrixVariable`的用例

   ```java
   @RestController
   public class ParameterTestController {
   
       // /cars/sell;low=34;brand=byd,audi,yd;key1=value1;key2=value2
       @GetMapping("/cars/{path}")
       public Map carsSell(@MatrixVariable("low") Integer low,
                           @MatrixVariable("brand") List<String> brand,
                           @PathVariable("path") String path,
                           @PathVariable("matrixVars") Map matrixVars){
           Map<String,Object> map = new HashMap<>();
   
           map.put("low",low);
           map.put("brand",brand);
           map.put("path",path);
           return map;
       }
   
       // 若在一次请求中，请求中的矩阵变量相同取值方式
       // /boss/1;age=40/2;age=45
       // 此处的1和2分别代表{bossId}和{empId}，和参数本身并无关系，1和2后面分别跟着的才是参数值
       @GetMapping("/boss/{bossId}/{empId}")
       public Map boss(@MatrixVariable(value = "age",pathVar = "bossId") Integer bossAge,
                       @MatrixVariable(value = "age",pathVar = "empId") Integer empAge){
           Map<String,Object> map = new HashMap<>();
   
           map.put("bossAge",bossAge);
           map.put("empAge",empAge);
           return map;
   
       }
   }
   ```

## 十一、请求处理之各种类型参数解析原理