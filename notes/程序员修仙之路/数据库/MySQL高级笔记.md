# 一、Linux下安装MySQL

## 一、查看是否安装过MySQL

1. 如果是用rpm安装，检查一下RPM PACKAGE

   ```mysql
   rpm -qa | grep -i mysql # -i 忽略大小写
   ```

2. 检查mysql service

   ```mysql
   systemctl status mysqld.service
   ```

3. 如果存在mysql-libs的旧版本，则显示旧版本的信息

4. 如果不存在mysql-libs的旧版本，则不显示任何内容

## 二、MySQL的卸载

1. 关闭MySQL服务

   ```mysql
   systemctl stop mysqld.service
   ```

2. 查看当前mysql安装状况

   ```mysql
   rpm -qa | grep -i mysql
   # 或
   yum list installed | grep mysql
   ```

3. 卸载上述命令查询出的已安装程序

   ```mysql
   yum remove mysql-xxx mysql-xxx mysql-xxx mysqk-xxxx
   #务必卸载干净，反复执行 rpm -qa | grep -i mysql 确认是否有卸载残留
   ```

4. 删除mysql相关文件

   ```mysql
   #查看相关文件
   find / -name mysql
   #删除上述命令查找出的相关文件
   rm -rf xx
   ```

5. 删除my.cnf

   ```mysql
   rm -rf /etc/my.cnf
   ```

## 三、MySQL的Linux版安装

### 一、MySQL的四大版本

```mysql
1、MySQL Community Server 社区版本，开源免费，自由下载，但不提供官方技术支持，适用于大多数普通用户。
2、MySQL Enterprise Edition 企业版本，需付费，不能在线下载，可以试用30天。提供了更多的功能和更完备的技术支持，更适合于对数据库的功能和可靠性要求较高的企业客户。
3、MySQL Cluster 集群版，开源免费。用于架设集群服务器，可将几个MySQL Server封装成一个Server。需要在社区版或企业版的基础上使用。
4、MySQL Cluster CGE 高级集群版，需付费
5、此外，官方还提供了 MySQL Workbench （GUITOOL）一款专为MySQL设计的 ER/数据库建模工具 。它是著名的数据库设计工具DBDesigner4的继任者。MySQLWorkbench又分为两个版本，分别是社区版（MySQL Workbench OSS）、商用版 （MySQL WorkbenchSE）
```

### 二、下载MySQL指定版本

1. 下载地址

   - [MySQL官网](https://www.mysql.com/)

2. 打开官网，点击DOWNLOADS

   - 点击最下方的MySQL Community(GPL) Downloads

3. 点击MySQL Community Server

4. 在General Availability(GA) Releases中选择适合的版本

   - 如果安装Windows系统下MySQL ，推荐下载MSI安装程序；点击 Go to Download Page 进行下载即可
   - Windows下的MySQL安装有两种安装程序：mysql-installer-web-community-8.0.25.0.msi，下载程序大小：2.4M，安装时需要联网安装组件；mysql-installer-community-8.0.25.0.msi，下载程序大小：435.7M，安装时离线安装即可。推荐

5. Linux系统下安装MySQL的几种方式

   - Linux系统下安装软件的常用三种方式

     ```mysql
     #方式1：rpm命令。使用rpm命令安装扩展名为".rpm"的软件包
     #方式2：yum命令。需联网，从互联网获取的yum源，直接使用yum命令安装
     #方式3：编译安装源码包。针对tar.gz这样的压缩格式，要用tar命令来解压；如果是其它压缩格式，就使用其它命令
     ```

   - Linux系统下安装MySQL，官方给出多种安装方式

     |    安装方式    |                         特点                         |
     | :------------: | :--------------------------------------------------: |
     |      rpm       |      安装简单，灵活性差，无法灵活选择版本、升级      |
     | rpm repository | 安装包极小，版本安装简单灵活，升级方便，需要联网安装 |
     |  通用二进制包  |         安装比较复杂，灵活性高，平台通用性好         |
     |     源码包     |       安装最复杂，时间长，参数设置灵活，性能好       |

     ```mysql
     #这里不能直接选择CentOS 7系统的版本，所以选择与之对应的 Red Hat Enterprise Linux
     #https://downloads.mysql.com/archives/community/ 直接点Download下载RPM Bundle全量包。包括了所有下面的组件。不需要一个一个下载了
     ```

6. 下载的tar包，用压缩工具打开，把下图中红框的包在Linux下分别安装即可

   ![Linux下MySQL的安装包](../../../TyporaImage/Linux下MySQ安装包.png)

### 三、CentOS7下检查MySQL依赖

1. 检查/tmp临时目录的权限（必不可少）

   ```mysql
   #由于mysql安装过程中，会通过mysql用户在/tmp目录下新建tmp_db文件，所以请给/tmp较大的权限
   chmod -R 777 /tmp
   ```

2. 安装前，检查依赖

   ```mysql
   rpm -qa|grep libaio
   
   #如果存在libaio包，则执行
   rpm -qa|grep net-tools
   
   #如果存在net-tools包，则执行
   rpm -qa|grep net-tools
   
   #如果不存在需要到centos安装盘里进行rpm安装。安装linux如果带图形化界面，这些都是安装好的
   ```

### 四、CentOS7下MySQL安装过程

1. 将安装程序拷贝到/opt目录下，在mysql的安装文件目录下执行（必须按照以下顺序执行）

   ```mysql
   rpm -ivh mysql-community-common-xxxx.rpm
   rpm -ivh mysql-community-client-plugins-xxxx.rpm
   rpm -ivh mysql-community-libs-xxxx.rpm
   rpm -ivh mysql-community-client-xxxx.rpm
   rpm -ivh mysql-community-server-xxxx.rpm
   ```

   - 如在检查工作时，没有检查mysql依赖环境在安装mysql-community-server会报错
   - rpm是Redhat Package Manage缩写，通过RPM的管理，用户可以把源代码包装成以rpm为扩展名的文件形式，易于安装
   - -i, --install 安装软件包 
   - -v, --verbose 提供更多的详细信息输出
   - -h, --hash 软件包安装的时候列出哈希标记 (和 -v 一起使用效果更好)，展示进度条 

2. 安装过程可能会报的错

   ```mysql
   mariadn-libs被mysql-community-libs-xxxx.rpm取代
   则使用命令yum remove mysql-libs 解决，清除之前安装过的依赖即可
   ```

3. 查看MySQL版本

   ```mysql
   #执行如下命令，如果成功表示安装mysql成功。类似java -version如果打出版本等信息
   mysql --version
   #或
   mysqladmin --version
   
   #执行如下命令，查看是否安装成功。需要增加 -i 不用去区分大小写，否则搜索不到
   rpm -qa|grep -i mysql
   ```

4. 服务的初始化

   ```mysql
   #为了保证数据库目录与文件的所有者为 mysql 登录用户，如果你是以 root 身份运行 mysql 服务，需要执行下面的命令初始化：
   mysqld --initialize --user=mysql
   #说明： --initialize 选项默认以“安全”模式来初始化，则会为 root 用户生成一个密码并将 该密码标记为过期，登录后你需要设置一个新的密码。生成的 临时密码 会往日志中记录一份
   
   #查看密码：
   cat /var/log/mysqld.log
   #root@localhost: 后面就是初始化的密码
   ```

5. 启动MySQL，查看状态

   ```mysql
   #加不加.service后缀都可以
   启动：systemctl start mysqld.service
   关闭：systemctl stop mysqld.service
   重启：systemctl restart mysqld.service
   查看状态：systemctl status mysqld.service
   #mysqld 这个可执行文件就代表着 MySQL 服务器程序，运行这个可执行文件就可以直接启动一个服务器进程
   
   #查看进程
   ps -ef | grep -i mysql
   ```

6. 查看MySQL服务是否自启动

   ```mysql
   systemctl list-unit-files|grep mysqld.service
   #默认是enabled
   
   #如不是enabled可以运行如下命令设置自启动
   systemctl enable mysqld.service
   #如果希望不进行自启动，运行如下命令设置
   systemctl disable mysqld.service
   ```

## 四、MySQL登录

### 一、首次登录

- 通过 mysql -hlocalhost -P3306 -uroot -p 进行登录，在Enter password：录入初始化密码

### 二、修改密码

- 因为初始化密码默认是过期的，所以查看数据库会报错 

- 修改密码

  ```mysql
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
  ```

- 5.7版本之后（不含5.7），mysql加入了全新的密码安全机制。设置新密码太简单会报错

- 改为更复杂的密码规则之后，设置成功，可以正常使用数据库了

### 三、设置远程登录

#### 一、当前问题

在用SQLyog或Navicat中配置远程连接Mysql数据库时遇到如下报错信息，这是由于Mysql配置了不支持远程连接引起的

```mysql
Host 'xxxx' is not allowed to connect to this MySQL server
```

#### 二、确认网络

1. 在远程机器上使用ping ip地址 保证网络畅通

2. 在远程机器上使用telnet命令保证端口号开放访问

   ```mysql
   telnet ip地址 端口号
   ```

3. 拓展： telnet命令开启 

   - 在Windows系统中找到控制面板 -> 程序和功能 -> 启动或关闭Windows功能
   - 找到Telnet客户端，勾上即可

#### 三、关闭防火墙或开放端口

1. 关闭防火墙

   - CentOS6

     ```mysql
     service iptables stop
     ```

   - CentOS7

     ```mysql
     systemctl start firewalld.service
     systemctl status firewalld.service
     systemctl stop firewalld.service
     #设置开机启用防火墙
     systemctl enable firewalld.service
     #设置开机禁用防火墙
     systemctl disable firewalld.service
     ```

2. 开放端口

   - 查看开放的端口号

     ```mysql
     firewall-cmd --list-all
     ```

   - 设置开放的端口号

     ```mysql
     firewall-cmd --add-service=http --permanent
     firewall-cmd --add-port=3306/tcp --permanent
     ```

   - 重启防火墙

     ```mysql
     firewall-cmd --reload
     ```

#### 四、Linux下修改配置

在Linux系统MySQL下测试

```mysql
use mysql;
select Host,User from user;
#可以看到root用户的当前主机配置信息为localhost

update user set host = '%' where user ='root';
#Host设置了“%”后便可以允许远程访问

flush privileges;
#Host修改完成后记得执行flush privileges使配置立即生效：
```

1. 修改Host为通配符%
2. Host列指定了允许用户登录所使用的IP，比如user=root Host=192.168.1.1。这里的意思就是说root用户只能通过192.168.1.1的客户端去访问。 user=root Host=localhost，表示只能通过本机客户端去访问。而 % 是个通配符，如果Host=192.168.1.%，那么就表示只要是IP地址前缀为“192.168.1.”的客户端都可以连接。如果 Host=%，表示所有IP都有连接权限 
3. 在生产环境下不能为了省事将host设置为%，这样做会存在安全问题，具体的设置可以根据生产环境的IP进行设置

#### 五、其他问题

如果是MySQL8版本中，连接时还可能出现如下情况

```mysql
Plugin caching_sha2_password could not be loaded:乱码
#配置新连接报错：错误号码 2058，分析是 mysql 密码加密方法变了

#解决方法：Linux下 mysql -u root -p 登录你的 mysql 数据库，然后执行这条SQL
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'new_password';
#其中mysql_native_password是MySQL数据库中一种身份验证插件，用于对用户进行身份验证
#然后在重新配置SQLyog的连接，则可连接成功了
```

### 四、MySQL8密码强度评估

#### 一、MySQL8之前安全策略

在MySQL 8.0之前，MySQL使用的是validate_password插件检测、验证账号密码强度，保障账号的安全性

1. 安装/启用插件方式1：在参数文件my.cnf中添加参数

   ```mysql
   [mysqld]
   plugin-load-add=validate_password.so
   \#ON/OFF/FORCE/FORCE_PLUS_PERMANENT: 是否使用该插件(及强制/永久强制使用)
   validate-password=FORCE_PLUS_PERMANENT
   
   #说明1： plugin library中的validate_password文件名的后缀名根据平台不同有所差异。 对于Unix和Unix-like系统而言，它的文件后缀名是.so，对于Windows系统而言，它的文件后缀名是.dll。
   #说明2： 修改参数后必须重启MySQL服务才能生效。
   #说明3： 参数FORCE_PLUS_PERMANENT是为了防止插件在MySQL运行时的时候被卸载。当你卸载插件时就会报错
   ```

2. 安装/启用插件方式2：运行时命令安装（推荐）

   ```mysql
   INSTALL PLUGIN validate_password SONAME 'validate_password.so'
   #此方法也会注册到元数据，也就是mysql.plugin表中，所以不用担心MySQL重启后插件会失效
   ```

#### 二、MySQL8安全策略

1. validate_password说明

   - MySQL 8.0，引入了服务器组件（Components）这个特性，validate_password插件已用服务器组件重新实现。8.0.25版本的数据库中，默认自动安装validate_password组件

   - 安装插件前后对比效果

     ```mysql
     show variables like 'validate_password%';
     SELECT * FROM mysql.component;
     #未安装插件前，执行这两个语句均无内容输出；安装插件后，执行这两个语句有内容输出
     #组件和插件的默认值可能有所不同。例如，MySQL 5.7. validate_password_check_user_name的默认值为OFF
     ```

   - 关于 validate_password 组件对应的系统变量说明

     |                 选项                 | 默认值 |                           参数描述                           |
     | :----------------------------------: | :----: | :----------------------------------------------------------: |
     |  validate_password_check_user_name   |   ON   |          设置为ON的时候表示能将密码设置成当前用户名          |
     |  validate_password_dictionary_file   |        |           用于检查密码的字典文件的路径名，默认为空           |
     |       validate_password_length       |   8    |       密码的最小长度，也就是说密码长度必须大于或等于8        |
     |  validate_password_mixed_case_count  |   1    | 如果密码策略是中等或更强的，validate_password要求密码具有的小写和大写字符的最小数量。对于给定的这个值密码必须有那么多小写字符和那么多大写字符 |
     |    validate_password_number_count    |   1    |                    密码必须包含的数字个数                    |
     |       validate_password_policy       | MEDIUM | 密码强度检验等级，可以使用数值0、1、2或相应的符号值LOW、MEDIUM、STRONG来指定。0/LOW：只检查长度；1/MEDIUM：检查长度、数字、大小写、特殊字符；2/STRONG ：检查长度、数字、大小写、特殊字符、字典文件 |
     | validate_password_special_char_count |   1    |                  密码必须包含的特殊字符个数                  |

2. 修改安全策略

   ```mysql
   SET GLOBAL validate_password_policy=LOW;
   SET GLOBAL validate_password_policy=MEDIUM;
   SET GLOBAL validate_password_policy=STRONG;
   SET GLOBAL validate_password_policy=0; # For LOW
   SET GLOBAL validate_password_policy=1; # For MEDIUM
   SET GLOBAL validate_password_policy=2; # For HIGH
   #注意，如果是插件的话,SQL为set global validate_password_policy=LOW
   set global validate_password_length=1;
   #可以修改密码中字符的长度
   ```

3. 密码强度测试

   如果你创建密码是遇到“Your password does not satisfy the current policy requirements”，可以通过函数组件去检测密码是否满足条件： 0-100。当评估在100时就是说明使用上了最基本的规则：大写+小写+特殊字符+数字组成的8位以上密码 

   ```mysql
   SELECT VALIDATE_PASSWORD_STRENGTH('aaaaaa');
   SELECT VALIDATE_PASSWORD_STRENGTH('K354*45jKd5');
   #如果没有安装validate_password组件或插件的话，那么这个函数永远都返回0。 关于密码复杂度对应的密码复杂度策略
   ```

#### 三、卸载插件、组件

```mysql
#卸载插件
UNINSTALL PLUGIN validate_password;

#卸载组件
UNINSTALL COMPONENT 'file://component_validate_password';
```

## 五、字符集相关操作

### 一、修改MySQL5.7字符集

#### 一、修改步骤

在MySQL 8.0版本之前，默认字符集为 latin1 ，utf8字符集指向的是 utf8mb3 。网站开发人员在数据库设计的时候往往会将编码修改为utf8字符集。如果遗忘修改默认的编码，就会出现乱码的问题。从MySQL8.0开始，数据库的默认编码将改为utf8mb4 ，从而避免上述乱码的问题

1. 查看默认使用的字符集

   ```mysql
   show variables like 'character%';
   # 或者
   show variables like '%char%';
   ```

2. 修改字符集

   ```mysql
   vim /etc/my.cnf
   #在MySQL5.7或之前的版本中，在文件最后加上中文字符集配置
   character_set_server=utf8
   ```

3. 重新启动MySQL服务

   ```mysql
   systemctl restart mysqld
   #但是原库、原表的设定不会发生变化，参数修改只对新建的数据库生效
   ```

#### 二、已有库/表字符集的变更

```mysql
#MySQL5.7版本中，以前创建的库，创建的表字符集还是latin1
#修改已创建数据库的字符集
alter database dbtest1 character set 'utf8';
#修改已创建数据表的字符集
alter table t_emp convert to character set 'utf8';

#注意：但是原有的数据如果是用非'utf8'编码的话，数据本身编码不会发生改变。已有数据需要导出或删除，然后重新插入
```

### 二、各级别的字符集

```mysql
show variables like 'character%';

#character_set_server：服务器级别的字符集
#character_set_database：当前数据库的字符集
#character_set_client：服务器解码请求时使用的字符集
#character_set_connection：服务器处理请求时会把请求字符串从character_set_client转为
#character_set_connection
#character_set_results：服务器向客户端返回数据时使用的字符集
```

1. 服务器级别

   - character_set_server：服务器级别的字符集

   - 可以在启动服务器程序时通过启动选项或者在服务器程序运行过程中使用 SET 语句修改这两个变量的值。比如我们可以在配置文件中这样写。当服务器启动的时候读取这个配置文件后这两个系统变量的值便修改了

     ```mysql
     [server]
     character_set_server=gbk # 默认字符集
     collation_server=gbk_chinese_ci #对应的默认的比较规则
     ```

2. 数据库级别

   - character_set_database ：当前数据库的字符集

   - 我们在创建和修改数据库的时候可以指定该数据库的字符集和比较规则

     ```mysql
     CREATE DATABASE 数据库名
     [[DEFAULT] CHARACTER SET 字符集名称]
     [[DEFAULT] COLLATE 比较规则名称];
     ALTER DATABASE 数据库名
     [[DEFAULT] CHARACTER SET 字符集名称]
     [[DEFAULT] COLLATE 比较规则名称];
     ```

3. 表级别

   - 可以在创建和修改表的时候指定表的字符集和比较规则

     ```mysql
     CREATE TABLE 表名 (列的信息)
     [[DEFAULT] CHARACTER SET 字符集名称]
     [COLLATE 比较规则名称]]
     ALTER TABLE 表名
     [[DEFAULT] CHARACTER SET 字符集名称]
     [COLLATE 比较规则名称]
     #如果创建和修改表的语句中没有指明字符集和比较规则，将使用该表所在数据库的字符集和比较规则作为该表的字符集和比较规则
     ```

4. 列级别

   - 对于存储字符串的列，同一个表中的不同的列也可以有不同的字符集和比较规则。我们在创建和修改列定义的时候可以指定该列的字符集和比较规则，语法如下： 

     ```mysql
     CREATE TABLE 表名(
     列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称],
     其他列...
     );
     ALTER TABLE 表名 MODIFY 列名 字符串类型 [CHARACTER SET 字符集名称] [COLLATE 比较规则名称];
     #对于某个列来说，如果在创建和修改的语句中没有指明字符集和比较规则，将使用该列所在表的字符集和比较规则作为该列的字符集和比较规则
     #在转换列的字符集时需要注意，如果转换前列中存储的数据不能用转换后的字符集进行表示会发生错误。比方说原先列使用的字符集是utf8，列中存储了一些汉字，现在把列的字符集转换为ascii的话就会出错，因为ascii字符集并不能表示汉字字符
     ```

5. 小结

   - 如果创建或修改列时没有显式的指定字符集和比较规则，则该列默认用表的字符集和比较规则
   - 如果创建表时没有显式的指定字符集和比较规则，则该表默认用数据库的字符集和比较规则
   - 如果创建数据库时没有显式的指定字符集和比较规则，则该数据库默认用服务器的字符集和比较规则

### 三、字符集与比较规则

#### 一、utf8与utf8mb4

utf8 字符集表示一个字符需要使用1～4个字节，但是我们常用的一些字符使用1～3个字节就可以表示了。而字符集表示一个字符所用的最大字节长度，在某些方面会影响系统的存储和性能，所以设计MySQL的设计者偷偷的定义了两个概念

- utf8mb3：阉割过的 utf8 字符集，只使用1～3个字节表示字符
- utf8mb4：正宗的 utf8 字符集，使用1～4个字节表示字符

#### 二、比较规则

MySQL版本一共支持41种字符集，其中的 Default collation 列表示这种字符集中一种默认 的比较规则，里面包含着该比较规则主要作用于哪种语言，比如 utf8_polish_ci 表示以波兰语的规则比较， utf8_spanish_ci 是以西班牙语的规则比较， utf8_general_ci 是一种通用的比较规则

后缀表示该比较规则是否区分语言中的重音、大小写

| 后缀 |      英文释义      |       描述       |
| :--: | :----------------: | :--------------: |
| _ai  | accent insensitive |    不区分重音    |
| _as  |  accent sensitive  |     区分重音     |
| _ci  |  case insensitive  |   不区分大小写   |
| _cs  |   case sensitive   |    区分大小写    |
| _bin |       binary       | 以二进制方式比较 |

最后一列 Maxlen ，它代表该种字符集表示一个字符最多需要几个字节

1. 查看字符集的比较规则

   ```mysql
   #查看GBK字符集的比较规则
   SHOW COLLATION LIKE 'gbk%';
   #查看UTF-8字符集的比较规则
   SHOW COLLATION LIKE 'utf8%';
   ```

2. 查看服务器/数据库的字符集和比较规则

   ```mysql
   #查看服务器的字符集和比较规则
   SHOW VARIABLES LIKE '%_server';
   #查看数据库的字符集和比较规则
   SHOW VARIABLES LIKE '%_database';
   #查看具体数据库的字符集
   SHOW CREATE DATABASE dbtest1;
   #修改具体数据库的字符集
   ALTER DATABASE dbtest1 DEFAULT CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
   ```

3. 查看表的字符集和比较规则

   ```mysql
   #查看表的字符集
   show create table employees;
   #查看表的比较规则
   show table status from atguigudb like 'employees';
   #修改表的字符集和比较规则
   ALTER TABLE emp1 DEFAULT CHARACTER SET 'utf8' COLLATE 'utf8_general_ci';
   ```

### 四、请求到响应过程中字符集的变化

这几个系统变量在不同操作系统的默认值可能不同

|         系统变量         |                             描述                             |
| :----------------------: | :----------------------------------------------------------: |
|   character_set_client   |                 服务器解码请求时使用的字符集                 |
| character_set_connection | 服务器处理请求时会把请求字符串从character_set_client 转为 character_set_connection |
|  character_set_results   |             服务器向客户端返回数据时使用的字符集             |

![MySQL数据传输过程中字符编码的变化](../../../TyporaImage/MySQL数据传输过程中字符编码的变化.png)

## 六、SQL大小写规范

### 一、Windows和Linux平台区别

1. 在 SQL 中，关键字和函数名是不用区分字母大小写的，比如 SELECT、WHERE、ORDER、GROUP BY 等关键字，以及 ABS、MOD、ROUND、MAX 等函数名

2. 不过在 SQL 中，你还是要确定大小写的规范，因为在 Linux 和 Windows 环境下，你可能会遇到不同的大小写问题

   ```mysql
   SHOW VARIABLES LIKE '%lower_case_table_names%'
   #输出字段lower_case_table_names在Windows下的值为1；在Linux下的值0
   #lower_case_table_names默认为0，大小写敏感
   #设置1，大小写不敏感。创建的表，数据库都是以小写形式存放在磁盘上，对于sql语句都是转换为小写对表和数据库进行查找
   #设置2，创建的表和数据库依据语句上格式存放，凡是查找都是转换为小写进行
   ```

   - windows系统默认大小写不敏感
   - linux系统是大小写敏感的

   ```mysql
   #MySQL在Linux下数据库名、表名、列名、别名大小写规则是这样的：
   1、数据库名、表名、表的别名、变量名是严格区分大小写的；
   2、关键字、函数名称在 SQL 中不区分大小写；
   3、列名（或字段名）与列的别名（或字段别名）在所有的情况下均是忽略大小写的；
   
   #MySQL在Windows的环境下全部不区分大小写
   ```

### 二、Linux下大小写规则设置

当想设置为大小写不敏感时，要在 my.cnf 这个配置文件 [mysqld] 中加入lower_case_table_names=1 ，然后重启服务器

- 但是要在重启数据库实例之前就需要将原来的数据库和表转换为小写，否则将找不到数据库名

- 此参数适用于MySQL5.7。在MySQL 8下禁止在重新启动 MySQL 服务时将lower_case_table_names设置成不同于初始化 MySQL 服务时设置的lower_case_table_names值。如果非要将MySQL8设置为大小写不敏感，具体步骤为：

  ```mysql
  1、停止MySQL服务
  2、删除数据目录，即删除 /var/lib/mysql 目录
  3、在MySQL配置文件（ /etc/my.cnf ）中添加 lower_case_table_names=1
  4、启动MySQL服务
  ```

### 三、SQL编写建议

- 关键字和函数名称全部大写
- 数据库名、表名、表别名、字段名、字段别名等全部小写
- SQL 语句必须以分号结尾

## 七、sql_mode的合理设置

1. 宽松模式

   - 如果设置的是宽松模式，那么我们在插入数据的时候，即便是给了一个错误的数据，也可能会被接受，并且不报错
   - 应用场景：通过设置sql mode为宽松模式，来保证大多数sql符合标准的sql语法，这样应用在不同数据库之间进行 迁移时，则不需要对业务sql进行较大的修改

2. 严格模式

   - 出现上面宽松模式的错误，应该报错才对，所以MySQL5.7版本就将sql_mode默认值改为了严格模式。所以在 生产等环境 中，我们必须采用的是严格模式，进而开发、测试环境的数据库也必须要设置，这样在开发测试阶段就可以发现问题。并且我们即便是用的MySQL5.6，也应该自行将其改为严格模式
   - 开发经验：MySQL等数据库总想把关于数据的所有操作都自己包揽下来，包括数据的校验，其实开发中，我们应该在自己 开发的项目程序级别将这些校验给做了 ，虽然写项目的时候麻烦了一些步骤，但是这样做之后，我们在进行数据库迁移或者在项目的迁移时，就会方便很多
   - 改为严格模式可能存在的问题：若设置模式中包含了NO_ZERO_DATE，那么MySQL数据库不允许插入零日期，插入零日期会抛出错误而不是警告。例如，表中含字段TIMESTAMP列（如果未声明为NULL或显示DEFAULT子句）将自动分配DEFAULT '0000-00-00 00:00:00'（零时间戳），这显然是不满足sql_mode中的NO_ZERO_DATE而报错

3. 模式查看与设置

   - 查看当前的sql_mode

     ```mysql
     select @@session.sql_mode
     select @@global.sql_mode
     
     #或者
     show variables like 'sql_mode';
     ```

   - 临时设置方式：设置当前窗口中设置sql_mode

     ```mysql
     SET GLOBAL sql_mode = 'modes...'; #全局
     SET SESSION sql_mode = 'modes...'; #当前会话
     
     #改为严格模式。此方法只在当前会话中生效，关闭当前会话就不生效了。
     set SESSION sql_mode='STRICT_TRANS_TABLES';
     #改为严格模式。此方法在当前服务中生效，重启MySQL服务后失效。
     set GLOBAL sql_mode='STRICT_TRANS_TABLES';
     ```

   - 永久设置方式：在/etc/my.cnf中配置sql_mode

     在my.cnf文件(windows系统是my.ini文件)，新增以下内容后重启。

     ```mysql
     [mysqld]
     sql_mode=ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR
     _DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
     ```

     当然生产环境上是禁止重启MySQL服务的，所以采用 临时设置方式 + 永久设置方式 来解决线上的问题， 

     那么即便是有一天真的重启了MySQL服务，也会永久生效了

# 二、MySQL的数据目录

## 一、MySQL8的主要目录结构

```mysql
find / -name mysql
#查看mysql相关的文件
```

1. 数据库文件的存放路径：/var/lib/mysql/

   ```mysql
   #数据库文件的存放路径的sql
   show variables like 'datadir';
   ```

2. 相关命令目录：/usr/bin（mysqladmin、mysqlbinlog、mysqldump等命令）和/usr/sbin

   ```mysql
   #切换到相应的目录
   cd /usr/bin
   #查看相关的命令
   find . -name "mysqladmin"
   ```

3. 配置文件目录：配置文件目录：/usr/share/mysql-8.0（命令及配置文件），/etc/mysql（如my.cnf）

## 二、数据库和文件系统的关系

### 一、查看默认数据库

```mysql
SHOW DATABASES;
```

1. mysql
   - MySQL 系统自带的核心数据库，它存储了MySQL的用户账户和权限信息，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，一些帮助信息以及时区信息等
2. information_schema
   - MySQL 系统自带的数据库，这个数据库保存着MySQL服务器 维护的所有其他数据库的信息 ，比如有哪些表、哪些视图、哪些触发器、哪些列、哪些索引。这些信息并不是真实的用户数据，而是一些 描述性信息，有时候也称之为元数据。在系统数据库 information_schema 中提供了一些以innodb_sys 开头的表，用于表示内部系统表
3. performance_schema
   - MySQL系统自带的数据库，这个数据库里主要保存MySQL服务器运行过程中的一些状态信息，可以用来监控MySQL服务的各类性能指标 。包括统计最近执行了哪些语句，在执行过程的每个阶段都花费了多长时间，内存的使用情况等信息
4. sys
   - MySQL 系统自带的数据库，这个数据库主要是通过视图的形式把 information_schema和performance_schema 结合起来，帮助系统管理员和开发人员监控 MySQL 的技术性能

### 二、数据库在文件系统中的表示

```mysql
 cd /var/lib/mysql
 #可能每个人的安装目录会有所不同，导致此目录会有所不同
 ll
 #查看此文件夹下所有的文件及文件夹
 #这个数据目录下的文件和子目录比较多，除了 information_schema 这个系统数据库外，其他的数据库在数据目录下都有对应的子目录
```

### 三、表在文件系统中的表示

#### 一、InnoDB存储引擎模式

1. 表结构

   - 为了保存表结构，InnoDB在数据目录（数据库文件夹）下对应的数据库子目录下创建了一个专门用于描述表结构的文件 ，文件后缀名为 表名.frm
   - .frm文件的格式在不同的平台上都是相同的。这个后缀名为.frm是以二进制格式存储的，我们直接打开是乱码的

2. 表中数据和索引

   - 系统表空间（system tablespace）：默认情况下，InnoDB会在数据目录下创建一个名为 ibdata1 、大小为 12M 的文件，这个文件就是对应的系统表空间在文件系统上的表示。默认大小为12M，注意这个文件是自扩展文件，当不够用的时候它会自己增加文件大小

     如果你想让系统表空间对应文件系统上多个实际文件，或者仅仅觉得原来的 ibdata1 这个文件名难听，那可以在MySQL启动时配置对应的文件路径以及它们的大小，比如修改一下my.cnf 配置文件

     ```mysql
     [server]
     innodb_data_file_path=data1:512M;data2:512M:autoextend
     ```

   - 独立表空间（file-per-table tablespace）：在MySQL5.6.6以及之后的版本中，InnoDB并不会默认的把各个表的数据存储到系统表空间中，而是为每一个表建立一个独立表空间，也就是说我们创建了多少个表，就有多少个独立表空间。使用独立表空间来存储表数据的话，会在该表所属数据库对应的子目录下创建一个表示该独立表空间的文件，文件名和表名相同，只不过添加了一个.ibd 的扩展名而已，所以完整的文件名称长这样：表名.ibd 

     如果我们使用了独立表空间去存储对应数据库下表的话，那么在该表所在的数据库对应的目录下会为该表创建两个文件，分别为 表名.frm和 表名.ibd。其中表名.ibd文件就用来存储表中的数据和索引

   - 系统表空间与独立表空间的设置：我们可以自己指定使用 系统表空间 还是 独立表空间 来存储数据，这个功能由启动参数 innodb_file_per_table 控制，比如说我们想刻意将表数据都存储到 系统表空间 时，可以在启动 MySQL服务器的时候这样配置：

     ```mysql
     [server]
     innodb_file_per_table=0 # 0：代表使用系统表空间； 1：代表使用独立表空间
     ```

   - 其他类型的表空间：随着MySQL的发展，除了上述两种老牌表空间之外，现在还新提出了一些不同类型的表空间，比如通用 表空间（general tablespace）、临时表空间（temporary tablespace）等

#### 二、MyISAM存储引擎模式

1. 表结构

   - 在存储表结构方面，MyISAM和InnoDB一样，也是在数据目录下对应的数据库子目录下创建了一个专门用于描述表结构的文件：表名.frm

2. 表中数据和索引

   - 在MyISAM中的索引全部都是二级索引 ，该存储引擎的 数据和索引是分开存放 的。所以在文件系统中也是使用不同的文件来存储数据文件和索引文件，同时表数据都存放在对应的数据库子目录下。假如某个表使用MyISAM存储引擎的话，那么在它所在数据库对应的目录下会为该表创建这三个文件： 

     ```mysql
     .frm 存储表结构
     .MYD 存储数据 (MYData)
     .MYI 存储索引 (MYIndex)
     ```

#### 三、小结

有数据库a，表b

1. 如果表b采用InnoDB，data（/var/lib/mysql）\a中会产生1个或者2个文件：
   - b.frm：描述表结构文件，字段长度等
   - 如果采用系统表空间模式的，数据信息和索引信息都存储在ibdata1中
   - 如果采用 独立表空间存储模式，data（/var/lib/mysql）\a中还会产生b.ibd文件（存储数据信息和索引信息） 
   - 此外，MySQL5.7中会在data（/var/lib/mysql）/a的目录下生成 db.opt 文件用于保存数据库的相关配置。比如：字符集、比较规则。而MySQL8.0不再提供db.opt文件。 MySQL8.0中不再单独提供b.frm，而是合并在b.ibd文件中
2. 如果表b采用MyISAM ，data（/var/lib/mysql）\a中会产生3个文件：
   - MySQL5.7中：b.frm为描述表结构文件，字段长度等；MySQL8.0 中 b.xxx.sdi：描述表结构文件，字段长度等
   - b.MYD (MYData)：数据信息文件，存储数据信息(如果采用独立表存储模式)
   - b.MYI (MYIndex)：存放索引信息文件 

# 三、用户与权限管理

## 一、用户管理

### 一、登录MySQL服务器

```mysql
mysql –h hostname|hostIP –P port –u username –p DatabaseName –e "SQL语句"
```

1. -h参数：后面接主机名或者主机IP，hostname为主机，hostIP为主机IP
2. -P参数：后面接MySQL服务的端口，通过该参数连接到指定的端口。MySQL服务的默认端口是3306，不使用该参数时自动连接到3306端口，port为连接的端口号
3. -u参数：后面接用户名，username为用户名，默认为root
4. -p参数：会提示输入密码
5. DatabaseName参数：指明登录到哪一个数据库中。如果没有该参数，就会直接登录到MySQL数据库中，然后可以使用USE命令来选择数据库
6. -e参数：后面可以直接加SQL语句。登录MySQL服务器以后即可执行这个SQL语句，然后退出MySQL服务器

### 二、创建用户

```mysql
CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']];

#示例
CREATE USER zhang3 IDENTIFIED BY '123123'; # 默认host是 %，表示任何地方可以访问到
CREATE USER 'kangshifu'@'localhost' IDENTIFIED BY '123456'; #表示localhost只能本地访问
```

1. 用户名参数表示新建用户的账户，由用户（User）和主机名（Host）构成，而且User和Host同时校验才能确定用户的唯一性
2.  “[ ]”表示可选，也就是说，可以指定用户登录时需要密码验证，也可以不指定密码验证，这样用户可以直接登录。不过，不指定密码的方式不安全，不推荐使用。如果指定密码值，这里需要使用 IDENTIFIED BY指定明文密码值
3. CREATE USER语句可以同时创建多个用户

### 三、修改用户

```mysql
UPDATE mysql.user SET USER = 新名称 WHERE USER = 旧名称
```

### 四、删除用户

1. 使用DROP方式删除（推荐）：使用DROP USER语句来删除用户时，必须用于DROP USER权限

   ```mysql
   DROP USER user[,user]…;
   
   ## 默认删除host为%的用户，也可以指明host
   DROP USER 'USER'@'HOST';
   ```

2. 使用DELETE方式删除

   ```mysql
   DELETE FROM mysql.user WHERE Host=’hostname’ AND User=’username’;
   #执行完DELETE命令后要使用FLUSH命令来使用户生效
   FLUSH PRIVILEGES;
   
   #注意：不推荐通过 DELETE FROM USER u WHERE USER='li4' 进行删除，系统会有残留信息留。而drop user命令会删除用户以及对应的权限，执行命令后你会发现mysql.user表和mysql.db表的相应记录都消失了
   ```

### 五、设置当前用户密码

1. 使用旧的写法，不推荐使用

   ```mysql
   # 修改当前用户的密码：（MySQL5.7测试有效）
   SET PASSWORD = PASSWORD('123456');
   ```

2. 使用ALTER USER命令来修改当前用户密码：用户可以使用ALTER命令来修改自身密码，如下语句代表修改当前登录用户的密码

   ```mysql
   ALTER USER USER() IDENTIFIED BY 'new_password';
   ```

3. 使用SET语句来修改当前用户密码：使用root用户登录MySQL后，可以使用SET语句来修改密码

   ```mysql
   SET PASSWORD='new_password';
   #该语句会自动将密码加密后再赋给当前用户
   ```

### 六、修改其他用户密码

1. 使用ALTER语句来修改普通用户的密码：可以使用ALTER USER语句来修改普通用户的密码

   ```mysql
   ALTER USER user [IDENTIFIED BY '新密码'][,user[IDENTIFIED BY '新密码']]…;
   ```

2. 使用SET命令来修改普通用户的密码：使用root用户登录到MySQL服务器后，可以使用SET语句来修改普通用户的密码

   ```mysql
   SET PASSWORD FOR 'username'@'hostname'='new_password';
   ```

3. 使用UPDATE语句修改普通用户的密码（不推荐）

   ```mysql
   UPDATE MySQL.user SET authentication_string=PASSWORD("新密码") WHERE User = "username" AND Host = "hostname";
   ```

## 二、权限管理

### 一、权限列表

```mysql
 show privileges;
 #查看权限列表
```

1. CREATE和DROP权限 ，可以创建新的数据库和表，或删除（移掉）已有的数据库和表。如果将MySQL数据库中的DROP权限授予某用户，用户就可以删除MySQL访问权限保存的数据库
2. SELECT、INSERT、UPDATE和DELETE权限 允许在一个数据库现有的表上实施操作
3. SELECT权限只有在它们真正从一个表中检索行时才被用到
4. INDEX权限允许创建或删除索引，INDEX适用于已有的表。如果具有某个表的CREATE权限，就可以在CREATE TABLE语句中包括索引定义
5. ALTER权限可以使用ALTER TABLE来更改表的结构和重新命名表
6. CREATE ROUTINE权限 用来创建保存的程序（函数和程序），ALTER ROUTINE权限用来更改和删除保存的程序， EXECUTE权限 用来执行保存的程序
7. GRANT权限 允许授权给其他用户，可用于数据库、表和保存的程序
8. FILE权限使用户可以使用LOAD DATA INFILE和SELECT ... INTO OUTFILE语句读或写服务器上的文件，任何被授予FILE权限的用户都能读或写MySQL服务器上的任何文件（说明用户可以读任何数据库目录下的文件，因为服务器可以访问这些文件）

### 二、授予权限的原则

1. 只授予能 满足需要的最小权限 ，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可以了，不要给用户赋予update、insert或者delete权限
2. 创建用户的时候限制用户的登录主机 ，一般是限制成指定IP或者内网IP段
3. 为每个用户设置满足密码复杂度的密码
4. 定期清理不需要的用户，回收权限或者删除用户

### 三、授予权限

1. 给用户授权的方式有 2 种，分别是通过把 角色赋予用户给用户授权 和 直接给用户授权 。用户是数据库的使用者，我们可以通过给用户授予访问数据库中资源的权限，来控制使用者对数据库的访问，消除安全隐患

2. 授权命令

   ```mysql
   GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址 [IDENTIFIED BY ‘密码口令’];
   
   #示例
   GRANT SELECT,INSERT,DELETE,UPDATE ON 数据库名称.* TO 用户名@主机名（或%）;
   GRANT ALL PRIVILEGES ON *.* TO joe@'%' IDENTIFIED BY '123';
   ```

   - 该权限如果发现没有该用户，则会直接新建一个用户
   - ALL PRIVILEGES这里唯独不包括grant的权限 

### 四、查看权限

1. 查看当前用户权限

   ```mysql
   SHOW GRANTS;
   # 或
   SHOW GRANTS FOR CURRENT_USER;
   # 或
   SHOW GRANTS FOR CURRENT_USER();
   ```

2. 查看某用户的全局权限

   ```mysql
   SHOW GRANTS FOR '用户名称'@'主机地址' ;
   ```

### 五、收回权限

1. 收回权限就是取消已经赋予用户的某些权限。收回用户不必要的权限可以在一定程度上保证系统的安全性

2. MySQL中使用 REVOKE语句 取消用户的某些权限。使用REVOKE收回权限之后，用户账户的记录将从db、host、tables_priv和columns_priv表中删除，但是用户账户记录仍然在user表中保存（删除user表中的账户记录使用DROP USER语句）。所以在将用户账户从user表删除之前，应该收回相应用户的所有权限

3. 注意点，须用户重新登录后才能生效

4. 收回权限命令

   ```mysql
   REVOKE 权限1,权限2,…权限n ON 数据库名称.表名称 FROM 用户名@用户地址;
   
   #收回全库全表的所有权限
   REVOKE ALL PRIVILEGES ON *.* FROM 用户名@用户地址;
   ```

## 三、权限表

### 一、user表

user表是MySQL中最重要的一个权限表， 记录用户账号和权限信息。这些字段可以分成4类，分别是范围列（或用户列）、权限列、安全列和资源控制列

#### 一、范围列（或用户列）

1. host：表示连接类型
   - %：表示所有远程通过 TCP方式的连接 
   - IP地址：通过制定ip地址进行的TCP方式的连接
   - 机器名：通过制定网络中的机器名进行的TCP方式的连接
   - ::1：IPv6的本地ip地址，等同于IPv4的 127.0.0.1 
   - localhost：本地方式通过命令行方式的连接 ，比如mysql -u xxx -p xxx 方式的连接
2. user：表示用户名，同一用户通过不同方式链接的权限是不一样的
3. password：密码
   - 所有密码串通过 password(明文字符串) 生成的密文字符串。MySQL 8.0 在用户管理方面增加了角色管理，默认的密码加密方式也做了调整，由之前的SHA1改为了SHA2，不可逆 。同时加上 MySQL 5.7 的禁用用户和用户过期的功能
   - mysql 5.7及之后版本的密码保存到authentication_string字段中不再使用password字段

#### 二、权限列

1. Grant_priv字段：表示是否拥有GRANT权限 
2. Shutdown_priv字段：表示是否拥有停止MySQL服务的权限
3. Super_priv字段：表示是否拥有超级权限
4. Execute_priv字段 ：表示是否拥有EXECUTE权限。拥有EXECUTE权限，可以执行存储过程和函数
5. Select_priv , Insert_priv等：为该用户所拥有的权限

#### 三、安全列

- 安全列只有6个字段，其中两个是ssl相关的（ssl_type、ssl_cipher），用于 加密 ；两个是x509相关的（x509_issuer、x509_subject），用于 标识用户 ；另外两个Plugin字段用于 验证用户身份 的插件，该字段不能为空。如果该字段为空，服务器就使用内建授权验证机制验证用户身份

#### 四、资源控制列

1. max_questions：用户每小时允许执行的查询操作次数
2. max_updates：用户每小时允许执行的更新操作次数
3. max_connections：用户每小时允许执行的连接操作次数
4. max_user_connections：用户允许同时建立的连接次数

### 二、db表

#### 一、用户列

db表用户列有3个字段，分别是Host、User、Db。这3个字段分别表示主机名、用户名和数据库名。表示从某个主机连接某个用户对某个数据库的操作权限，这3个字段的组合构成了db表的主键

#### 二、权限列

Create_routine_priv和Alter_routine_priv这两个字段决定用户是否具有创建和修改存储过程的权限

### 三、tables_priv表和columns_priv表

1. tables_priv表用来对表设置操作权限，columns_priv表用来对表的 某一列设置权限
2. tables_priv表有8个字段，分别是Host、Db、User、Table_name、Grantor、Timestamp、Table_priv和Column_priv
   - Host 、 Db 、 User 和 Table_name 四个字段分别表示主机名、数据库名、用户名和表名
   - Grantor表示修改该记录的用户
   - Timestamp表示修改该记录的时间
   - Table_priv 表示对象的操作权限。包括Select、Insert、Update、Delete、Create、Drop、Grant、 References、Index和Alter
   - Column_priv字段表示对表中的列的操作权限，包括Select、Insert、Update和References

### 四、procs_priv表

procs_priv表可以对存储过程和存储函数设置操作权限

## 四、角色管理

### 一、角色的理解

- 引入角色的目的是 方便管理拥有相同权限的用户 。恰当的权限设定，可以确保数据的安全性，这是至关重要的

### 二、创建角色

1. 角色名称的命名规则和用户名类似。如果host_name省略，默认为%，role_name不可省略，不可为空

2. 创建角色语句

   ```mysql
   CREATE ROLE 'role_name'[@'host_name'] [,'role_name'[@'host_name']]...
   ```

### 三、赋予角色权限

```mysql
GRANT privileges ON table_name TO 'role_name'[@'host_name'];
```

1. 创建角色之后，默认这个角色是没有任何权限的，我们需要给角色授权，系统就会自动给你一个“ USAGE ”权限
2. 多个权限，”,“分割

### 四、查看角色的权限

```mysql
SHOW GRANTS FOR '角色名';
```

### 五、回收角色的权限

```mysql
REVOKE 权限 ON tablename FROM 'rolename';
```

### 六、删除角色

```mysql
DROP ROLE role [,role2]...
#删除了角色，那么用户也就失去了通过这个角色所获得的所有权限
```

### 七、给用户赋予角色

```mysql
GRANT role [,role2,...] TO user [,user2,...];
```

### 八、激活角色

1. 使用set default role命令激活角色

   ```mysql
   SET DEFAULT ROLE ALL TO 'username'@'localhost'[,'role_name'[@'host_name']]...
   ```

2. 将activate_all_roles_on_login设置为ON

   ```mysql
   SET GLOBAL activate_all_roles_on_login=ON;
   #对 所有角色永久激活 。运行这条语句之后，用户才真正拥有了赋予角色的所有权限
   ```

### 九、撤销用户的角色

```mysql
REVOKE 角色名称 FROM 用户名称;
```

### 十、设置强制角色

1. 服务启动前设置

   ```mysql
   [mysqld]
   mandatory_roles='role1,role2@localhost,r3'
   ```

2. 运行时设置

   ```mysql
   SET PERSIST mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后仍然有效
   SET GLOBAL mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后失效
   ```


# 四、逻辑架构

## 一、逻辑架构剖析

### 一、Connectors

Connectors包含各种图形化数据库连接工具、各种开发语言连接方式等应用程序

### 二、连接层（MySQL第一层）

1. 系统（客户端）访问MySQL服务器前，做的第一件事就是建立TCP连接
2. 经过三次握手建立连接成功后， MySQL 服务器对 TCP 传输过来的账号密码做身份认证、权限获取
   - 用户名或密码不对，会收到一个Access denied for user错误，客户端程序结束执行
   - 用户名密码认证通过，会从权限表查出账号拥有的权限与连接关联，之后的权限判断逻辑，都将依赖于此时读到的权限
3. TCP 连接收到请求后，必须要分配给一个线程专门与这个客户端的交互。所以还会有个线程池，去走后面的流程。每一个连接从线程池中获取线程，省去了创建和销毁线程的开销
4. MySQL服务器有专门的TCP连接池限制连接数，采用长连接模式复用TCP连接来解决无限创建与TCP频繁创建销毁带来的资源耗尽、性能下降问题
5. MySQL连接管理分为TCP连接池和线程池（用于认证、权限获取、SQL操作等），所以连接管理的职责是负责认证、管理连接、获取权限信息

### 三、服务层（MySQL第二层）

1. SQL interface：SQL接口
   - 接收用户的SQL命令，并且返回用户需要查询的结果。比如SELECT ... FROM就是调用SQL Interface
   - MySQL支持DML（数据操作语言）、DDL（数据定义语言）、存储过程、视图、触发器、自定义函数等多种SQL语言接口
2. Parser：解析器
   - 在解析器中对 SQL 语句进行语法分析、语义分析。将SQL语句分解成数据结构，并将这个结构传递到后续步骤，以后SQL语句的传递和处理就是基于这个结构的。如果在分解构成中遇到错误，那么就说明这个SQL语句是不合理的
   - 在SQL命令传递到解析器的时候会被解析器验证和解析，并为其创建语法树 ，并根据数据字典丰富查询语法树，会验证该客户端是否具有执行该查询的权限。创建好语法树后，MySQL还会对SQl查询进行语法上的优化，进行查询重写
3. Optimizer：查询优化器
   - SQL语句在语法解析之后、查询之前会使用查询优化器确定 SQL 语句的执行路径，生成一个执行计划
   - 这个执行计划表明应该使用哪些索引进行查询（全表检索还是使用索引检索），表之间的连接顺序如何，最后会按照执行计划中的步骤调用存储引擎提供的方法来真正的执行查询，并将查询结果返回给用户
   - 它使用“ 选取-投影-连接 ”策略进行查询。例如执行SELECT查询语句之前先根据WHERE语句进行选取，SELECT中有要查询的几个属性（字段）则先进行属性投影，而不是将属性全部取出之后再进行过滤，最后则将选取和投影进行连接
4. Caches&Buffers：查询缓存组件
   - MySQL内部维持着一些Cache和Buffer，比如Query Cache用来缓存一条SELECT语句的执行结果，如果能够在其中找到对应的查询结果，那么就不必再进行查询解析、优化和执行的整个过程了，直接将结果反馈给客户端
   - 这个缓存机制是由一系列小缓存组成的。比如表缓存，记录缓存，key缓存，权限缓存等
   - 这个查询缓存可以在不同客户端之间共享
   - 从MySQL5.7.20开始，不推荐使用查询缓存，并在MySQL8.0中删除
   - MySQL 中的查询缓存，不是缓存查询计划，而是查询对应的结果。这就意味着查询匹配的鲁棒性大大降低 ，只有相同的查询操作才会命中查询缓存。两个查询请求在任何字符上的不同（例如：空格、注释、大小写），都会导致缓存不会命中，因此MySQL的查询缓存命中率不高；同时，如果查询请求中包含某些系统函数、用户自定义变量和函数，比如NOW每次调用都会产生最新的当前时间，如果在一个查询请求中调用了这个函数，那即使查询请求的文本信息都一样，那不同时间的两次查询也应该得到不同的结果，如果在第一次查询时就缓存了，那第二次查询的时候直接使用第一次查询的结果就是错误的

### 四、引擎层（MySQL第三层）

插件式存储引擎层（ Storage Engines），真正的负责了MySQL中数据的存储和提取，对物理服务器级别维护的底层数据执行操作，服务器通过API与存储引擎进行通信。不同的存储引擎具有的功能不同，这样我们可以根据自己的实际需要进行选取

### 五、存储层

所有的数据，数据库、表的定义，表的每一行的内容，索引，都是存在 文件系统 上，以 文件 的方式存在的，并完成与存储引擎的交互。当然有些存储引擎比如InnoDB，也支持不使用文件系统直接管理裸设备，但现代文件系统的实现使得这样做没有必要了。在文件系统之下，可以使用本地磁盘，可以使用DAS、NAS、SAN等各种存储系统

### 六、逻辑架构简化版

![MySQL逻辑架构剖析图](../../../TyporaImage/MySQL%E9%80%BB%E8%BE%91%E6%9E%B6%E6%9E%84%E5%89%96%E6%9E%90%E5%9B%BE.png)

1. 连接层：客户端和服务器端建立连接，客户端发送SQL至服务器端
2. SQL层（服务层）：对SQL语句进行查询处理；与数据库文件的存储方式无关
3. 存储引擎层：与数据库文件打交道，负责数据的存储和读取

## 二、SQL执行流程

### 一、具体的执行过程

![MySQL语句执行具体过程](../../../TyporaImage/MySQL%E8%AF%AD%E5%8F%A5%E6%89%A7%E8%A1%8C%E5%85%B7%E4%BD%93%E8%BF%87%E7%A8%8B.png)

### 二、简化的执行过程

![MySQL语句执行简化过程](../../../TyporaImage/MySQL%E8%AF%AD%E5%8F%A5%E6%89%A7%E8%A1%8C%E7%AE%80%E5%8C%96%E8%BF%87%E7%A8%8B.png)

## 三、数据库缓冲池

InnoDB 存储引擎是以页为单位（每页可能是固定大小的值）来管理存储空间的，我们进行的增删改查操作其实本质上都是在访问页面（包括读页面、写页面、创建新页面等操作）。而磁盘 I/O 需要消耗的时间很多，而在内存中进行操作，效率则会高很多，为了能让数据表或者索引中的数据随时被我们所用，DBMS 会申请 占用内存来作为数据缓冲池 ，在真正访问页面之前，需要把在磁盘上的页缓存到内存中的 Buffer Pool 之后才可以访问

这样做的好处是可以让磁盘活动最小化，从而 减少与磁盘直接进行 I/O 的时间。要知道，这种策略对提升SQL语句的查询性能来说至关重要。如果索引的数据在缓冲池里，那么访问的成本就会降低很多

### 一、缓冲池存储的的数据

 在InnoDB 缓冲池包括了数据页、索引页、插入缓冲、锁信息、自适应 Hash 和数据字典信息等

### 二、缓存原则

1. “ 位置 * 频次 ”这个原则，可以帮我们对 I/O 访问效率进行优化
2. 位置决定效率，提供缓冲池就是为了在内存中可以直接访问数据
3. 频次决定优先级顺序。因为缓冲池的大小是有限的，就无法将所有数据都加载到缓冲池里，这时就涉及到优先级顺序，会优先对使用频次高的热数据进行加载

### 三、缓冲池读取数据过程

缓冲池管理器会尽量将经常使用的数据保存起来，在数据库进行页面读操作的时候，首先会判断该页面是否在缓冲池中，如果存在就直接读取，如果不存在，就会通过内存或磁盘将页面存放到缓冲池中再进行读取

![MySQL缓冲池获取数据过程](../../../TyporaImage/MySQL%E7%BC%93%E5%86%B2%E6%B1%A0%E8%8E%B7%E5%8F%96%E6%95%B0%E6%8D%AE%E8%BF%87%E7%A8%8B.png)

# 五、存储引擎

## 一、查看存储引擎

```mysql
show engines;
#或
show engines \G;
```

## 二、设置系统默认的存储引擎

### 一、查看默认的存储引擎

```mysql
show variables like '%storage_engine%';
#或
SELECT @@default_storage_engine;
```

### 二、修改默认的存储引擎

```mysql
#在命令行中使用
SET DEFAULT_STORAGE_ENGINE=MyISAM;

#在my.cnf文件中使用
default-storage-engine=MyISAM
# 重启服务
systemctl restart mysqld.service
```

### 三、设置表的存储引擎

1. 创建表时指定存储引擎

   ```mysql
   CREATE TABLE 表名(
   建表语句;
   ) ENGINE = 存储引擎名称;
   ```

2. 修改表的存储引擎

   ```mysql
   ALTER TABLE 表名 ENGINE = 存储引擎名称;
   ```

## 三、引擎介绍

### 一、InnoDB引擎

InnoDB引擎是具有外键支持功能的事务存储引擎

1. MySQL从3.23.34a开始就包含InnoDB存储引擎。大于等于5.5之后，默认采用InnoDB引擎
2. InnoDB是MySQL的默认事务型引擎，它被设计用来处理大量的短期(short-lived)事务。可以确保事务的完整提交(Commit)和回滚(Rollback)
3. 除了增加和查询外，还需要更新、删除操作，那么，应优先选择InnoDB存储引擎
4. 除非有非常特别的原因需要使用其他的存储引擎，否则应该优先考虑nnoDB引擎
5. 数据文件结构
   - 表名.frm 存储表结构（MySQL8.0时，合并在表名.ibd中）
   - 表名.ibd 存储数据和索引
6. InnoDB是为处理巨大数据量的最大性能设计
7. 对比MyISAM的存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间以保存数据和索引
8. MyISAM只缓存索引，不缓存真实数据；InnoDB不仅缓存索引还要缓存真实数据， 对内存要求较高 ，而且内存大小对性能有决定性的影响

### 二、MyISAM引擎

MyISAM引擎是主要的非事务处理存储引擎

1. MyISAM提供了大量的特性，包括全文索引、压缩、空间函数(GIS)等，但MyISAM 不支持事务、行级锁、外键，有一个毫无疑问的缺陷就是 崩溃后无法安全恢复
2. 5.5之前默认的存储引擎
3. 优势是访问的速度快，对事务完整性没有要求或者以SELECT、INSERT为主的应用
4. 针对数据统计有额外的常数存储。故而 count(*) 的查询效率很高
5. 数据文件结构
   - 表名.frm 存储表结构
   - 表名.MYD 存储数据 (MYData) 
   - 表名.MYI 存储索引 (MYIndex) 
6. 应用场景只要为只读应用或者以读为主的业务 

### 三、引擎对比

|     对比项     |                          MyISAM                          |                            InnoDB                            |
| :------------: | :------------------------------------------------------: | :----------------------------------------------------------: |
|      外键      |                          不支持                          |                             支持                             |
|      事务      |                          不支持                          |                             支持                             |
|     行表锁     | 表锁，即使操作一条记录也会锁住整个表，不适合高并发的操作 | 行锁，操作时只锁某一行，不对其它行有影响，适用于高并发的操作 |
|      缓存      |                只缓存索引，不缓存真实数据                | 不仅缓存索引还要缓存真实数据，对内存要求较高，而且内存大小对性能有决定性的影响 |
| 自带系统表使用 |                            是                            |                              否                              |
|     关注点     |             性能：节省资源、消耗少、简单业务             |                 事务：并发写、事务、更大资源                 |
|    默认安装    |                            是                            |                              是                              |
|    默认使用    |                            否                            |                              是                              |

