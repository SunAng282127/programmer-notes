# 一、版本控制

版本控制是一种在开发的过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术

- 实现跨区域多人协同开发
- 追踪和记载一个或多个文件的历史记录
- 组织和保护你的源代码和文档
- 统计工作量
- 并行开发、提高开发效率
- 跟踪记录整个软件的开发过程
- 减轻开发人员的负担，节省时间，同时降低认为错误
- 简单的说就是用于管理多人协同开发项目的技术

1. 本地版本控制：记录文件每次的更新，可以对每个版本做一个快照，或是记录补丁文件，适合个人使用
2. 集中版本控制：所有的版本数据都保存在服务器上，协同开发者从服务器上同步更新或上传自己的修改。如SVN
3. 分布式版本控制：所有版本信息仓库全部同步到本地的每个用户，这样就可以在本地查看所有的版本历史，可以离线在本地提交，只需联网时push到相应的服务器或其他用户那里。由于每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没问题就可以恢复所有的数据，但这增加了本地存储空间的占用。如Git

# 二、Git环境配置

1、下载地址：

- [官网下载地址](https://git-scm.com/downloads)
- [淘宝镜像下载](http://npm.taobao.org/mirrors)

2、卸载：

- 电脑系统应用直接卸载
- 清理Git系统环境变量

3、安装：

傻瓜式安装即可，可以在安装过程中选择notepad++编译器，不用Vim编译器，并没有多大的区别

# 三、基本的Linux命令

- cd：改变目录
- cd..：回退到上一个目录，直接cd进入默认目录
- pwd：显示当前所在的目录路径
- ls(ll)：都是列出当前目录中的所有文件，只不过ll(两个ll)列出的内容更为详细
- touch：新建一个文件
- rm：删除一个文件
- mkdir：新建一个目录，就是新建一个文件夹
- rm -r：删除一个文件夹
- mv：移动文件，mv index.html src， index.html是要移动的文件，src是目标文件夹，不标明目录名就是默认在同一文件夹下
- reset：重新初始化终端/清屏
- clear：清屏
- history：查看命令历史
- help：帮助
- exit：退出
- #：表示注释

# 四、Git常用命令

- git config -l：查看配置
- git config --system --list：查看系统配置
- git config --global --list：查看全局配置
- git config --global user.name ""：设置用户名
- git config --global user.email ""：设置邮箱
- git int：初始化仓库
- git clone url：拉取远程仓库
- git status 文件名称.后缀名：查看文件状态
- git commit -m "说明"：提交到本地仓库
- git push url：提交到远程仓库
- git reflog：查看git仓库的版本日志
- [Git命令大全](https://gitee.com/all-about-git)

# 五、Git基础理论

Git本地有三个工作区域：工作目录(Working Directory)、暂存区(Stage/Index)、资源库(Repository或Git Directory)以及远程仓库(Remote Directory)

 ![img](D:\Program Files\TyporaData\TyporaImage\v2-9829c123f4ce2c78ad780370457e0fc6_720w.webp) 

大概的过程就是首先用git add将代码放到暂存区中，这个时候只是暂存，然后用git commit将代码发送到本地仓库中，最后用git push代码放置到github远程库中。git管理的文件有三种状态：已修改、已暂存、已提交

 <img src="D:\Program Files\TyporaData\TyporaImage\v2-51e676ef74e2d4cf8e63215811944766_720w.webp" alt="img" style="zoom:80%;" /> 

#  六、Git项目创建以及克隆

1. git init：初始化本地仓库

2. git clone：从远程仓库拉取

3. git add .：提交全部文件到暂存区，“.”可以换成具体的文件名称以及后缀名

4. git commit -m "说明"：提交到本地仓库

5. git push url：提交到远程仓库

6. 忽略文件，在主目录下建立“.gitignore”文件，规则如下

   ```
   .gitignore文件中的目录名称或文件名称都是相对于.gitignore来说的
   忽略文件中的空行或以 # 开始的行将会被忽略
   可以使用Linux通配符。例如 * 代表任意多个字符；? 代表一个字符；[] 代表可选字符范围；{} 代表可选的字符串
   如果名称最前面有一个 !，表示例外规则，将不被忽略
   如果名称最前面是一个路径分隔符（/）,表示要忽略的文件在此目录下，而子目录中的文件不忽略
   如果名称最后面是一个路径分隔符（/）,表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）
   ```

   ```
   # 为注释
   *.txt		#忽略所有.txt结尾的文件
   !lib.txtx	#lib.txt不忽略
   /temp		#仅忽略项目根目录下的TODD文件，不包括其他目录temp
   build/		#忽略build/目录下的所有文件
   doc/*.txtx	#忽略doc/notes.txt 但是不包括doc/server/arch.txt
   ```

7. Gitee ssh免登录操作（可以存储在自创的文件夹中）

   ```
   ssh-keygen -t rsa		#rsa是常用的加密算法
   将生成的公钥填入Gitee公钥中
   ```

8. git pull url：拉取代码

#  七、同项目同时同步到Gitee和GitHub

1. 在Github或者Gitee上建立一个仓库，一个建立，另一个导入即可
2. 使用git clone 命令拉取代码
3. 点击.git文件，在里面找到config文件打开filemode=true，即忽略文件夹权限
4. 使用git remote add url 分别添加Gitee和GitHub的远程仓库地址
5. 可以在config文件查看添加的远程仓库地址
6. push的时候使用git push url分别push到gitee和github上，或使用git push --all
7. 如果Gitee和GitHub同时关联，第四步可省略，第六步只需要git push则会同时更新Gitee和GitHub代码
8. 如果IDEA直接打开以上配置好的项目，则会直接关联到远程仓库。如果没有则需在IDEA上方GIT的Manage Remote中去添加远程仓库的地址





