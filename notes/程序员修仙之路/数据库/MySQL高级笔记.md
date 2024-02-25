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
2. 如果表b采用 MyISAM ，data（/var/lib/mysql）\a中会产生3个文件：
   - MySQL5.7中：b.frm为描述表结构文件，字段长度等；MySQL8.0 中 b.xxx.sdi：描述表结构文件，字段长度等
   - b.MYD (MYData)：数据信息文件，存储数据信息(如果采用独立表存储模式)
   - b.MYI (MYIndex)：存放索引信息文件 

