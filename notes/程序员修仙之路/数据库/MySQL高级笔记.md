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

所有的数据，数据库、表的定义，表的每一行的内容，索引，都是存在文件系统 上，以文件的方式存在的，并完成与存储引擎的交互。当然有些存储引擎比如InnoDB，也支持不使用文件系统直接管理裸设备，但现代文件系统的实现使得这样做没有必要了。在文件系统之下，可以使用本地磁盘，可以使用DAS、NAS、SAN等各种存储系统

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

# 六、索引的数据结构

## 一、索引及其优缺点

### 一、索引概述

1. 索引是帮助MySQL高效获取数据的数据结构
2. 索引的本质是数据结构。可以简单理解为“排好序的快速查找数据结构”，满足特定查找算法。这些数据结构以某种方式指向数据，这样就可以在这些数据结构的基础上实现高级查找算法
3. 一般情况下我们用到的B+树都不会超过4层，B+树一般使用二分法实现快速定位记录

### 二、优点

1. 降低数据库IO成本
2. 通过创建唯一索引，可以保证数据库表中每一行数据的唯一性
3. 在实现数据的参考完整性方面，可以加速表和表之间的连接。换而言之，对于有依赖关系的子表和父表联合查询时，可以提高查询速度
4. 在使用分组和排序子句进行数据查询时，可以显著减少查询中分组和排序的时间，降低CPU的消耗

### 三、缺点

1. 创建索引和维护索引要耗费时间，并且随着数据量的增加，所耗费的时间也会增加
2. 索引需要占用磁盘空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，存储在磁盘上，如果有大量的索引，索引文件就可能比数据文件达到最大文件尺寸
3. 虽然索引大大提高了查询速度，同时却会降低更新表的速度 。当对表中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度

## 二、InnoDB中的索引

### 一、聚簇索引

#### 一、特点

1. 使用记录主键值的大小进行记录和页的排序
   - 页内的记录（叶子节点内部）是按照主键的大小顺序排成一个单向链表
   - 各个存放用户记录的页（也就是叶子节点之间）也是根据页中用户记录的主键大小顺序排成一个双向连表
   - 存放目录项记录的页（非叶子节点之间）分为不同的层次，在同一层次中的页也是根据页中目录项记录的主键大小顺序排成一个双向链表，非叶子节点内部使用的是单项链表
   - 如果主键的顺序不对，MySQL引擎会自动给主键进行排序，会消耗一定的时间
2. B+树的叶子节点存储的是完整的用户记录，非叶子节点存储的是其他索引信息。所谓完整的用户记录，就是指这个记录中存储了所有列的值（包括隐藏列）

#### 二、优点

1. 数据访问更快，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快
2. 聚簇索引对于主键的排序查找和范围查找速度非常快
3. 按照聚簇索引排列顺序，查询显示一定范围数据的时候，由于数据都是紧密相连，数据库不用从多个数据块中提取数据，所以 节省了大量的io操作

#### 三、缺点

1. 插入速度严重依赖于插入顺序，按照主键的顺序插入是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个自增的ID列为主键
2. 更新主键的代价很高，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义主键为不可更新
3. 二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据

### 二、非聚簇索引

#### 一、二级索引

1. 回表：我们根据某个非主键字段大小排序的B+树只能确定我们要查找记录的主键值，所以如果我们想根据这个非主键字段的值查找到完整的用户记录的话，仍然需要到聚簇索引中再查一遍，这个过程称为回表。也就是根据这个非主键字段的值查询一条完整的用户记录需要使用到 2 棵B+树
2. 二级索引的含义大概就是根据非主键索引的字段进行索引，找到主键索引，由主键索引再去找具体的数据

#### 二、联合索引

1. 可以同时以多个列的大小作为排序规则，也就是同时为多个列建立索引，比如按照非主键字段A和B的大小进行排序
   - 先把各个记录和页按照A列进行排序
   - 在记录A列相同的情况下，采用B列进行排序
2. 以A和B列的大小为排序规则建立的B+树称为联合索引，本质上也是一个二级索引。它的意思与分别为c2和c3列分别建立索引的表述是不同的
   - 建立联合索引只会建立如上图一样的1棵B+树
   - 为c2和c3列分别建立索引会分别以c2和c3列的大小为排序规则建立2棵B+树

### 三、InnoDB的B+树索引注意事项

1. 根页面位置万年不动
2. 内节点中目录项记录的唯一性
3. 一个页面最少存储2条记录

## 三、MyISAM中的索引

MyISAM引擎使用B+Tree作为索引结构，叶子节点的data域存放的是数据记录的地址

## 四、MyISAM与InnoDB对比

1. MyISAM的索引方式都是非聚簇的；InnoDB必须包含1个聚簇索引
2. 在InnoDB存储引擎中，我们只需要根据主键值对聚簇索引进行一次查找就能找到对应的记录，而在MyISAM中却需要进行一次回表操作，意味着MyISAM中建立的索引相当于全部都是二级索引
3. InnoDB的数据文件本身就是索引文件，而MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址
4. InnoDB的非聚簇索引data域存储相应记录主键的值，而MyISAM索引记录的是地址。换句话说，InnoDB的所有非聚簇索引都引用主键作为data域
5. MyISAM的回表操作是十分快速的，因为是拿着地址偏移量直接到文件中取数据的，反观InnoDB是通过获取主键之后再去聚簇索引里找记录，虽然说也不慢，但还是比不上直接用地址去访问
6. InnoDB要求表必须有主键（MyISAM可以没有）。如果没有显式指定，则MySQL系统会自动选择一个可以非空且唯一标识数据记录的列作为主键。如果不存在这种列，则MySQL自动为InnoDB表生成一个隐 含字段作为主键，这个字段长度为6个字节，类型为长整型

## 五、索引的代价

1. 空间上的代价
   - 每建立一个索引都要为它建立一棵B+树，每一棵B+树的每一个节点都是一个数据页，一个页默认会占用 16KB的存储空间，一棵很大的B+树由许多数据页组成，那就是很大的一片存储空间
2. 时间上的代价
   - 每次对表中的数据进行增、删、改 操作时，都需要去修改各个B+树索引
   - 由于索引时叶子节点/非叶子节点内部使用的单链表结构，叶子节点/非叶子节点之间连接使用的是双向链表结构，而增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需要额外的时间进行一些记录移位，页面分裂、页面回收等操作来维护好节点和记录的排序。如果我们建了许多索引，每个索引对应的B+树都要进行相关的维护操作，会给性能拖后腿

## 六、数据结构选取的合理性

### 一、Hash结构

1. 哈希函数h有可能将两个不同的关键字映射到相同的位置，这叫做碰撞，在数据库中一般采用链接法来解决。在链接法中，将散列到同一槽位的元素放在一个链表中
2. 在MyISAM和InnoDB中都不支持Hash索引，但都采用自适应Hash索引，目的是方便根据SQL的查询条件加速定位到叶子节点，特别是当B+树比较深的时候，通过自适应 Hash 索引可以明显提高数据的检索效率

### 二、二叉搜索树

1. 查找规则：节点左侧的值大于父节点的值，节点右侧的值小于父节点的值
2. 为了提高查询效率，就需要减少磁盘IO次数。为了减少磁盘IO的次数，就需要尽量降低树的高度，需要把原来“瘦高”的树形结构变得“矮胖”，树的每层的分叉越多越好

### 三、平衡二叉树

1. 在二叉搜索树的基础上进行平衡数据
2. 可以把节点的二叉分为更多，可以分为三叉、四叉等等

### 四、B-Tree

1. M阶的B树中的M指的是除去根节点外的节点最多有M个孩子的节点（子树）
2. 根节点的儿子数的范围是[2,M]
3. 每个中间节点包含k-1个关键字和k个孩子，孩子的数量=关键字的数量+1，k的取值范围为[ceil(M/2), M]，其中ceil的意思为向上取整
4. 叶子节点包括k-1个关键字（叶子节点没有孩子），k的取值范围为[ceil(M/2), M]
5. 假设中间节点节点的关键字为：Key[1], Key[2], …, Key[k-1]，且关键字按照升序排序，即 Key[i] <Key[i+1]。此时 k-1 个关键字相当于划分了 k 个范围，也就是对应着 k 个指针，即为：P[1], P[2], …,P[k]，其中 P[1] 指向关键字小于 Key[1] 的子树，P[i] 指向关键字属于 (Key[i-1], Key[i]) 的子树，P[k]指向关键字大于 Key[k-1] 的子树
6. 所有叶子节点位于同一层

### 五、B+Tree

1. 有k个孩子的节点就有k个关键字。也就是孩子数量=关键字数，而B树中，孩子数量=关键字数 +1
2. 非叶子节点的关键字也会同时存在在子节点中，并且是在子节点中所有关键字的最大（或最小）
3. 非叶子节点仅用于索引，不保存数据记录，跟记录有关的信息都放在叶子节点中。而B树中，非叶子节点既保存索引，也保存数据记录
4. 所有关键字都在叶子节点出现，叶子节点构成一个有序链表，而且叶子节点本身按照关键字的大小从小到大顺序链接

# 七、InnoDB数据存储结构

## 一、数据的存储结构：页

### 一、磁盘与内存交互基本单位：页

1. InnoDB将数据划分为若干个页，InnoDB中页的大小默认为16KB
2. 以页作为磁盘和内存之间交互的基本单位，也就是一次最少从磁盘中读取16KB的内容到内存中，一次最少把内存中的16KB内容刷新到磁盘中。也就是说，在数据库中，不论读一行，还是读多行，都是将这些行所在的页进行加载。简而言之，数据库管理存储空间的基本单位是页，数据库I/O操作的最小单位也是页。一个页中可以存储多个行记录

### 二、页结构概述

1. 不同的页可以不在物理结构上相连，只要通过双向链表相关联即可。每个数据页中的记录会按照主键值从小到大的顺序组成一个单向链表，每个数据页都会为存储在它里面的记录生成一个页目录，在通过主键查找某条记录的时候可以在目录中使用二分法快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录

### 三、页的大小

不同的数据库管理系统（简称DBMS）的页大小不同，比如在MySQL的InnoDB存储引擎中，默认页的大小是16KB

### 四、页的上层结构

![页的上层结构](../../../TyporaImage/MySQL%E9%A1%B5%E7%9A%84%E4%B8%8A%E5%B1%82%E7%BB%93%E6%9E%84.png)

1. 区（Extent）是比页大一级的存储结构，在InnoDB存储引擎中，一个区会分配64个连续的页。因为InnoDB中的页大小默认为16KB，所以一个区的大小是64*16KB=1MB。区与区之间可能在物理上不是连续存储的，但是是通过双向链表进行连接的
2. 碎片区（Fragment）：为了考虑以完整的区为单位分配给某个段对于数据量小的表太浪费空间的这种情况，则提出碎片区的概念。在一个碎片去内，并不是所有的页都是为了存储同一个段的数据而存在的，而是碎片区中的页可以用于不同的目的。碎片区直属于表空间，并不属于任何一个段。在刚开始向表中插入数据的时候，段是从某个碎片区以单个页面为单位来分配存储空间的；当某个段已经占用了32个碎片区页面之后，就会申请以完整的区为单位来分配存储空间。所以段不能仅定义为是某些区的集合，更精确的应该是某些零散的页面以及一些完整的区的集合
3. 段（Segment）由一个或多个区组成，不过在段中不要求区与区之间是相邻的。段是数据库中的分配单位，不同类型的数据库对象以不同的段形式存在。当我们创建表、索引的时候，就会创建对应的段，比如创建一张表时会创建一个表段，创建一个索引时会创建一个索引段，也就是说一个索引会生成两个段，一个是叶子节点段，一个是非叶子节点段。存放叶子节点的区的集合就算是一个段，存放非叶子节点的区的集合也算是一个段，当然还有其他的段。段其实不对应表空间中某一个连续的物理区域，而是一个逻辑上的概念，由若干零散的页面以及一些完整的区组成
4. 表空间（Taablespace）是一个逻辑容器，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。数据库由一个或多个表空间组成，表空间从管理上可以划分为系统表空间、独立表空间、撤销表空间、临时表空间等

## 二、页的内部结构

### 一、页的组成部分

1. 页如果按照类型划分，则分为数据页（保存B+树节点）、系统页、Undo页和事务数据页等
2. 数据页的16KB大小的存储空间被划分为7个部分，分别是文件头、页头、最大最小记录、用户记录、空闲空间、页目录、文件尾

### 二、数据页的组成明细

|       名称       | 占用大小 |               说明               |
| :--------------: | :------: | :------------------------------: |
|   File Header    |  38字节  |       文件头，描述页的信息       |
|   Page Header    |  56字节  |        页头，页的状态信息        |
| Infimum+Supermum |  26字节  | 最大和最小记录，这是虚拟的行记录 |
|   User Records   |  不确定  |     用户记录，存储行记录内容     |
|    Free Space    |  不确定  | 空闲记录，页中还没有被使用的空间 |
|  Page Directory  |  不确定  |  页目录，存储用户记录的相对位置  |
|   File Trailer   |  8字节   |      文件尾，校验页是否完整      |

## 三、InnoDB行格式（或记录格式）

![COMPACT行格式](../../../TyporaImage/MySQL_COMPACT行格式.png)

# 八、索引的创建与设计原则

## 一、索引的声明与使用

### 一、索引的分类

- 从功能逻辑上说，分为普通索引、唯一索引、主键索引、全文索引
- 从物理实现上说，分为聚簇索引和非聚簇索引
- 从字段个数上说，分为单列索引和联合索引

1. 普通索引：在创建普通索引时，不附加任何限制条件，只是用于提高查询效率，这类索引可以创建在任何数据类型中，其值是否唯一和非空，要由字段本身的完整性约束条件来决定
2. 唯一性索引：使用UNIQUE参数可以设置索引为唯一性索引，在创建唯一性索引时，限制该索引的值必须是唯一性的，但允许有空值。在一张数据表中可以有多个唯一性索引
3. 主键索引：主键索引就是一种特殊的唯一性索引，在唯一性约束的基础上增加了不为空的约束，也就是NOT NULL+UNIQUE，一张表里最多有一个主键索引
4. 单例索引：在表中的单个字段上创建索引。单列索引可以是普通索引、唯一性索引，或者是全文索引。只要保证该索引只对应一个字段即可，一个表可以有多个单列索引
5. 多列（组合、联合）索引：在表的多个字段上创建一个索引。该索引指向创建时对应的多个字段，可以通过这几个字段进行查询，但是只有查询条件中使用了这些字段中的第一个字段时才会被使用
6. 全文索引：能够利用分词技术等多种算法智能分析出文本文字中关键字的频率和重要性，然后按照一定的算法规则智能地筛选出我们想要的搜索结果。全文索引非常适用大型数据集，对于小的数据集，它的用处比较小。适用参数FULLTEXT可以设置索引为全文索引
7. 空间索引：使用参数SPATIAL可以设置索引为空间索引，空间索引只能建立在空间数据类型上，这样可以提高系统获取空间数据的效率。MySQL中的空间数据类型包括GEOMETRY、POINT、LINESTRING和POLYGON等，目前只有MyISAM存储引擎支持空间索引，而且空间索引的字段不能为空

### 二、创建索引

#### 一、创建表时创建索引

1. 隐式的方式创建索引

   ```mysql
   #在声明有主键约束、唯一性约束、外键约束的子段上，会自动的添加相关的索引
   CREATE TABLE dept(
   dept_id INT PRIMARY KEY AUTO_INCREMENT,
   dept_name VARCHAR(20)
   );
   
   CREATE TABLE emp(
   emp_id INT PRIMARY KEY AUTO_INCREMENT,
   emp_name VARCHAR(20) UNIQUE,
   dept_id INT,
   CONSTRAINT emp_dept_id_fk FOREIGN KEY(dept_id) REFERENCES dept(dept=_id)
   );
   ```

2. 显示的方式创建索引

   ```mysql
   CREATE TABLE table_name [col_name data_type]
   [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC |
   DESC]
   ```

   - UNIQUE、 FULLTEXT和SPATIAL为可选参数，分别表示唯一索引、全文索引和空间索引，不写就是普通索引
   - INDEX与KEY为同义词，两者的作用相同，用来指定创建索引
   - index_name指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名
   - col_name为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择
   - length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度
   - ASC或DESC指定升序或者降序的索引值存储

   ```mysql
   #0、查看索引
   SHOW INDEX FROM table_name;
   
   #1、创建普通索引
   CREATE TABLE book(
   book_id INT ,
   book_name VARCHAR(100),
   authors VARCHAR(100),
   info VARCHAR(100) ,
   comment VARCHAR(100),
   year_publication YEAR,
   INDEX(year_publication)
   );
   
   #2、创建唯一索引
   CREATE TABLE test1(
   id INT NOT NULL,
   name varchar(30) NOT NULL,
   UNIQUE INDEX uk_idx_id(id)
   );
   
   #3、创建主键索引，设置主键后数据库会自动建立索引，InnoDB为聚簇索引
   CREATE TABLE student (
   id INT(10) UNSIGNED AUTO_INCREMENT ,
   student_no VARCHAR(200),
   student_name VARCHAR(200),
   PRIMARY KEY(id)
   )
   #删除主键索引
   ALTER TABLE student drop PRIMARY KEY ;
   #修改主键索引时，必须先删除掉（drop）原索引，再创建（add）索引
   
   #4、创建单列索引
   CREATE TABLE test2(
   id INT NOT NULL,
   name CHAR(50) NULL,
   INDEX single_idx_name(name(20))
   );
   
   #5、创建组合索引
   CREATE TABLE test3(
   id INT(11) NOT NULL,
   name CHAR(30) NOT NULL,
   age INT(11) NOT NULL,
   info VARCHAR(255),
   INDEX multi_idx(id,name,age)
   );
   
   #6、创建全文索引。在MySQL5.7及之后版本中可以不指定最后的ENGINE了，因为在此版本中InnoDB支持全文索引
   CREATE TABLE test4(
   id INT NOT NULL,
   name CHAR(30) NOT NULL,
   age INT NOT NULL,
   info VARCHAR(255),
   FULLTEXT INDEX futxt_idx_info(info)
   ) ENGINE=MyISAM;
   #全文索引时使用match+against方式，如
   select * from test4 where match(info) against('查询的字符串');
   
   #7、创建空间索引。空间索引创建时，要求空间类型的字段必须是非空的
   CREATE TABLE test5(
   geo GEOMETRY NOT NULL,
   SPATIAL INDEX spa_idx_geo(geo)
   ) ENGINE=MyISAM;
   ```

#### 二、存在表时创建索引

1. 使用ALTER TABLE语句创建索引

   ```mysql
   ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY]
   [index_name] (col_name[length],...) [ASC | DESC]
   ```

2. 使用CREATE INDEX语句创建索引

   ```mysql
   CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
   ON table_name (col_name[length],...) [ASC | DESC]
   ```

### 三、删除索引

1. 使用ALTER TABLE语句删除索引

   ```mysql
   ALTER TABLE table_name DROP INDEX index_name;
   #添加AUTO_INCREMENT约束字段的唯一索引不能被删除
   #删除表中列时，如果删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除
   ```

2. 使用DROP INDEX语句删除索引

   ```mysql
   DROP INDEX index_name ON table_name;
   #添加AUTO_INCREMENT约束字段的唯一索引不能被删除
   #删除表中列时，如果删除的列为索引的组成部分，则该列也会从索引中删除。如果组成索引的所有列都被删除，则整个索引将被删除
   ```

## 二、MySQL8索引新特性

### 一、支持降序索引

```mysql
CREATE TABLE ts1(a int,b int,index idx_a_b(a asc,b desc));

#降序索引只对查询中特定的排序顺序有效
select * from ts1 order by a desc,b desc
#这种排序方式则上述索引不会起作用
```

### 二、隐藏索引

从MySQL8开始支持隐藏索引（invisible indexes），只需要将待删除的索引设置为隐藏索引，使查询优化器不再使用这个索引（即使使用force index（强制使用索引），优化器也不会使用该索引），确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。 这种通过先将索引设置为隐藏索引，再删除索引的方式就是软删除

1. 创建表时直接创建隐藏索引

   ```mysql
   CREATE TABLE tablename(
   propname1 type1[CONSTRAINT1],
   propname2 type2[CONSTRAINT2],
   ……
   propnamen typen,
   INDEX [indexname](propname1 [(length)]) INVISIBLE
   );
   #上述语句比普通索引多了一个关键字INVISIBLE，用来标记索引为不可见索引
   ```

2. 在已经存在的表上创建隐藏索引

   ```mysql
   CREATE INDEX indexname ON tablename(propname[(length)]) INVISIBLE;
   ```

3. 通过ALTER TABLE语句创建

   ```mysql
   ALTER TABLE tablename
   ADD INDEX indexname (propname [(length)]) INVISIBLE;
   ```

4. 切换索引可见状态（前提是该索引已存在）

   ```mysql
   ALTER TABLE tablename ALTER INDEX index_name INVISIBLE; #切换成隐藏索引
   ALTER TABLE tablename ALTER INDEX index_name VISIBLE; #切换成非隐藏索引
   
   #注意 当索引被隐藏时，它的内容仍然是和正常索引一样实时更新的。如果一个索引需要长期被隐藏，那么可以将其删除，因为索引的存在会影响插入、更新和删除的性能
   ```

5. 使隐藏索引对查询优化器可见

    use_invisible_indexes设置为off（默认），优化器会忽略隐藏索引，设置为on则为打开，即使隐藏索引不可见，优化器在生成执行计划时仍会考虑使用隐藏索引

   ```mysql
   #查看查询优化器的开关设置
   select @@optimizer_switch
   #在输出结果中use_invisible_indexes=off，则说明隐藏索引默认对查询优化器不可见
   
   #使隐藏索引对查询优化器可见
   set session optimizer_switch="use_invisible_indexes=on";
   
   #使隐藏索引对查询优化器不可见
   set session optimizer_switch="use_invisible_indexes=off";
   ```

## 三、索引的设计原则

### 一、适合创建索引的情况

1. 字段的数值有唯一性的限制

   - 业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引
   - 不要以为唯一索引影响了insert的速度，这个速度损耗可以忽略，但提高查找速度是明显的

2. 频繁作为哦WHERE查询条件的字段

   - 某个字段在SELECT语句的 WHERE 条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率

3. 经常GROUP BY和ORDER BY的列

   - 索引就是让数据按照某种顺序进行存储或检索，简而言之按照某个字段的值排好序和分组了，因此当我们使用GROUP BY对数据进行分组查询，或者使用ORDER BY对数据进行排序的时候，就需要对分组或者排序的字段进行索引
   - 如果待排序的列有多个，那么可以在这些列上建立组合索引，此组合索引要把GROUP BY中要使用的字段放在前面
   - 组合索引中的字段不仅可以在WHERE条件中使用，组合索引中的字段也可以分别在GROUP BY和ORDER BY中单独使用

4. UPDATE、DELETE的WHERE条件列

   - 对数据按照某个条件进行查询后再进行UPDATE或DELETE的操作，如果对WHERE字段创建了索引，就能大幅提升效率。原理是因为我们需要先根据WHERE条件列检索出来这条记录，然后再对它进行更新或删除
   - 如果进行更新的时候，更新的字段是非索引字段，提升的效率会更明显，这是因为非索引字段更新不需要对索引进行维护

5. DISTINCT字段需要创建索引

   - 需要对某个字段进行去重，使用DISTINCT，那么对这个字段创建索引，也会提升查询效率

6. 多表JOIN连接操作时，创建索引注意事项

   - 连接表的数量尽量不要超过3张，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增长会非常快，严重影响查询的效率
   - 对WHERE条件创建索引 ，因为WHERE才是对数据条件的过滤。如果在数据量非常大的情况下，没有WHERE条件过滤是非常可怕的
   - 对用于连接的字段创建索引，并且该字段在多张表中的类型必须一致

7. 使用列的类型小的创建索引

   - 此处所说的类型大小指的是该类型表示的数据范围的大小，以整型为例，能使用INT就不要使用BIGINT，能使用MEDIUMINT就不要使用INT
   - 数据类型越小，在查询时进行的比较操作越快
   - 数据类型越小，索引占用的存储空间就越少，在一个数据页内就可以放下更多的记录，从而减少磁盘I/O带来的性能损耗，也就意味着可以把更多的数据页缓存在内存中，从而加快读写效率
   - 这个建议对于表的主键来说更加适用，因为不仅是聚簇索引中会存储主键值，其他所有的二级索引的节点处都会存储一份记录的主键值，如果主键使用更小的数据类型，也就意味着节省更多的存储空间和更高效的I/O

8. 使用字符串前缀创建索引

   ```mysql
   create table shop(address varchar(120) not null);
   alter table shop add index(address(12));
   #截取前12个字符作为索引
   
   #索引的截取公式
   count(distinct left(列名, 索引长度))/count(*)
   #此处的索引长度可以不断尝试大小，如果多次尝试的结果最后的值接近，且接近1则选择最小的那个索引长度；如果相差很大，则需将索引长度不断放大
   ```

   - B+树索引中的记录需要把该列的完整字符串存储起来，更费时。而且字符串越长，在索引中占用的存储空间越大
   - 如果B+树索引中索引列存储的字符串很长，那在做字符串比较时会占用更多的时间
   - 可以通过截取字段的前面一部分内容建立索引，这个就叫做前缀索引。这样在查找记录时虽然不能精确定位到记录的位置，但是能定位到相应前缀所在的位置，然后根据前缀相同的记录的主键值回表查询完整的字符串值。既节约空间，又减少了字符串的比较时间，还大体能解决排序的问题
   - 在varchar类型字段上建立索引时，必须指定索引的长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度。索引的长度与区分度是一对矛盾体，一般对字符串类型的数据，长度为20的索引，区分度会高达90%以上，可以使用上述的索引截取公式来确定区分度
   - 如果使用了索引前缀，因为二级索引中不包含完整的列信息，所以无法对截取的索引的前缀进行排序。简而言之，使用索引前缀的方式无法支持使用索引排序，只能使用文件排序

9. 区分度高（散列性高）的列适合作为索引

   - 列的基数指的是某一列中不重复数据的个数
   - 在记录行数一定的情况下，列的基数越大，该列中的值越分散；列的基数越小，该列中的值越集中
   - 可以使用公式 select count(distinct 字段名)/count(*) from 表名来计算区分度，越接近1越好，一般超过33%就算是比较高效的索引了
   - 联合索引把区分度（散列性）高的列放在前面

10. 使用最频繁的列放到联合索引的左侧

    - 遵循最左匹配原则，可以增加联合索引的使用率

11. 在多个字段都需要创建索引的情况下，联合索引优于单值索引

### 二、限制索引的数目

- 索引的数目不是越多越好，建议单表索引的数量不超过6个
- 每个索引都需要占用磁盘空间，索引越多，需要的磁盘空间就越大
- 索引会影响INSERT、DELETE、UPDATE等语句的性能，因为表中的数据更改时的同时，索引也会进行调整和更新，会造成负担
- 优化器在选择如何优化查询时，会根据统一信息，对每一个可以用到的索引来进行评估，以生成一个最好的执行计划，如果同时有很多个索引都可以用于查询，会增加MySQL优化器生成执行计划时间，降低查询性能

### 三、不适用创建索引的情况

1. 在WHERE条件中使用不到的字段，不要设置索引
   - WHERE条件（包括GROUP BY、ORDER BY）里用不到的字段不需要创建索引
2. 数据量小的表最好不要使用索引
   - 表记录太少，是否创建索引对查询效率的影响并不大
3. 有大量重复数据的列上不要创建索引
   - 当数据重复大时，比如高于10%的时候，也不需要对这个字段使用索引
4. 避免对经常更新的表创建过多的索引
   - 频繁更新的字段不一定要创建索引。因为更新数据的时候，也要更新索引，如果索引太多，在更新索引的时候也会造成负担，从而影响效率
   - 避免对经常更新的表创建过多的索引，并且索引中的列尽可能少。此时，虽然提高了查询效率，同时却会降低更新表的速度
5. 不建议用无序的值作为索引
6. 删除不再使用或很少使用的索引
   - 为了减少索引对更新操作的影响
7. 不要定义冗余或重复的索引
   - 不要定义冗余索引：比如有一个字段已经在联合索引中（联合索引中的第一个索引字段）了，就没必要再去单独定义一个
   - 不要定义重复索引：比如一个字段已经是主键索引了，就没必要再去定义它为唯一性索引或者普通索引

# 九、性能分析工具的使用

## 一、查看系统性能参数

可以使用SHOW STATUS语句查询一些MySQL数据库服务器的性能参数、执行频率

```mysql
#SHOW STATUS语句语法
SHOW [GLOBAL|SESSION] STATUS LIKE '参数';
```

常用的参数有

- Connections：连接MySQL服务器的次数
- Uptime：MySQL服务器的上线时间
- Slow_queries：慢查询的次数
- Innodb_rows_read：Select查询返回的行数
- Innodb_rows_inserted：执行INSERT操作插入的行数
- Innodb_rows_updated：执行UPDATE操作更新的行数
- Innodb_rows_deleted：执行DELETE操作删除的行数
- Com_select：查询操作的次数
- Com_insert：插入操作的次数，对于批量插入的 INSERT 操作，只累加一次
- Com_update：更新操作的次数
- Com_delete：删除操作的次数

## 二、统计SQL的查询成本

```mysql
 SHOW STATUS LIKE 'last_query_cost';
 #返回值last_query_cost的值为页的数量
```

- 相同的SQL即使last_query_cost的数值不同，但是效率上可能相同
- 查询效率接近的原因是采用了顺序读取的方式将页面一次性加载到缓冲池中，然后再进行查找。虽然页数量（last_query_cost）增加了不少，但是通过缓冲池的机制，并没有增加多少查询时间
- 使用场景为对于开销的比较，特别是有好几种查询方式可选的时候

## 三、慢查询日志

1. 开启show_query_log

   ```mysql
   set global slow_query_log='ON';
   
   #查看下慢查询日志是否开启，以及慢查询日志文件的位置
   show variables like '%slow_query_log%';
   ```

2. 修改long_query_time阈值

   ```mysql
   #查看long_query_time阈值
   show variables like '%long_query_time%';
   
   #修改long_query_time阈值
   #设置global的方式对当前session的long_query_time失效。对新连接的客户端有效，所以还需要设置当前会话的阈值
   set global long_query_time = 1;
   set long_query_time=1;
   ```

3. 查看慢查询数目

   ```mysql
   SHOW GLOBAL STATUS LIKE '%Slow_queries%';
   ```

4. 慢查询日志分析工具：mysqldumpslow

   ```mysql
   #按照查询时间排序，查看前五条SQL语句
   mysqldumpslow -s t -t 5 /var/lib/mysql/慢日志文件名称.log
   #得到返回记录集最多的10个SQL
   mysqldumpslow -s r -t 10 /var/lib/mysql/慢日志文件名称.log
   #得到访问次数最多的10个SQL
   mysqldumpslow -s c -t 10 /var/lib/mysql/慢日志文件名称.log
   #得到按照时间排序的前10条里面含有左连接的查询语句
   mysqldumpslow -s t -t 10 -g "left join" /var/lib/mysql/慢日志文件名称.log
   #另外建议在使用这些命令时结合 | 和more 使用 ，否则有可能出现爆屏情况
   mysqldumpslow -s r -t 10 /var/lib/mysql/慢日志文件名称.log | more
   ```

   mysqldumpslow 命令的具体参数如下：

   - -a：不将数字抽象成N，字符串抽象成S
   - -s:：是表示按照何种方式排序：
     - c：访问次数 
     - l：锁定时间 
     - r：返回记录 
     - t：查询时间
     - al：平均锁定时间
     - ar：平均返回记录数
     - at：平均查询时间（默认方式）
     - ac：平均查询次数
   - -t:：即为返回前面多少条的数据
   - -g：后边搭配一个正则匹配模式，大小写不敏感的

5. 关闭慢查询日志

   - 永久性关闭

     ```mysql
     [mysqld]
     slow_query_log=OFF
     
     #或者，把slow_query_log一项注释掉 或 删除
     #重启MySQL服务
     ```

   - 临时性关闭

     ```mysql
     SET GLOBAL slow_query_log=off
     #重启MySQL服务
     ```

6. 删除慢查询日志

## 四、查看SQL执行成本

1. 查看SQL执行成本的PROFILE是否开启

   ```mysql
   show variables like 'profiling';
   ```

2. 开启PROFILE

   ```mysql
   set profiling = 'ON';
   ```

3. 查看当前会话中的profiles

   ```mysql
   #查看当前会话中的profile
   show profile;
   
   #查看具体的SQL执行情况，其中Query_id来自于show profile输出的Query_id
   show profile cpu,block io for query Query_id;
   #输出的executing即为SQL的执行效率
   ```

## 五、分析查询语句：EXPLAIN

### 一、版本情况

- MySQL5.6.3以前只能EXPLAIN SELECT
- MySQL5.6.3以后就可以EXPLAIN SELECT,UPDATE,DELETE

### 二、基本语法

```mysql
EXPLAIN SELECT select_options
或者
DESCRIBE SELECT select_options
```

EXPLAIN语句输出的各个列的作用

|     列名      |                          描述                          |
| :-----------: | :----------------------------------------------------: |
|      id       | 在一个大的查询语句中每个SELECT关键字都对应一个唯一的id |
|  select_type  |            SELECT关键字对应的那个查询的类型            |
|     table     |                          表名                          |
|  partitions   |                     匹配的分区信息                     |
|     type      |                   针对单表的访问方法                   |
| possible_keys |                     可能用到的索引                     |
|      key      |                    实际上使用的索引                    |
|    key_len    |                  实际使用到的索引长度                  |
|      ref      | 当使用索引列等值查询时，与索引列进行等值匹配的对象信息 |
|     rows      |                预估的需要读取的记录条数                |
|   filtered    |      某个表经过搜索条件过滤后剩余记录条数的百分比      |
|     Extra     |                     一些额外的信息                     |

### 三、EXPLAIN各列作用

#### 一、table

- 不论查询语句有多复杂，里面包含多个张表，到最后也是需要对每个表进行单表访问的，所以MySQL规定EXPLAIN语句输出的每条记录都对应着某个单表的访问方法，该条记录的table列代表着该表的表名（有时候不是真实的表名字，可能是简称，也可能是临时表，也可能是结果集）

#### 二、id

- 一般的一个SELECT关键字对应着一个id，但是SQL可能会优化我们的SQL，把SELECT语句减少，所以可能出现id会比SELECT关键字少的情况
- id如果相同，可以认为是一组，从上往下顺序执行
- 在所有组中，id值越大，优先级越高，越先执行
- id中每个号码，表示一趟独立的查询，一个SQL的查询趟数越少越好

#### 三、select_type

- 一条大的查询语句中可能包含若干个SELECT关键字，每个SELECT关键字代表一个小的查询语句，而每个SELECT关键字的FROM子句中都包含若干张表（这些表用来做连接拆查询），每一张表都对应着执行计划输出中的一条记录，对于在同一个SELECT关键字中的表来说，它们的id值是相同的
- MySQL为每一个SELECT关键字代表的小查询都定义了一个称之为select_type的属性，根据此属性就知道了小查询在整个大查询中扮演的角色

#### 四、partitions

- partitions代表分区表中的命中情况，非分区表，该项为NULL。一般情况下我们的查询语句的执行情况partitions列的值都是NULL

#### 五、type

- 执行计划的一条记录就代表MySQL对某个表的执行查询时的访问方法，又称访问类型，其中type列就表明这个访问方法是什么
- 完整的访问方法为：system，const，eq_ref，ref，fulltext，ref_or_null，index_merge，unique_subquery，index_subquery，range，index，ALL。从前往后，性能依次降低。SQL中执行计划最好能达到system，const，eq_ref，ref级别，至少要达到range级别

1. system：当表中只有一条记录并且该表使用的存储引擎的统计数据是精确的，如MyISAM、Memory存储引擎等，那么对该表的访问方法就是system
2. const：当我们根据主键或者唯一二级索引列与常数进行等值匹配时，常数要与列的数据类型一致，则对单表的访问方法就是const
3. eq_ref：在连接查询时，如果被驱动表（一般为join后面的表）是通过主键或唯一二级索引列等值匹配的方式进行访问的（如果该主键或唯一二级索引是联合索引的话，所有的索引列都必须进行等值比较），则对该被驱动表的访问方式就是eq_ref
4. ref：当通过普通的二级索引列与常量进行等值匹配时来查询某个表，那么对该表的访问方法就可能是ref
5. ref_or_null：当通过普通的二级索引列进行等值匹配查询，该索引列的值也可能是NULL值时，那么对该表的访问方法就可能是ref_or_null
6. index_merge：单表访问方法时在某种场景下可以使用Intersection、union、sort_union这三种索引方式合并的方式来执行查询
7. unique_subquery：针对一些包含在IN子查询中的查询语句，如果查询优化器决定将IN子查询转换为EXISTS子查询，而且子查询可以使用到主键进行等值匹配的话，那么该子查询执行计划的type就是unique_subquery
8. range：如果使用索引获取某些范围区间的记录，那么就可能使用到range访问方法
9. index：当我们可以使用索引覆盖（在使用联合索引时，查询的字段和过滤的字段都在联合索引字段中时，且可以不是联合索引的第一列，则叫索引覆盖），但需要扫描全部的索引记录时，该表的访问方法就是index
10. all：全表扫描 

#### 六、possible_keys和key

1. possible_keys：表示的是在对某个表执行单表查询时可能用到的索引，一般查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询出来，possible_keys所列出的索引越少越好，这样执行器在执行时就不用纠结
2. key：表示实际用到的列，如果为NULL，则没有使用索引
3. key并不是possible_keys的子集关系

#### 七、key_len

- 实际使用到的索引长度（即：字节数）。帮助检查是否充分的利用上了索引，值越大越好，主要针对联合索引有一定的意义

#### 八、ref

- 当使用索引列等值查询时，与索引列进行等值匹配的对象信息

#### 九、rows

- 预估的需要读取的记录条数，值越小越好

#### 十、filtered

- 某个表经过搜索条件过滤后剩余记录条数的百分比，值越大越好
- 如果使用的是索引执行的单表扫描，那么计算时需要估计出满足除使用到对应索引的搜索条件外的其他搜索条件的记录有多少条
- 对于单表查询，这个filtered列的值没有意义，更加关注连表查询中驱动表对应的执行计划的filtered值，它决定了被驱动表要执行的次数（即：rows*filtered）

#### 十一、Extra

- Extra列用来说明一些额外信息的，包含不适合在其他列中显示但十分重要的额外信息，可以根据这些额外信息来更准确的理解MySQL到底将如何执行给定的查询语句
- No tables used：当查询语句中没有FROM子句时将会提示该额外信息
- Impossible WHERE：查询语句中WHERE子句永远为FALSE时将会提示该额外信息
- Using where：使用全表扫描来执行对某个表的查询，并且该语句WHERE子句中有针对该表的搜索条件时将会提示该额外信息
- No matching min/max row：当查询列表处有MIN或MAX聚合函数，但是并没有符合WHERE子句中的搜索条件的记录时将会提示该额外信息
- Using index：当查询列表以及搜索条件中只包含属于某个索引的列，也就是在可以使用覆盖索引的情况下将会显示该额外信息
- Using index condition：有些搜索条件中虽然出现了索引项，但却不能使用到索引将提示该额外信息，比如模糊查询
- Using join buffer (Block Nested Loop)：在连接查询执行过程中，当被驱动表不能有效地利用索引加快访问速度，MySQL一般会为其分配一块名为join buffer的内存块来加快查询速度，也就是基于块的嵌套循环算法
- Not exists：当使用左连接时，如果WHERE子句中包含要求被驱动表的某个列等于NULL值的搜索条件，而且那个列又是不允许存储NULL值的，那么将提示该额外信息
- Using intersect(...) 、 Using union(...) 和 Using sort_union(...) ：如果执行计划的Extra列出现了Using intersect(...)提示，说明准备使用Intersect索引合并的方式执行查询，括号中的...表示需要进行索引合并的索引名称；如果出现Using union(...)提示，说明准备使用Union索引合并的方式执行查询；如果出现Using sort_union(...)提示，说明准备使用Sort_Union索引合并的方式执行查询
- Zero limit：当LIMIT子句的参数为0时，表示不打算从表中读取任何记录，将会提示该信息
- Using filesort：很多情况下排序操作无法使用到索引，只能在内存中（记录较少的时候）或者磁盘中（记录较多的时候）进行排序，MySQL把这种在内存中或者磁盘上进行排序的方式称为文件排序（filesort）。如果某个查询需要使用文件排序的方式执行查询，就会提示该额外信息
- Using temporary：在许多查询的执行过程中，MySQL可能会借助临时表来完成一些功能，比如去重、排序之类的，在执行许多包含DISTINICT、GROUP BY、UNION等子句的查询过程中，如果不能有效地利用索引来完成查询，MySQL很可能寻求通过建立内部的临时表来执行查询。如果查询中使用到了内部的临时表则会提示该额外信息。在执行计划中出翔Using temporary并不是一个好事，因为建立与维护临时表要付出很大成本的，所以最好能使用索引替代掉使用临时表

### 四、EXPLAIN小结

- EXPLAIN不考虑各种Cache
- EXPLAIN不能显示MySQL在执行查询时所作的优化工作
- EXPLAIN不会告诉我们关于触发器、存储过程的信息或者用户自定义函数对查询的影响情况
- 部分统计信息是估算的，并非精确值

### 五、EXPLAIN进阶使用

#### 一、EXPLAIN四种输出格式

1. 传统格式

   ```mysql
   EXPLAIN SELECT select_options
   ```

2. JSON格式

   ```mysql
   EXPLAIN FORMAT=JSON SELECT select_options
   ```

3. TREE格式

   ```mysql
   EXPLAIN FORMAT=TREE SELECT select_options
   ```

4. 可视化输出

#### 二、SHOW WARNINGS的使用

- 在使用EXPLAIN语句查看某个查询语句的执行计划后，紧接着还可以使用SHOW WARNINGS语句查看与这个查询的执行计划有关的一些扩展信息
- 此语句也可以查询到优化后SQL执行语句

# 十、索引优化与查询优化

## 一、查询优化的分类

虽然SQL查询优化的技术多种多样，但是大致可分为物理查询优化和逻辑查询优化

- 物理查询优化是通过索引和表连接方式等技术来进行优化，重点需要掌握索引的使用
- 逻辑查询优化是通过SQL等价变换提高查询效率，简而言之就是换一种执行效率更高的写法

## 二、索引失效场景

1. 非全值匹配导致索引失效
   - 当WHERE条件中有多个等值查询时，可以为这几个等值查询建立联合索引以提高查询速度
2. 非最佳左前缀法则导致索引失效
   - 最佳左前缀法则使用在联合索引中
   - 在联合索引中使用的第一个索引字段必须出现在WHERE条件中，否则索引失效。
   - 第一个索引字段与WHERE条件中的顺序无关，只要有就行
   - 如果WHERE条件中出现联合索引中第一个索引字段以及第三个索引字段，也就是中间未使用某些联合索引中字段，则联合索引只会匹配到被跳过的索引字段的前一个或前几个索引字段
3. 非主键插入顺序导致索引失效
   - 当自增主键中间缺失一定的数值时，再插入的数据就会插入到中间缺失的部分，那么则会造成页分裂情况
   - 建议让插入的记录的主键值依次递增，让主键具有AUTO_INCREMENT，让存储引擎自己为表生成主键
4. 计算、函数、类型转换（自动或手动）导致索引失效
   - 计算、函数、类型转换（自动或手动）可能甚至比模糊查询的效率更慢
5. 类型转换导致索引失效
   - 要查询数据与字段数据类型不一致会导致索引失效
   - JOIN连表查询时，连接的字段数据类型不一致可能也会导致索引失效
6. 范围条件右边的列索引失效
   - 范围条件右边指的是创建联合索引后，在WHERE条件中写了范围条件，但是在WHERE中的
   - 范围条件的位置无关，而是与创建联合索引时索引字段的位置有关
   - 如果想要带有范围条件的联合索引中所有的索引字段都起作用，则在创建索引时需要把可能会出现范围条件的索引字段放在最后
7. 不等于（!=或<>）导致索引失效
8. is null可以使用索引，is not null不可以使用索引
9. like以通配符%开头的索引失效
   - 如果匹配字符串的第一个字符为%，索引不会生效
   - 只有将%放在非首位，所以你才会生效
   - 页面搜索严禁左模糊或全模糊，如果需要则使用搜索引擎解决
10. OR前后存在非索引的列则会导致索引失效
    - OR前后存在非索引列则会导致索引失效
    - OR前后的列都是索引列则索引正常
11. 数据库和表的字符集统一使用utf8mb4
    - 统一字符集可以避免由于字符集转换产生的乱码，不同的字符集进行比较前需要进行转换会造成索引失效

## 三、针对索引失效的建议

1. 对于单列索引，尽量选择针对当前query过滤性更好的索引
2. 在选择组合索引时，当前query中过滤性最好的字段在索引字段顺序中，位置越靠前越好
3. 在选择组合索引时，尽量选择能够包含当前query中的where子句中更多字段的索引
4. 在选择组合索引时，如果某个字段可能出现范围查询时，尽量把这个字段放在索引次序的最后面

## 四、关联查询优化

### 一、采取左外连接

- 采取左外连接时，驱动表可能就是LEFT JOIN左侧的表，被驱动表可能就是LEFT JOIN右侧的表，在被动表中添加JOIN时所使用的字段索引可以避免全表扫描，而驱动表则无法避免全表扫描

### 二、采集内连接

- 采用内连接时，MySQL会自动选择有索引字段的表或者运行成本低的表作为驱动表以提高执行效率
- 对于内连接来说，查询优化器可以决定谁是驱动表，谁是被驱动表
- 对于内连接来说，如果表的连接条件中只能有一个字段有索引，则有索引的字段所在的表会被作为被驱动表
- 对于内连接来说，在两个表的连接条件都存在索引的情况下，会选择数据量小的的表作为驱动表，小表驱动大表

### 三、JOIN原理

#### 一、join连接原理分类

1. MySQL5.5版本之前只支持嵌套循环（Nested Loop Join），如果关联表的数据量很大，则join关联的执行时间会非常长
2. MySQL5.5版本之后使用BNLJ算法来优化嵌套执行

#### 二、驱动表与被驱动表

- 驱动表就是主表，被驱动表就是从表、非驱动表
- 是否为驱动表并不是看在JOIN的前后位置，而是由优化器所决定的

#### 三、简单嵌套循环连接

- 简单嵌套循环连接英文为Simple Nested-Loop Join
- 原理就是每次从驱动表中取出一条数据，每条数据再去与被驱动表进行全表遍历

#### 四、索引嵌套循环连接

- 索引嵌套循环连接英文为Index Nested-Loop Join
- 索引嵌套循环连接主要是为了减少内层表数据的匹配次数，所以要求被驱动表上必须有索引才行。通过外层表匹配条件直接与内层表索引进行匹配，避免和内层表的每条记录去进行比较，这样大大减少了对内层表的匹配次数

#### 五、块嵌套循环连接

- 块嵌套循环连接英文为Block Nested-Loop Join
- 块嵌套循环连接不再是逐条获取驱动表的数据，而是一块一块的获取，引入了join buffer缓冲区，将驱动表join相关的部分数据列（大小受join buffer的限制）缓存到join buffer中，然后全表扫描被驱动表，被驱动表的每一条记录一次性和join buffer中的所有驱动表记录进行匹配（内存中操作），将简单嵌套循环中的多次比较合并成一次，降低了被驱动表的访问频率
- join buffer缓存的不只是关联表的列，select后面的列也会缓存起来
- 在一个有N个join关联的sql中会分配N-1个join buffer，所以查询的时候尽量减少不必要的字段，可以让join buffer中可以存放更多的列

#### 六、JOIN结论

1. 整体效率比较：INLJ > BNLJ > SNLJ
2. 永远用小结果集（可能是大表通过WHERE过滤后的结果集）驱动大结果集（其本质就是减少外层循环的数据数量）（小的度量单位指的是表行数*每行大小）
3. 为被驱动表匹配的条件增加索引（减少内层表的循环匹配次数）
4. 增加join buffer size的大小（一次缓存的数据越多，那么内层包的扫描次数就越少）
5. 减少驱动表不必要的字段查询（字段越少，join buffer所缓存的数据就越多）
6. 需要join的字段，数据类型保持绝对一致
7. 能够直接多表关联的尽量直接关联，不用子查询（减少查询的趟数）
8. 不建议使用子查询，建议将子查询SQL拆开结合程序多次查询，或使用JOIN来代替子查询
9. 衍生表建不了索引

#### 七、Hash Join

1. 从MySQL的8.0.20版本开始将废弃BNLJ，而是使用了hash join作为默认的连接
2. Hash Join是做大数据集连接时的常用方法，优化器使用两个表中较小（相对较小）的表利用Join key在内存中建立散列表，然后扫描较大的表并探测散列表，找出与Hash表匹配的行

## 五、子查询优化

1. 子查询执行效率不高的原因
   - 执行子查询时，MySQL需要为内层查询语句的查询结果建立一个临时表，然后外层查询语句从临时表中查询记录。查询完毕后，再撤销这些临时表。这样会消耗过多的CPU和IO资源，产生大量的慢查询
   - 子查询的结果集存储的临时表，不论是内存临时表还是磁盘临时表都不会存在索引 ，所以查询性能会受到一定的影响
   - 对于返回结果集比较大的子查询，其对查询性能的影响也就越大
2. 使用连接（JOIN）查询来替代子查询
   - 使用连接查询不需要建立临时表，其速度比子查询要块，如果查询中使用索引的话，性能就会更好
   - 尽量不要使用NOT IN或者NOT EXISTS，用LEFT JOIN xxx WHERE xx IS NULL替代

## 六、排序优化

### 一、排序分类

1. Index排序：索引可以保证数据的有序性，不需要再进行排序，效率更高
2. FileSort排序：一般在内存中进行排序，占用CPU较多。如果待排结果较大，会产生临时文件I/O到磁盘进行排序的情况，效率较低

### 二、排序优化建议

1. SQL中，可以在WHERE子句和ORDER BY子句中使用索引，目的是在WHERE子句中避免全表扫描 ，在ORDER BY子句避免使用FileSort排序。当然，某些情况下全表扫描，或者FileSort排序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率
2. 尽量使用Index完成ORDER BY排序。如果WHERE和ORDER BY后面是相同的列就使用单索引列；如果不同就使用联合索引
3. 无法使用Index时，需要对FileSort方式进行调优
4. 当范围条件和group by或者order by的字段出现二选一时，优先观察条件字段的过滤数量，如果过滤的数据足够多，而需要排序的数据并不多时，优先把索引放在范围字段上。反之，亦然

### 三、排序索引失效场景

1. order by时不加limit，可能导致索引失效，但是使用覆盖索引，不加limit时，索引则会生效
2. order by时顺序错误，索引失效。即在联合索引中，违反最左匹配原则导致排序索引失效，此时联合索引中的字段可以分布在WHERE、ORDER BY、GROUP BY中、
3. order by时规则不一致，索引失效。即在联合索引中排序时字段的升降序与索引的升降序不一致导致索引失效，但是排序时所有字段与索引中的字段升降序都相反时则会命中排序索引
4. 无过滤，不索引
5. order by时使用了非联合索引中的字段进行了排序
6. 如果在order by中的字段的前面使用了多个相等条件（如IN过滤条件）则会导致索引失效

### 四、filesort算法

1. 双路排序（慢）
   - MySQL4.1之前是使用双路排序，字面意思就是两次扫描磁盘，最终得到数据，读取行指针和order by列，对他们进行排序，然后扫描已经排序好的列表，按照列表中的值重新从列表中读取对应的数据输出
   - 从磁盘取排序字段，在buffer进行排序，再从磁盘取其他字段
   - 取一批数据，要对磁盘进行两次扫描，众所周知，IO是很耗时的
2. 单路排序（快）
   - 从磁盘读取查询需要的所有列，按照order by列在buffer对它们进行排序，然后扫描排序后的列表进行输 出，它的效率更快一些，避免了第二次读取数据。并且把随机IO变成了顺序IO，但是它会使用更多的空 间， 因为它把每一行都保存在内存中了
3. filesort优化策略
   - 尝试提高sort_buffer_size
   - 尝试提高max_length_for_sort_data
   - order by时select *是一个大忌，最好只Query需要的字段

## 七、分组优化

- group by使用索引的原则几乎跟order by一致，group by即使没有过滤条件用到索引，也可以直接使用索引
- group by先排序再分组，遵照索引建的最佳左前缀法则
- 当无法使用索引列，增大max_length_for_sort_data和sort_buffer_size参数的设置
- where效率高于having，能写在where限定的条件就不要写在having中了
- 减少使用order by，和业务沟通能不排序就不排序，或将排序放到程序端去做。Order by、group by、distinct这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的
- 包含了order by、group by、distinct这些查询的语句，where条件过滤出来的结果集请保持在1000行以内，否则SQL会很慢

## 八、优化分页查询

1. 一般分页查询时，通过创建覆盖索引能够比较好地提高性能

2. 在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容

   ```mysql
   SELECT * FROM student t,(SELECT id FROM student ORDER BY id LIMIT 2000000,10) a
   WHERE t.id = a.id;
   ```

3. 适用于主键自增的表，可以把Limit 查询转换成某个位置的查询

   ```mysql
   SELECT * FROM student WHERE id > 2000000 LIMIT 10;
   ```

## 九、覆盖索引

### 一、覆盖索引的理解

1. 理解方式一：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。一个索引包含了满足查询结果的数据就叫做覆盖索引。覆盖索引可以在联合索引中生效，也可在单列索引中生效，也可在多个单例索引组合中生效
2. 理解方式二：非聚簇复合索引的一种形式，它包括在查询里的SELECT、JOIN和WHERE子句用到的所有列（即建索引的字段正好是覆盖查询条件中所涉及的字段，但是查询的字段不能多于建索引的字段，否则覆盖索引不生效）
3. 理解方式三：索引列+主键包含SELECT到FROM之间查询的列

### 二、覆盖索引的优点

1. 避免Innodb表进行索引的二次查询（回表）
   - InnoDB是以聚集索引的顺序来存储的，对于InnoDB来说，二级索引在叶子节点中所保存的是行的主键信息，如果是用二级索引查询数据，在查找到相应的键值后，还需要通过主键进行二次查询才能获取我们真实所需要的数据
   - 在覆盖索引中，二级索引的键值中可以获取所要的数据，避免了对主键的二次查询，减少了IO操作，提升了查询效率
2. 可以把随机IO变成顺序IO加快查询效率
   - 由于覆盖索引是按照键值的顺序存储的，对于IO密集型的范围来说，对比随机从磁盘读取每一行的数据IO要少的多，因此利用覆盖索引在访问时也可以把磁盘的随机读取的IO转变成索引查找的顺序IO
   - 由于覆盖索引可以减少树的搜索次数，显著提升查询性能，所以使用覆盖索引是一个常用的性能优化手段
3. 覆盖索引生效的情况下可能打破之前的一些规则，比如like以通配符%开头的索引失效、不等于查询导致索引失效的情况下可能被推翻，毕竟查询时要先考虑CPU和IO的消耗，再去考虑规则

### 三、覆盖索引的缺点

- 索引字段的维护总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要权衡考虑了。这是业务DBA，或者称为业务数据架构师的工作

## 十、为字符串添加索引

1. 前缀索引的意思就是指定索引字段的长度
2. 使用前缀索引，定义好长度，就可以做到既节省空间，又不用额外增加太多的查询成本。但是建立前缀索引时要考虑索引字段的长度，毕竟区分度越高越好，因为区分度越高，意味着重复的键值越少
3. 使用前缀索引就用不上覆盖索引对查询性能的优化了

## 十一、索引条件下推（ICP）

### 一、使用ICP扫描的过程

1. storage层：首先将index key条件满足的索引记录区间确定，然后在索引上使用index filter进行过滤。将满足的index filter条件的索引记录才去回表取出整行记录返回server层。不满足index filter条件的索引记录丢弃，不回 表、也不会返回server层
2. server层：对返回的数据，使用table filter条件做最后的过滤
3. 使用ICP前后的成本差别
   - 使用前，存储层多返回了需要被index filter过滤掉的整行记录 
   - 使用ICP后，直接就去掉了不满足index filter条件的记录，省去了他们回表和传递到server层的成本
   - ICP的加速效果取决于在存储引擎内通过ICP筛选掉的数据的比例

### 二、ICP使用条件

1. 只能用于二级索引(secondary index) 
2. explain显示的执行计划中type值（join 类型）为range、ref、eq_ref或者ref_or_null
3. 并非全部where条件都可以用ICP筛选，如果where条件的字段不在索引列中，还是要读取整表的记录到server端做where过滤
4. ICP可以用于MyISAM和InnnoDB存储引擎
5. MySQL5.6版本的不支持分区表的ICP功能，5.7版本的开始支持
6. 当SQL使用覆盖索引时，不支持ICP优化方法

## 十二、其他查询优化策略

### 一、EXISTS和IN的区别

- 选择EXISTS和IN的前提时索引，但是选择与否还是要看表的大小，可以理解为小表驱动大表
- 当驱动表小于被驱动表时，用EXISTS，因为EXISTS的实现相当于外表循环
- 当驱动表大于被驱动表时，用IN

### 二、COUNT效率比较

1. COUNT(*)和COUNT(1)都是对所有结果进行COUNT
2. COUNT(具体字段)只能统计此字段非空的数据行
   - 如果使用的是MyISAM存储引擎，则COUNT(*)、COUNT(1)和COUNT(expr)的效率是相同的，都是O(1)
   - 如果使用的是InnoDB存储引擎，则COUNT(*)=COUNT(1)>COUNT(expr)，采用的全表扫描的方式，时间复杂度为O(n)
3. 在InnoDB引擎中，如果采用COUNT(具体字段)来统计数据行数，尽量使用二级索引，因为主键采用的索引是聚簇索引，聚簇索引包含的信息多，明显会大于二级索引（非聚簇索引）；对于COUNT(*)和COUNT(1)来说，它们不需要查找具体的行，只是统计行数，系统会自动采用占用空间小的二级索引来进行统计，如果有多个二级索引，会使用key_len小的二级索引进行扫描，当没有二级索引的时候，才会采用主键索引来进行统计

### 三、关于SELECT(*)

- 在表查询时，建议明确字段，不要使用*作为查询的字段列表，推荐使用SELECT<字段列表>查询
  1. MySQL在解析过程中，会通过查询数据字典将*按序转成所有的列名，这会大大的耗费资源和时间
  2. 无法使用覆盖索引

### 四、LiMIT 1对优化的影响

- 针对的是会扫描全表的SQL语句，如果可以确定结果集只有一条，那么加上LIMIT 1的时候，当找到一条结果的时候就不会继续扫描了，这样会加快查询速度
- 如果数据表已经对字段建立了唯一索引，那么可以通过索引进行查询，不会全表扫描，就不需要加上LIMIT 1了

### 五、多使用COMMIT 

1. 只要有可能，在程序中尽量多使用COMMIT，这样程序的性能得到提高，需求也会因为COMMIT所释放的资源而减少
2. COMMIT所释放的资源
   - 回滚段上用于恢复数据的信息
   - 被程序语句获得的锁 
   - redo / undo log buffer 中的空间
   - 管理上述 3 种资源中的内部花费

## 十三、淘宝数据库主键的设计

### 一、自增ID的问题

1. 可靠性不高
   - 存在自增ID回溯的问题，这个问题直到最新版本的MySQL 8.0才修复
2. 安全性不高
   - 对外暴露的接口可以非常容易猜测对应的信息。比如：/User/1/这样的接口，可以非常容易猜测用户ID的值为多少，总用户数量有多少，也可以非常容易地通过接口进行数据的爬取
3. 性能差
   - 自增ID的性能较差，需要在数据库服务器端生成
4. 交互多
   - 业务还需要额外执行一次类似last_insert_id()的函数才能知道刚才插入的自增值，这需要多一次的网络交互。在海量并发的系统中，多1条SQL，就多一次性能上的开销
5. 局部唯一性
   - 最重要的一点，自增ID是局部唯一，只在当前数据库实例中唯一，而不是全局唯一，在任意服务器间都是唯一的。对于目前分布式系统来说，这简直就是噩梦

### 二、业务字段做主键

- 建议尽量不要用跟业务有关的字段做主键。毕竟，作为项目设计的技术人员，我们谁也无法预测在项目的整个生命周期中，哪个业务字段会因为项目的业务需求而有重复，或者重用之类的情况出现

### 三、淘宝主键的设计

- 订单ID = 时间 + 去重字段 + 用户ID的后6位尾号（仅是猜测）

### 四、推荐的主键设计

- 非核心业务：对应表的主键自增ID，如告警、日志、监控等信息
- 核心业务：主键设计至少应该是全局唯一且是单调递增。全局唯一保证在yi各系统之间都是唯一的，单调递增是希望插入时不影响数据库性能
- 推荐的主键设计：UUID
- UUID的特点：全局唯一，占用36字节，数据无序，插入性能差

# 十一、数据库的设计规范

## 一、范式

### 一、范式简介

- 在关系型数据库中，关于数据表设计的基本原则，规则就称为范式
- 范式的英文名称是Normal Form，简称NF
- 范式是关系型数据库理论的基础，也是我们在设计数据库结构过程中所要遵循的规则和指导方法

### 二、范式分类

按照范式的级别，从低往高。在满足第一范式的基础上进一步满足更多规范的称为第二范式，以此类推。遵循第三范式即可，也可能会反范式

1. 第一范式（1NF）
2. 第二范式（2NF）
3. 第三范式（3NF）
4. 巴斯-科德范式（BCNF）
5. 第四范式（4NF）
6. 第五范式（5NF，又称完美范式）

### 三、键和相关属性的概念

1. 数据库中的键（Key）由一个或者多个属性组成
   - 超键：能唯一标识元组的属性集叫做超键。可以理解为多个字段构成唯一标识，也可能是一个字段
   - 候选键：如果超键不包含多余的属性，那么这个超键就是候选键。可以理解为可以最小唯一标识的字段
   - 主键：用户可以从候选键中选择一个作为主键，主键也可能是多个字段的组合
   - 外键：如果数据表R1中的某属性集不是R1的主键，而是另一张表R2的主键，那么这个属性集就是数据表R1的外键
   - 主属性：包含在任一候选键中的属性称为主属性
   - 非主属性：与主属性相对，指的是不包含在任何一个候选键中的属性
2. 将候选键称之为码，把主键也称之为主码。键可能是由多个属性组成的，针对单个属性，还可以分为主属性和非主属性

### 四、第一范式

- 第一范式主要是确保数据表中每个字段的值必须具有原子性，也就是说数据表中每个字段的值为不可再次拆分的最小数据单元。也就是说表中的每个字段的值不可能再细化出去，创建子表了
- 事实上，任何的DBMS都会满足第一范式的要求，不会将字段进行拆分
- 属性的原子性是主观的，字段值拆不拆分需要具体项目需求具体分析

### 五、第二范式

- 第二范式要求，在满足第一范式的基础上，还要满足数据表里的每一条数据记录，都是可唯一标识的。而且所有非主键字段，都必须完成依赖主键，不能只依赖主键的一部分。尤其是在复合主键的情况下，非主键部分不应该依赖于部分主键
- 如果知道主键的所有属性的值，就可以检索到任何组（行）的任何属性的任何值。要求中的主键，其实可以扩展替换为候选键
- 第一范式告诉我们属性需要是原子性的，而第二范式告诉我们一张表就是一个独立的对象，一张表只表达一个意思
- 第二范式要求实体的属性完全依赖于主关键字。如果存在不完全依赖，那么这个属性和主关键字的一部分应该分离出来形成一个新的实体。新实体与原实体之间是一对多的关系

### 六、第三范式

- 第三范式是在第二范式的基础上，确保数据表中的每一个非主键字段都和主键字段直接相关。也就是说，要求数据表中的所有非主键字段不能依赖于其他非主键字段。通俗的讲，所有的非主键属性之间不能有依赖关系，必须相互独立
- 确保每列都和主键列直接相关，而不是间接相关

### 七、范式的优缺点

1. 优点
   - 数据的标准化有助于消除数据表中的数据冗余，第三范式通常被认为在性能、扩展性和数据完整性方面达到了最好的平衡
2. 缺点
   - 范式的使用，可能降低查询的效率。因为范式等级越高，设计出来的数据表就越多、越精细，数据的冗余度就越低，进行数据查询时就可能需要关联多张表，这不但代价高昂，也可能使一些索引策略失效
   - 范式只是提出了设计上的标准，开发中，会出现为了性能和读取效率违反范式化的原则

### 八、反范式化

#### 一、反范式概述

1. 为满足某种商业目标 , 数据库性能比规范化数据库更重要
2. 在数据规范化的同时 , 要综合考虑数据库的性能
3. 通过在给定的表中添加额外的字段，以大量减少需要从中搜索信息所需的时间
4. 通过在给定的表中插入计算列，以方便查询
5. 以空间换时间的方式，提高查询效率

#### 二、反范式新问题

1. 存储空间变大了
2.  一个表中字段做了修改，另一个表中冗余的字段也需要做同步修改，否则 数据不一致
3. 若采用存储过程来支持数据的更新、删除等额外操作，如果更新频繁，会非常消耗系统资源
4. 在数据量小的情况下，反范式不能体现性能的优势，可能还会让数据库的设计更加复杂 

#### 三、反范式适用场景

1. 当冗余信息有价值或者能大幅度提高查询效率的时候，我们才会采取反范式的优化
2. 增加冗余字段要符合的条件
   - 这个冗余字段不需要经常进行修改
   - 这个冗余字段查询的时候不可或缺
3. 历史快照、历史数据的需要
   - 保存操作日志中信息，可以适用冗余字段，用来保存历史快照、历史数据
   - 反范式优化也常用在数据仓库的设计中，因为数据仓库通常存储历史数据 ，对增删改的实时性要求不强，对历史数据的分析需求强。这时适当允许数据的冗余度，更方便进行数据分析

### 九、巴斯范式

- 若一个关系达到了第三方式，并且只有一个候选键，或者它的每个候选键都是单属性，则为巴斯范式
- 一般来说，一个数据库设计符合3NF或BCNF就可以了

### 十、第四范式

1. 多值依赖的概念
   - 多值依赖：即属性之间的一对多关系
   - 函数依赖：事实上是单值依赖，一对一关系
   - 平凡的多值依赖：全集U=K+A，一个K可以对应于多个A。此时整个表就是一组一对多关系。也就是某个字段对另一个字段是一对多的关系，该字段并对其他字段无一对多关系
   - 非平凡的多值依赖：全集U=K+A+B，一个K可以对应于多个A，也可以对应于多个B，A与B互相独立。此时整个表有多组一对多关系。也就是某个字段对另一个字段是一对多的关系，该字段并对其他字段还有一对多关系
2. 第四范式即在满足巴斯范式的基础上，消除非平凡且非函数依赖的多值依赖（即把同一表内的多对多关系删除，只保留平凡的多值依赖）

### 十一、第五范式

- 在满足第四范式的基础上，消除不是由候选键所蕴含的连接依赖。如果关系模式R中的每一个连接依赖均由R的候选键所隐含，则称此关系模式符合第五范式

## 二、ER模型

### 一、ER模型的要素

- 实体：可以看做是数据对象，往往对应于现实生活中的真实存在的个体。在ER模型中，用矩形来表示。实体分为两类，分别是强实体和弱实体。强实体是指不依赖于其他实体的实体；弱实体是指对另一个实体有很强的依赖关系的实体
- 属性：则是指实体的特性。在ER模型中用椭圆形来表示
- 关系：则是指实体之间的联系。在ER模型中用菱形来表示

### 二、关系的类型

- 一对一关系：指实体之间的关系是一一对应的
- 一对多关系：指一边的实体通过关系，可以对应多个另外一边的实体。相反，另一边的实体通过这个关系，则只能对应唯一的一边的实体
- 多对多关系：指关系两边的实体都可以通过关系对应多个对方的实体
- 在ER模型关系线上可以标记上“1”和“N”，代表一对一、一对多、多对多

### 三、 ER模型转数据表

1. 一个实体通常转换成一个数据表
2. 一个多对多的关系，通常也转换成一个数据表
3. 一个1对1，或者1对多的关系，往往通过表的外键来表达，而不是设计一个新的数据表
   - 外键约束主要是在数据库层面上保证数据的一致性，但是因为插入和更新数据需要检查外键，理论上性能会有所下降，对性能是负面影响
   - 实际项目中，不建议使用外键，一方面是降低开发的复杂度，另一方面是外键在处理数据时非常麻烦
4. 属性转换成表的字段

## 三、数据表设计原则

1. 数据表的个数越少越好
2. 数据表中的字段个数越少越好
3. 数据表中联合主键的字段个数越少越好
   - 联合主键中的字段越多，占用的索引空间越大，不仅会加大理解难度，还会增加运行时间和索引空间
4. 使用主键和外键越多越好 
   - 此外键非真正的外键，此处的外键只要表示连接关系的外键

## 四、数据库对象编写建议

### 一、关于库

1. 库的名称必须控制在32个字符以内，只能使用英文字母、数字和下划线，建议以英文字母开头
2. 库名中英文一律小写，不同单词采用下划线分割。须见名知意
3. 库的名称格式：业务系统名称_子系统名
4. 库名禁止使用关键字（如type,order等）
5. 创建数据库时必须 显式指定字符集，并且字符集只能是utf8或者utf8mb4
6. 对于程序连接数据库账号，遵循权限最小原则。使用数据库账号只能在一个DB下使用，不准跨库。程序使用的账号 原则上不准有drop权限
7. 临时库以tmp_为前缀，并以日期为后缀
8. 备份库以bak_为前缀，并以日期为后缀

### 二、关于表、列

1. 表和列的名称必须控制在32个字符以内，表名只能使用英文字母、数字和下划线，建议以英文字母开头
2. 表名、列名一律小写，不同单词采用下划线分割。须见名知意
3. 表名要求有模块名强相关，同一模块的表名尽量使用统一前缀
4. 创建表时必须显式指定字符集为utf8或utf8mb4
5. 表名、列名禁止使用关键字（如type,order等）
6. 创建表时必须 显式指定表存储引擎 类型。如无特殊需求，一律为InnoDB
7. 建表必须有comment
8. 字段命名应尽可能使用表达实际含义的英文单词或缩写
9. 布尔值类型的字段命名为is_描述
10. 禁止在数据库中存储图片、文件等大的二进制数据。通常文件很大，短时间内造成数据量快速增长，数据库进行数据库读取时，通常会进行大量的随机IO操作，文件很大时，IO操作很耗时。通常存储于文件服务器，数据库只存储文件地址信息
11. 建表时关于主键： 表必须有主键 
    - 强制要求主键为id，类型为int或bigint，且为auto_increment建议使用unsigned无符号型
    - 标识表里每一行主体的字段不要设为主键，建议设为其他字段如user_id，order_id等，并建立unique key索引。因为如果设为主键且主键值为随机插入，则会导致innodb内部页分裂和大量随机I/O，性能下降核心表（如用户表）必须有行数据的创建时间字段和最后更新时间字段，便于查问题
12. 表中所有字段尽量都是NOT NULL属性，业务可以根据需要定义DEFAULT值 。 因为使用NULL值会存在每一行都会占用额外存储空间、数据迁移容易出错、聚合函数计算结果偏差等问题
13. 所有存储相同数据的列名和列类型必须一致 （一般作为关联列，如果查询时关联列类型不一致会自动进行数据类型隐式转换，会造成列上的索引失效，导致查询效率降低）
14. 中间表（或临时表）用于保留中间结果集，名称以 tmp_ 开头
15. 备份表用于备份或抓取源表快照，名称以 bak_ 开头。中间表和备份表定期清理

### 三、关于索引

1. InnoDB表必须主键为id int/bigint auto_increment，且主键值禁止被更新
2. InnoDB和MyISAM存储引擎表，索引类型必须为BTREE
3. 主键的名称以pk_开头，唯一键以uni_或uk_开头，普通索引以idx_开头，一律使用小写格式，以字段的名称或缩写作为后缀
4. 多单词组成的columnname，取前几个单词首字母，加末单词组成column_name。如: sample表member_id上的索引：idx_sample_mid
5. 单个表上的索引个数不能超过6个
6. 在建立索引时，多考虑建立 联合索引 ，并把区分度最高的字段放在最前面
7. 在多表 OIN的SQL里，保证被驱动表的连接列上有索引，这样JOIN 执行效率最高
8. 建表或加索引时，保证表里互相不存在冗余索引

### 四、SQL编写

1. 程序端SELECT语句必须指定具体字段名称，禁止写成 *
2. 程序端insert语句指定具体字段名称，不要写成INSERT INTO t1 VALUES(…)
3. 除静态表或小表（100行以内），DML语句必须有WHERE条件，且使用索引查找
4. INSERT INTO…VALUES(XX),(XX),(XX).. 这里XX的值不要超过5000个。 值过多虽然上线很快，但会引起主从同步延迟
5. SELECT语句不要使用UNION，推荐使用UNION ALL，并且UNION子句个数限制在5个以内 
6. 线上环境，多表 JOIN 不要超过5个表
7. 减少使用ORDER BY，和业务沟通能不排序就不排序，或将排序放到程序端去做。ORDER BY、GROUP BY、DISTINCT 这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的
8. 包含了ORDER BY、GROUP BY、DISTINCT这些查询的语句，WHERE条件过滤出来的结果集请保持在1000行以内，否则SQL会很慢
9. 对单表的多次alter操作必须合并为一次。对于超过100W行的大表进行alter table，必须经过DBA审核，并在业务低峰期执行，多个alter需整合在一起。 因为alter table会产生 表锁 ，期间阻塞对于该表的所有写入，对于业务可能会产生极大影响
10. 批量操作数据时，需要控制事务处理间隔时间，进行必要的sleep
11. 事务里包含SQL不超过5个。因为过长的事务会导致锁数据较久，MySQL内部缓存、连接消耗过多等问题
12. 事务里更新语句尽量基于主键或UNIQUE KEY，否则会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁

## 五、PowerDesigner的使用

- PowerDesigner是数据库建模工具，可以制作数据流程图、概念数据模型、物理数据模型
- 常用的模型分类：概念模型(CDM Conceptual Data Model) ，物理模型（PDM,Physical Data Model）， 面向对象的模型（OOM Objcet Oriented Model）和 业务模型（BPM Business Process Model）
- 建立好概念模型或者物理模型后，使用PowerDesigner页面上方的Database则会生成SQL语句

# 十二、数据库其他调优策略

## 一、数据库调优措施

### 一、调优的目标

1. 尽可能节省系统资源 ，以便系统可以提供更大负荷的服务（吞吐量更大）
2. 合理的结构设计和参数调整，以提高用户操作响应的速度（响应速度更快）
3. 减少系统的瓶颈，提高MySQL数据库整体的性能 

### 二、定位调优问题步骤

1. 用户的反馈
2. 日志分析
3. 服务器资源使用监控
4. 数据库内部状况监控
5. 其他

### 三、调优的维度和步骤

1. 选择适合的DBMS
2. 优化表设计
3. 优化逻辑查询
4. 优化物理查询
   - 物理查询优化是在确定了逻辑查询优化之后，采用物理优化技术（比如索引等），通过计算代价模型对 各种可能的访问路径进行估算，从而找到执行方式中代价最小的作为执行计划。在这个部分中，我们需 要掌握的重点是对索引的创建和使用
5. 使用Redis或Memcached作为缓存
   - 键值存储数据库（Redis和Memcached等）可以将数据存放到内存中，以提高查询速度
6. 库级优化
   - 读写分离（一主一从模式、双主双从模式）
   - 数据分片

## 二、优化MySQL服务器

### 一、优化服务器硬件

服务器的硬件性能直接决定着MySQL数据库的性能。硬件的性能瓶颈直接决定MySQL数据库的运行速度和效率。针对性能瓶颈提高硬件配置，可以提高MySQL数据库查询、更新的速度

1. 配置较大的内存
2. 配置高速磁盘系统
3. 合理分布磁盘I/O
4. 配置多处理器

### 二、优化MySQL参数

1. innodb_buffer_pool_size：这个参数是Mysql数据库最重要的参数之一，表示InnoDB类型的表和索引的最大缓存。它不仅仅缓存 索引数据，还会缓存表的数据。这个值越大，查询的速度就会越快。但是这个值太大会影响操作系统的性能
2. key_buffer_size：表示索引缓冲区的大小。索引缓冲区是所有的线程共享 。增加索引缓冲区可以得到更好处理的索引（对所有读和多重写）。当然，这个值不是越大越好，它的大小取决于内存的大小。如果这个值太大，就会导致操作系统频繁换页，也会降低系统性能。对于内存在4GB左右的服务器该参数可设置为256M或384M
3. table_cache：表示同时打开的表的个数。这个值越大，能够同时打开的表的个数越多。物理内存越大，设置就越大。默认为2402，调到512-1024最佳。这个值不是越大越好，因为同时打开的表太多会影响操作系统的性能
4. query_cache_size：表示查询缓冲区的大小 。可以通过在MySQL控制台观察，如果Qcache_lowmem_prunes的值非常大，则表明经常出现缓冲不够的情况，就要增加Query_cache_size的值；如果Qcache_hits的值非常大，则表明查询缓冲使用非常频繁，如果该值较小反而会影响效 率，那么可以考虑不用查询缓存；Qcache_free_blocks，如果该值非常大，则表明缓冲区中碎片很多。MySQL8.0之后失效。该参数需要和query_cache_type配合使用
5. query_cache_type的值是0时，所有的查询都不使用查询缓存区。但是query_cache_type=0并不会导致MySQL释放query_cache_size所配置的缓存区内存
   - 当query_cache_type=1时，所有的查询都将使用查询缓存区，除非在查询语句中指定 SQL_NO_CACHE，如SELECT SQL_NO_CACHE * FROM tbl_name
   - 当query_cache_type=2时，只有在查询语句中使用SQL_CACHE关键字，查询才会使用查询缓存区。使用查询缓存区可以提高查询的速度，这种方式只适用于修改操作少且经常执行相同的查询操作的情况
6. sort_buffer_size：表示每个需要进行排序的线程分配的缓冲区的大小。增加这个参数的值可以提高ORDER BY或 GROUP BY操作的速度。默认数值是2097144字节（约2MB）。对于内存在4GB左右的服务器推荐设置为6-8M，如果有100个连接，那么实际分配的总共排序缓冲区大小为100 × 6 ＝ 600MB
7. join_buffer_size = 8M：表示 联合查询操作所能使用的缓冲区大小 ，和sort_buffer_size一样，该参数对应的分配内存也是每个连接独享
8. read_buffer_size：表示 每个线程连续扫描时为扫描的每个表分配的缓冲区的大小（字节）。当线程从表中连续读取记录时需要用到这个缓冲区。SET SESSION read_buffer_size=n可以临时设置该参数的值。默认为64K，可以设置为4M
9. innodb_flush_log_at_trx_commit：表示 何时将缓冲区的数据写入日志文件 ，并且将日志文件写入磁盘中。该参数对于innoDB引擎非常重要。该参数有3个值，分别为0、1和2。该参数的默认值为1
   - 值为0时，表示每秒1次的频率将数据写入日志文件并将日志文件写入磁盘。每个事务的commit并不会触发前面的任何操作。该模式速度最快，但不太安全，mysqld进程的崩溃会导致上一秒钟所有事务数据的丢失
   - 值为1时，表示每次提交事务时将数据写入日志文件并将日志文件写入磁盘进行同步。该模式是最安全的，但也是最慢的一种方式。因为每次事务提交或事务外的指令都需要把日志写入（flush）硬盘
   - 值为2时，表示每次提交事务时将数据写入日志文件， 每隔1秒将日志文件写入磁盘。该模式速度较快，也比0安全，只有在操作系统崩溃或者系统断电的情况下，上一秒钟所有事务数据才可能丢失
10. innodb_log_buffer_size：这是InnoDB存储引擎的事务日志所使用的缓冲区。为了提高性能，也是先将信息写入 Innodb Log Buffer 中，当满足 innodb_flush_log_trx_commit 参数所设置的相应条件（或者日志缓冲区写满）之后，才会将日志写到文件（或者同步到磁盘）中
11. max_connections：表示允许连接到MySQL数据库的最大数量，默认值是151 。如果状态变量 connection_errors_max_connections不为零，并且一直增长，则说明不断有连接请求因数据库连接数已达到允许最大值而失败，这是可以考虑增大max_connections 的值。在Linux 平台下，性能好的服务器，支持 500-1000 个连接不是难事，需要根据服务器性能进行评估设定。这个连接数不是越大越好 ，因为这些连接会浪费内存的资源。过多的连接可能会导致MySQL服务器僵死
12. back_log：用于 控制MySQL监听TCP端口时设置的积压请求栈大小 。如果MySql的连接数达到 max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源，将会报错。5.6.6 版本之前默认值为50， 之后的版本默认为 50 + （max_connections / 5）， 对于Linux系统推荐设置为小于512的整数，但最大不超过900。如果需要数据库在较短的时间内处理大量连接请求， 可以考虑适当增大back_log的值
13. thread_cache_size： 线程池缓存线程数量的大小 ，当客户端断开连接后将当前线程缓存起来，当在接到新的连接请求时快速响应无需创建新的线程 。这尤其对那些使用短连接的应用程序来说可以极大的提高创建连接的效率。那么为了提高性能可以增大该参数的值。默认为60，可以设置为120。当 Threads_cached 越来越少，但 Threads_connected始终不降，且Threads_created持续升高，可适当增加thread_cache_size的大小
14. wait_timeout：指定 一个请求的最大连接时间，对于4GB左右内存的服务器可以设置为5-10
15. interactive_timeout：表示服务器在关闭连接前等待行动的秒数

## 三、优化数据库结构

1. 拆分表：冷热数据分离
   - 冷数据：不常用的字段
   - 热数据：常用的字段
2. 增加中间表
3. 增加冗余字段
4. 优化数据类型
   - 对整数类型数据进行优化。常用的整数型数据类型用INT，对于非负数数据，优先使用无符号整型UNSIGNED来存储。因为无符号相对于有符号，同样的字节数，存储的数值范围更大。如tinyint有符号为-128-127，无符号为0-255，多出一倍的存储空间、
   - 既可以使用文本类型也可以使用整数类型的字段，要选择使用整数类型。跟文本类型数据相比，大整数往往占用更少的存储空间，因此，在存取和比对的时候，可以占用更少的内存空间。所以，在二者皆可用的情况下，尽量使用整数类型，这样可以提高查询的效率
   - 避免使用TEXT、BLOB数据类型。MySQL的内存临时表不支持TEXT、BLOB这样的大数据类型，如果查询中包含这样的数据，在排序等操作时，就不能使用临时表，必须使用磁盘临时表。并且对于这种数据，MySQL还是要进行二次查询，会使SQL性能变得很差，但是不是说一定不能使用这样的数据类型。如果一定要使用，建议把TEXT、BLOB列分离到单独的扩展表中，并且查询时一定不要使用select *
   - 避免使用ENUM类型。修改ENUM值需要使用ALTER语句。ENUM类型的ORDER BY操作效率低，需要额外操作，可以使用TINYINT来替代ENUM类型
   - 使用TIMESTAMP存储时间
   - 用DECIMAL代替FLOAT和DOUBLE存储精确浮点数
5. 优化插入记录的速度
   -  MyISAM引擎的表
     - 禁用索引
     - 禁用唯一性索引
     - 使用批量插入
     - 使用LOAD DATA INFILE批量导入
   - InnoDB引擎的表
     - 禁用唯一性检查
     - 禁用外键检查
     - 禁止自动提交
6. 使用非空约束
7. 分析表、检查表与优化表

## 四、大表优化

1. 限制查询的范围

   - 禁止不带任何限制数据范围条件的查询语句

2. 读/写分离：主库负责写，从库负责读

   - 一主一从模式

     ![MySQL主从分离一主一从模式](../../../TyporaImage/MySQL%E4%B8%BB%E4%BB%8E%E5%88%86%E7%A6%BB%E4%B8%80%E4%B8%BB%E4%B8%80%E4%BB%8E%E6%A8%A1%E5%BC%8F.png)

   - 双主双从模式

     ![MySQL主从分离双主双从模式](../../../TyporaImage/MySQL%E4%B8%BB%E4%BB%8E%E5%88%86%E7%A6%BB%E5%8F%8C%E4%B8%BB%E5%8F%8C%E4%BB%8E%E6%A8%A1%E5%BC%8F.png)

3. 垂直拆分

   - 当数据量级达到千万级以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上， 减少对单一数据库服务器的访问压力。采用分库分表
   - 如果数据库中的数据表过多，可以采用垂直分库的方式，将关联的数据表（同一个模块的数据表）部署在同一个数据库中，不同模块的数据表放在不同的数据库中
   - 如果数据表中的列过多，可以采用垂直分表的方式，将一张数据表分拆成多张数据表，把经常一起使用的列放到同一张表里
   - 垂直拆分的优点：可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区 可以简化表的结构，易于维护
   - 垂直拆分的缺点：主键会出现冗余，需要管理冗余列，并会引起JOIN操作。此外，垂直拆分会让事务变得更加复杂

4. 水平拆分

   - 可以按照数据的使用程度，创建的日期等等进行数据的分割，把同一个结构表中数据放在不同不同的数据表中即为水平拆分

5. 数据库分片方案

   - 客户端代理：分片逻辑在应用端，封装在jar包中，通过修改或者封装JDBC层来实现。当当网的 Sharding-JDBC 、阿里的TDDL是两种比较常用的实现
   - 中间件代理：在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中。我们现在谈的 Mycat、360的Atlas、网易的DDB等等都是这种架构的实现

## 五、其他调优策略

1. 服务器语句超时处理
   - 在MySQL8.0中可以设置服务器语句超时的限制，单位可以达到毫秒级别。当中断的执行语句超过设置的毫秒数后，服务器将终止查询影响不大的事务或连接，然后将错误报给客户端
2. 创建全局通用表空间
3. MySQL 8.0新特性：隐藏索引（设置invisible）对调优的帮助，前面有记录过

# 十三、事务基础知识

## 一、事务基本概念

1. 支持事物的存储引擎：InnoDB存储引擎支持事物，而MyISAM则不支持事务
2. 事务的概念：一组逻辑操作单元（也就是具体的SQL操作语句），使数据从一种状态变换到另一种状态
3. 事务处理原则：保证所有事务都作为一个工作单元来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都被提交( commit )，那么这些修改就永久地保存下来；要么数据库管理系统将 放弃 所作的所有 修改 ，整个事务回滚( rollback )到最初状态

## 二、事务的特性（ACID）

1. 原子性（atomicity）
   - 原子性是指事务是一个不可分割的工作单位，要么全部提交，要么全部失败回滚，没有中间状态
2. 一致性（consistency）
   - 根据定义，一致性是指事务执行前后，数据从一个合法性状态变换到另外一个合法性状态。这种状态是语义上的而不是语法上的，跟具体的业务有关
   - 满足预定的约束的状态就叫做合法的状态。通俗一点，这状态是由你自己来定义的（比如满足现实世界中的约束）。满足这个状态，数据就是一致的，不满足这个状态，数据就是不一致的！如果事务中的某个操作失败了，系统就会自动撤销当前正在执行的事务，返回到事务操作之前的状态
3. 隔离型（isolation）
   - 事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰
4. 持久性（durability）
   - 持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响
   - 持久性是通过事务日志来保证的。日志包括了重做日志和回滚日志。当我们通过事务对数据进行修改的时候，首先会将数据库的变化信息记录到重做日志中，然后再对数据库中对应的行进行修改。这样做的好处是，即使数据库系统崩溃，数据库重启后也能找到没有更新到数据库系统中的重做日志，重新执行，从而使事务具有持久性
5. 总结
   - ACID是事务的四大特性，原子性是基础，隔离性是手段，一致性是约束条件，持久性是最终目的
   - 数据库事务，就是把需要保证原子性、隔离性、一致性和持久性的一个或多个数据库操作

## 三、事务的状态

![MySQL事务状态](../../../TyporaImage/MySQL事务状态.png)

1. 活动的（active）
   - 事务对应的数据库操作正在执行过程中时，我们就说该事务处在活动的状态
2. 部分提交的（partially committed）
   - 当事务中的最后一个操作执行完成，但由于操作都在内存中执行，所造成的影响并没有刷新到磁盘时，我们就说该事务处在部分提交的状态
3. 失败的（failed）
   - 当事务处在活动的或者部分提交的状态时，可能遇到了某些错误（数据库自身的错误、操作系统错误或者直接断电等）而无法继续执行，或者人为的停止当前事务的执行，我们就说该事务处在失败的状态
4. 中止的（aborted、rollback）
   - 如果事务执行了一部分而变为失败的状态，那么就需要把已经修改的事务中的操作还原到事务执行前的状态。换句话说，就是要撤销失败事务对当前数据库造成的影响。我们把这个撤销的过程称之为回滚。当回滚操作执行完毕时，也就是数据库恢复到了执行事务之前的状态，我们就说该事务处在了中止的状态
5. 提交的（committed）
   - 当一个处在部分提交的状态的事务将修改过的数据都同步到磁盘上之后，我们就可以说该事务处在了提交的状态
6. 事务的最终状态只能是提交的和中止的

## 四、显示事务和隐式事务

### 一、显示事务

1. 开启显示事务：START TRANSACTION 或者 BEGIN（此处的BEGIN可以单独使用，不同于存储函数和存储过程中的BEGIN），TRANSACTION 可跟的修饰符有，

   - READ ONLY：标识当前事务是一个只读事务，也就是属于该事务的数据库操作只能读取数据，而不能修改数据。但是只读事务也可以对临时表进行增、删、改
   - READ WRITE：标识当前事务是一个读写事务 ，也就是属于该事务的数据库操作既可以读取数据，也可以修改数据。默认的，READ ONLY和READ WRITE只能任选其一
   - WITH CONSISTENT SNAPSHOT：启动一致性读

2. 保存点：ROLLBACK TO [SAVEPOINT]。保存点不是最终的事务状态，而是将数据保存在这个保存点中，方便ROLLBACK后续操作

   ```mysql
   #提交事务，当提交事务后，对数据库的修改是永久的
   COMMIT;
   
   #回滚事务，即撤销正在进行的所有没有提交的修改
   ROLLBACK;
   
   #将事务回滚到某个保存点
   ROLLBACK TO [SAVEPOINT];
   
   #在事务中创建保存点，方便后续针对保存点进行回滚，一个事务中可以存在多个保存点
   SAVEPOINT 保存点名称;
   
   #删除某个保存点
   RELEASE SAVEPOINT 保存点名称;
   ```

### 二、隐式事务

MySQL中有一个系统变量autocommit，默认值为ON，即会自动提交。只会对DML语句生效

```mysql
#关闭自动提交
SET autocommit = OFF;
#或
SET autocommit = 0;
```

在autocommit值为ON的情况下，显式的的使用START TRANSACTION或者BEGIN语句开启一个事务，这样在本次事务提交或者回滚前也会暂时关闭掉自动提交的功能

### 三、隐式提交数据的情况

1. 数据定义语言（DDL）
2. 隐式使用或修改mysql数据库中的表
3. 事务控制或关于锁定的语句
   - 当我们在一个事务还没提交或者回滚时就又使用START TRANSACTION或者BEGIN语句开启了另一个事务时，会隐式的提交上一个事务
   - 当前的autocommit系统变量的值为OFF，我们手动把它调为ON时，也会隐式的提交前边语句所属的事务
   - 使用LOCK TABLES、UNLOCK TABLES等关于锁定的语句也会隐式的提交前边语句所属的事务
4. 加载数据的语句
5. 关于MySQL复制的语句
6. 其他的一些语句

## 五、事务的隔离级别

### 一、数据的并发问题

1. 脏写（Dirty Write）
   - 对于两个事务Session A、Session B，如果事务Session A修改了另一个未提交事务Session B修改过的数据，那就意味着发生了脏写
2. 脏读（Dirty Read）
   - 对于两个事务Session A、Session B，Session A读取了已经被Session B更新但还没有被提交的字段。之后若Session B回滚，Session A读取的内容就是临时且无效的
3. 不可重复读（Non**-**Repeatable Read ）
   - 对于两个事务Session A、Session B，Session A读取 了一个字段，然后Session B更新了该字段。 之后Session A再次读取同一个字段，值就不同了。那就意味着发生了不可重复读
4.  幻读（Phantom）
   - 对于两个事务Session A、Session B，Session A从一个表中读取 了一个字段，然后Session B在该表中 插入（只能是插入，删除不算）了一些新的行。 之后，如果Session A再次读取同一个表，就会多出几行。那就意味着发生了幻读
   - 幻读只是重点强调读取到了之前读取没有获取到的记录
5. 并发问题的严重性排序：脏写 > 脏读 > 不可重复读 > 幻读 

### 二、SQL隔离级别

舍弃一部分隔离性来换取一部分性能在这里就体现在：设立一些隔离级别，隔离级别越低，并发问题发生的就越多，隔离级别越高，数据库的并发性能越差，这4个隔离级别都可解决脏读的问题。SQL标准中设立了4个隔离级别

1. READ UNCOMMITTED：读未提交，在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。不能避免脏读、不可重复读、幻读
2. READ COMMITTED：读已提交，它满足了隔离的简单定义，即一个事务只能看见已经提交事务所做的改变。这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。可以避免脏读，但不可重复读、幻读问题仍然存在
3. REPEATABLE READ：可重复读，事务A在读到一条数据之后，此时事务B对该数据进行了修改并提交，那么事务A再读该数据，读到的还是原来的内容。可以避免脏读、不可重复读，但幻读问题仍然存在。这是MySQL的默认隔离级别
4. SERIALIZABLE：可串行化，确保事务可以从一个表中读取相同的行。在这个事务持续期间，禁止其他事务对该表执行插入、更新和删除操作。所有的并发问题都可以避免，但性能十分低下。能避免脏读、不可重复读和幻读

### 三、设置事务的隔离级别

```mysql
SET [GLOBAL|SESSION] TRANSACTION ISOLATION LEVEL 隔离级别;
#其中，隔离级别格式：
> READ UNCOMMITTED
> READ COMMITTED
> REPEATABLE READ
> SERIALIZABLE

#或

SET [GLOBAL|SESSION] TRANSACTION_ISOLATION = '隔离级别'
#其中，隔离级别格式：
> READ-UNCOMMITTED
> READ-COMMITTED
> REPEATABLE-READ
> SERIALIZABLE
```

关于设置时使用GLOBAL或SESSION的影响

1. 使用GLOBAL关键字（在全局范围影响）
   - 当前已经存在的会话无效
   - 只对执行完该语句之后产生的会话起作用
2. 使用SESSION关键字（在会话范围影响）
   - 对当前会话的所有后续的事务有效
   - 如果在事务之间执行，则对后续的事务有效
   - 该语句可以在已经开启的事务中间执行，但不会影响当前正在执行的事务
3. 数据库规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱

# 十四、MySQL事务日志

## 一、事务日志的分类

1. 事务的隔离性由锁机制实现
2. 而事务的原子性、一致性和持久性由事务的redo日志和undo日志来保证
   - REDO LOG称为重做日志，提供再写入操作，恢复提交事务修改的页操作，用来保证事务的持久性
   - UNDO LOG称为回滚日志，回滚行记录到某个特定版本，用来保证事务的原子性、一致性
3. 有的DBA或许会认为UNDO是REDO的逆过程，其实不然，REDO和UNDO都可以视为一种恢复操作
   - redo log：是存储引擎（InnoDB）生成的日志，记录的是物理级别上的页的修改操作，比如页号、偏移量、写入的数据。主要是保证数据的可靠性。使用的技术是Write-Ahead Log(预先日志持久化)，在持久化一个数据页之前，先将内存中相应的日志页持久化
   - undo log：是存储引擎（InnoDB）生成的日志，记录的是逻辑操作日志，比如对某一行数据进行了INSERT语句操作，那么undo log就记录一条与之相反的DELETE操作。主要用于事务的回滚（undo log记录的是每个修改操作的逆操作）和一致性非锁定读（undo log回滚行记录到某种特定的版本，即多版本并发控制，MVCC）

## 二、redo日志

1. InnoDB存储引擎是以页为单位来管理存储空间的。在真正访问页面之前，需要把在磁盘上的页缓存到内存中Buffer Pool之后才可以访问到。所有的变更都必须先更新缓冲池中的数据，然后缓冲池中的脏页（数据只存在内存中，还未刷新到磁盘上，即脏页）会以一定的频率被刷入磁盘（checkPoint机制），通过缓冲池来优化CPU和磁盘之间的鸿沟，这样就可以保证整体性能不会下降太快
2. redo日志可以简单理解为即使数据按照一定的频率把以commit的数据从内存刷新到磁盘中，也会在redo日志文件中做一次备份（增、删、改操作日志备份，并不是数据备份，备份的时候可能还未commit），防止数据库服务器在宕机等紧急情况下发生数据的丢失，不能保证数据的持久性。这样即使在服务器宕机后，也会从redo日志文件中去读取数据，保证数据的持久性
3. redo日志的好处
   - redo日志降低了刷盘频率
   - redo日志占用的空间非常小
4. redo日志的特点
   - redo日志是顺序写入磁盘的
   - 事务执行过程中，redo log不断记录
5. redo日志的组成
   - 重做日志的缓冲（redo log buffer），保存在内存中，是易失的
   - 重做日志文件（redo log file），保存在磁盘中，是持久的
6. redo整体步骤
   - 先将原始数据从磁盘中读入内存中来，修改数据的内存拷贝
   - 生成一条重做日志并写入redo log buffer，记录的是数据被修改后的值 
   - 当事务commit时，将redo log buffer中的内容刷新redo log file，对redo log file采用追加写的方式
   - 定期将内存中修改的数据刷新到磁盘中
7. redo刷盘策略
   - redo刷盘策略使用的步骤在将redo log buffer中的内容刷新到redo log file中
   - redo log buffer刷盘到redo log file的过程并不是真正的刷到磁盘中去，只是刷入到文件系统缓存（page cache）中去（这是现代操作系统为了提高文件写入效率做的一个优化），真正的写入会交给系统自己来决定（比如page cache足够大了）
   - InnoDB给出innodb_flush_log_at_trx_commit参数，该参数控制commit提交事务时，如何将redo log buffer中的日志刷新到redo log file中。设置为0，表示每次事务提交时不进行刷盘操作（系统默认master thread每隔1s进行一次重做日志的同步）；设置为1，表示每次事务提交时都将进行同步，刷盘操作（默认值），设置为2，表示每次事务提交时都只把redo log buffer内容写入page cache，不进行同步。由os自己决定什么时候同步到磁盘文件
8. 写入redo log buffer过程
   - MySQL把对底层页面中的一次原子（不可再拆分）访问的过程称之为一个Mini-Transaction，简称mtr、一个事务可以包含若干语句，每一条语句是由若干mtr组成，每一个mtr又可以包含若干条redo日志
   - 向log buffer中写入redo日志的过程是顺序的，也就是先往前边的block中写，当该block的空闲用完之后再往下一个block中写。所以InnoDB提供了一个buf-free的全局变量，该变量指明后续写入的redo日志应该写入到log buffer中的位置
   - 一个mtr执行过程中可能产生若干条redo日志，这些redo日志是一个不可分割的组（同一个mtr的redo日志的位置相邻），所以并不是每生成一条redo日志，就将其插入到log buffer中，而是每个mtr运行过程中产生的日志先暂时存到一个地方，当该mtr结束的时候，将过程中产生的一组redo日志再全部复制到log buffer中 
   - 不同的事物可能是并发执行的，每当一个mtr执行完成时，伴随该mtr生成的一组redo日志就需要被复制到log buffer中，也就是说不同事物的mtr可能时交替写入log buffer的
   - 一个redo log block是由日志头、日志体、日志尾组成。日志头占用12字节，日志尾占用8字节，所以一个block真正能存储的数据就是512-12-8=492字节。一个block设计成512字节的原因时机械磁盘默认的扇区就是512字节，也就是一个block对应一个扇区
9. 写入redo log file过程

## 三、undo日志

1. redo log是事务持久性的保证，undo log是事务原子性的保证。在事务中更新数据前要先写入一个undo log

2. undo的理解

   - 对记录进行增、删、改之前都要把回滚所需的东西记录下来，记录的不是数据本身，而是操作。比如插入时undo日志会记录一个该记录的删除操作，删除时undo日志会记录一个该记录的插入操作，修改时undo日志会记录一个该记录相反的update操作，将修改前的行放回去
   - MySQL把这些为了回滚而记录的这些内容称之为撤销日志或者回滚日志（即undo log）。但是并不会记录查询操作，因为查询操作不会修改任何数据
   - undo log会产生redo log，也就是undo log的产生会伴随着redo log的产生，这是因为undo log也需要持久性的保护

3. undo的作用

   - 回滚数据，undo是逻辑日志，只是将数据库逻辑地恢复到原来的样子，所有修改都被逻辑取消了，但是数据结构和页本身在回滚之后可能大不相同。在多用户并发系统中，数据库的主要任务就是协调对数据记录的并发访问，因此不能将一个页回滚到事务开始的样子，因为这样会影响其他事务正在进行的工作
   - MVCC，当用户读取一行记录时，若该记录已经被其他事务占用，当前事务可以通过undo读取之前的行版本信息，以此实现非锁定读取

4. undo的存储结构

   - 回滚段与undo页：InnoDB对undo log的管理采用段的方式，也就是回滚段（rollback segment）。每个回滚段记录了1024个undo log segment，而在每个undo log segment段中进行undo页的申请

   - 回滚段与事务

     ```mysql
     1. 每个事务只会使用一个回滚段，一个回滚段在同一时刻可能会服务于多个事务。
     2. 当一个事务开始的时候，会制定一个回滚段，在事务进行的过程中，当数据被修改时，原始的数据会被复制到回滚段。
     3. 在回滚段中，事务会不断填充盘区，直到事务结束或所有的空间被用完。如果当前的盘区不够用，事务会在段中请求扩展下一个盘区，如果所有已分配的盘区都被用完，事务会覆盖最初的盘区或者在回滚段允许的情况下扩展新的盘区来使用。
     4. 回滚段存在于undo表空间中，在数据库中可以存在多个undo表空间，但同一时刻只能使用一个undo表空间。
     5. 当事务提交时，InnoDB存储引擎会做以下两件事情：
     	将undo log放入列表中，以供之后的清洗操作
     	判断undo log所在的页是否可以重用，若可以分配给下个事务使用
     ```

   - 回滚段中的数据分类。事务提交后并不能马上删除undo log及undo log所在的页。这是因为可能还有其他事务需要通过undo log来得到行记录之前的版本。故事务提交时将undo log放入一个链表中，是否可以最终删除undo log及undo log所在页由purge线程判断

     ```mysql
     1. 未提交的回滚数据(uncommitted undo information)：该数据所关联的事务并未提交，用于实现读一致性，所以该数据不能被其他事务的数据覆盖
     2. 已经提交但未过期的回滚数据(committed undo information)：该数据关联的事务已经提交，但是仍收到undo retention参数的操持时间的影响
     3. 事务已经提交并过期的数据(expired undo information)：事务已经提交，而且数据保存时间已经超过undo retention参数指定的时间，属于已经过期的数据。当回滚段满了之后，会优先覆盖事务已经提交并过期的数据
     ```

5. undo的类型

   - insert undo log是指在insert操作中产生的undo log。因为insert操作的本身，只对事务本身可见，对其他事务不可见（这是事务隔离性的要求），该undo log可以在事务提交后直接删除，不需要进行purge操作
   - update undo log记录的是对delete和update操作产生的undo log。该undo log可能需要提供MVCC机制，因此不能在事务提交时就进行删除，提交时放入undo log链表，等待purce线程进行最后的删除

6. undo log的生命周期

   ![MySQL undo生命周期](../../../TyporaImage/MySQL%20undo%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

   对于InnoDB引擎来说，每个行记录除了记录本身的数据之外，还有隐藏的列

   - DB_ROW_ID：如果没有为表显式的定义主键，并且表中也没有定义唯一索引，那么InnoDB会自动为表添加一个row_id的隐藏列作为主键
   - DB_TRX_ID：每个事务都会分配一个事务ID，当对某条记录发生变更时，就会将这个事务的事务ID写入trx_id中
   - DB_ROLL_PTR：回滚指针，本质上就是指向undo log的指针

   对于更新主键的操作，会把原来的数据deletemark（记录行的隐藏字段）标识打开，这时并没有真正的删除数据，真正的删除会交给清理线程去判断，然后在后面插入一条新的数据，新的数据也会产生undo log，并且undo log的序号会递增。每次对数据的变更都会产生一个undo log，当一条记录被变更多次时，那么就会产生多条undo log，undo log记录的时变更前的日志，并且每个undo log的序号是递增的，那么当要回滚的时候，按照序号依次向前推，就可以找到原始数据了

   针对于insert undo log的删除，因为insert操作的记录，只对事务本身可见，对其他事务不可见，故undo log可以在事务提交后直接删除，不需要进行purge操作；针对于update undo log的删除，因为可能需要提供MVCC机制，因此不能再事务提交时进行删除，提交时放入undo log链表，等待purge线程进行最后的删除

## 四、MySQL事务日志小结

![MySQL事务日志大概流程](../../../TyporaImage/MySQL%E4%BA%8B%E5%8A%A1%E6%97%A5%E5%BF%97%E5%A4%A7%E6%A6%82%E6%B5%81%E7%A8%8B.png)

- undo log是逻辑日志，对事务回滚时，只是将数据库逻辑地恢复到原来的样子
- redo log是物理日志，记录的是数据页的物理变化，undo log不是redo log的逆过程

# 十五、锁

## 一、锁的概述

- 事务的隔离性由锁来实现
- 在数据库中，除传统的计算资源（如CPU、RAM、I/O等）的争用以外，数据也是一种供许多用户共享的资源。为保证数据的一致性，需要对并发操作进行控制，因此产生了锁。同时锁机制也为实现MySQL的各个隔离级别提供了保证。锁冲突也是影响数据库并发访问性能的一个重要因素。所以锁对数据库而言显得尤其重要，也更加复杂

## 二、并发事务访问相同记录

### 一、读-读情况

- 读-读情况，即并发事务相继读取相同的记录。读取操作本身不会对记录有任何影响，并不会引起什么问题，所以允许这种情况的发生

### 二、写-写情况

1. 写-写情况，即并发事务相继对相同的记录做出改动
2. 在这种情况下会发生脏写的问题，任何一种隔离级别都不允许这种问题的发生。所以在多个未提交事务相继对一条记录做改动时，需要让它们排队执行 ，这个排队的过程其实是通过锁来实现的
3. 锁结构中的两个重要属性
   - trx信息：代表这个锁结构是哪个事务生成的
   - is_waiting：代表当前事务是否在等待
4. 当一个事务想对某条录做改动时，首先会看看内存中有没有与这条记录关联的锁结构，当没有的时候就会在内存中生成一个锁结构与之关联
5. 写-写情况下事务与锁的关系
   - 假设有两个事务，T1和T2要对某条记录做改动
   - 当事务T1改动了这条记录后，就生成了一个锁结构与该记录关联，因为之前没有别的事务为这条记录加锁，所以is_waiting属性就是false，把这个场景称之为获取锁成功，或者加锁成功，然后就可以继续执行操作了
   - 在事务T1提交之前，另一个事务T2也想对该记录做改动，那么先看看有没有锁结构与这条记录关联，发现有一个锁结构与之关联后，然后也生成一个锁结构与这条记录关联，不过锁结构的is_waiting属性值为true，表示当前事务需要等待，把这个场景就称之为获取锁失败或者加锁失败
   - 在事务T1提交之后，机会把该事务生成的锁结构释放掉，然后看看还有没有别的事物在等待获取锁，发现了事物T2还在等待获取锁，所以把事物T2对应的锁结构的is_waiting属性设置为false，然后把该事务对应线程唤醒，让它继续执行，此时事务T2就获取到了锁
6. 写-写情况下事务与锁的关系总结
   - 不加锁：意思就是不需要在内存中生成对应的锁结构，可以直接执行操作
   - 获取锁成功，或者加锁成功：意思就是在内存中生成了对应的锁结构，而且锁结构的is_waiting属性为false ，也就是事务可以继续执行操作
   - 获取锁失败，或者加锁失败，或者没有获取到锁意思就是在内存中生成了对应的锁结构 ，不过锁结构的is_waiting属性为true，也就是事务需要等待，不可以继续执行操作

### 三、读-写或写-读情况

- 读-写或写-读，即一个事务进行读取操作，另一个进行改动操作。这种情况下可能发生脏读、不可重复读、幻读的问题

### 四、并发问题解决方案

1. 读操作利用多版本并发控制（MVCC），写操作进行加锁
   - 普通的SELECT语句在READ COMMITTED和REPEATABLE READ隔离级别下会使用到MVCC读取记录
   - 在READ COMMITTED隔离级别下，一个事务在执行过程中每次执行SELECT操作时都会生成一个ReadView，ReadView的存在本身就保证了事务不可以读取到未提交的事务所做的更改，也就是避免了脏读现象
   - 在 REPEATABLE READ隔离级别下，一个事务在执行过程中只有 第一次执行SELECT操作才会生成一个ReadView，之后的SELECT操作都复用这个ReadView，这样也就避免了不可重复读和幻读的问题
2. 读、写操作都采用加锁的方式
3. MVCC和加锁对比
   - 采用MVCC方式的话， 读-写操作彼此并不冲突，性能更高。一般情况下都是用MVCC
   - 采用加锁方式的话， 读-写操作彼此需要排队执行，影响性能