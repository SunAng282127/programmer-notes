# 一、Docker简介

## 一、Docker概述

1. 假定正在开发一个大型的项目，您使用的是一台笔记本电脑而且您的开发环境具有特定的配置。其他开发人员身处的环境配置也各有不同。您正在开发的应用依赖于您当前的配置且还要依赖于某些配置文件。此外，您的企业还拥有标准化的测试和生产环境，且具有自身的配置和一系列支持文件。您希望尽可能多在本地模拟这些环境而不产生重新创建服务器环境的开销，那么可以使用容器，确保应用能够在不同的环境中运行和通过质量检测，在部署过程中不出现令人头疼的版本、配置问题，也无需重新编写代码和进行故障修复

2. Docker之所以发展如此迅速，也是因为它对此给出了一个标准化的解决方案 --> 系统平滑移植，容器虚拟化技术

3. 环境配置相当麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。开发人员利用 Docker 可以消除协作编码时“在我的机器上可正常工作”的问题

   ![](../../../TyporaImage/image-1719407901989.png)

4. Docker就是将程序和程序所需要的环境一起打包部署，达到程序跨平台运行。解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术

## 二、Docker理念

1. Docker是基于Go语言实现的云开源项目

2. Docker的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“一次镜像，处处运行”

   ![](../../../TyporaImage/image-1719408176539.png)

3. Linux容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的。将应用打成镜像，通过镜像成为运行在Docker容器上面的实例，而 Docker容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作

## 三、容器与虚拟机比较

1. 容器发展简史

   ![](../../../TyporaImage/image-1719408325658.png)

2. 传统虚拟机技术

   - 虚拟机（virtual machine）就是带环境安装的一种解决方案

   - 它可以在一种操作系统里面运行另一种操作系统，比如在Windows10系统里面运行Linux系统CentOS7。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变

     ![](../../../TyporaImage/image-1719408407196.png)

3. 虚拟机的缺点

   - 资源占用多
   - 冗余步骤多
   - 启动慢

4. 容器虚拟化技术

   - 由于前面虚拟机存在某些缺点，Linux发展出了另一种虚拟化技术：Linux容器（Linux Containers，缩写为 LXC）

   - Linux容器是与系统其他部分隔离开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性

   - Linux 容器不是模拟一个完整的操作系统而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行

     ![](../../../TyporaImage/image-1719408623846.png)

5.  比较 Docker 和传统虚拟化方式的不同之处：

   ![](../../../TyporaImage/image-1719408651808.png)

   - 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程
   - 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便
   - 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源

## 四、开发/运维（DevOps）新一代牛马

1. 一次构建、随处运行

   - 一次构建、随处运行：传统的应用开发完成后，需要提供一堆安装程序和配置说明文档，安装部署后需根据配置文档进行繁杂的配置才能正常运行。Docker化之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间
   - 更便捷的升级和扩缩容：随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级
   - 更简单的系统运维：应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复
   - 更高效的计算资源利用：Docker是内核级虚拟化，其不像传统的虚拟化技术一样需要额外的Hypervisor支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率

2. Docker应用场景

   ![](../../../TyporaImage/image-1719409069420.png)

# 二、Docker安装

## 一、前提说明

![](../../../TyporaImage/image-1719409881729.png)

1. 目前Docker的网站和github地址是访问不到的

2. 目前，CentOS 仅发行版本中的内核支持 Docker。Docker 运行在CentOS7（64-bit）上。要求系统为64位、Linux系统内核版本为3.8以上，这里选用Centos7.x

3. 查看自己的内核

   ```shell
   [root@CentOS201 sunsh]# cat /etc/redhat-release 
   CentOS Linux release 7.9.2009 (Core)
   [root@CentOS201 sunsh]# uname -r
   3.10.0-1160.71.1.el7.x86_64
   ```

## 二、Docker的基本组成

![](../../../TyporaImage/image-1719410173432.png)

1. image镜像

   - Docker 镜像（Image）就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器

   - 它也相当于是一个root文件系统。比如官方镜像 centos:7 就包含了完整的一套 centos:7 最小系统的 root 文件系统

   - 相当于容器的“源代码”，docker镜像文件类似于Java的类模板，而docker容器实例类似于java中new出来的实例对象

     ![](../../../TyporaImage/image-1719410380688.png)

2. container容器

   - 从面向对象角度分析：Docker 利用容器（Container）独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，容器是用镜像创建的运行实例。就像是Java中的类和实例对象一样，镜像是静态的定义，容器是镜像运行时的实体。容器为镜像提供了一个标准的和隔离的运行环境，它可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的平台
   - 从镜像容器角度分析：可以把容器看做是一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序

3. repository仓库

   - 仓库（Repository）是集中存放镜像文件的场所
   - 类似于Maven仓库，存放各种jar包的地方；github仓库，存放各种git项目的地方；Docker公司提供的官方registry被称为Docker Hub，存放各种镜像模板的地方（已经不能访问）
   - 仓库分为公开仓库（Public）和私有仓库（Private）两种形式。最大的公开仓库是 Docker Hub，存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云 、网易云等

4. 概念分析

   - Docker 本身是一个容器运行载体或称之为管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就是image镜像文件。只有通过这个镜像文件才能生成Docker容器实例，类似Java中new出来一个对象
   - image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。image 文件生成的容器实例，本身也是一个文件，称为镜像文件
   - 容器实例：一个容器运行一种服务，当我们需要的时候，就可以通过docker客户端创建一个对应的运行实例，也就是我们的容器
   - 仓库：就是放一堆镜像的地方，我们可以把镜像发布到仓库中，需要的时候再从仓库中拉下来就可以了

## 三、Docker平台架构图解

- Docker 是一个 C/S 模式的架构，后端是一个松耦合架构，众多模块各司其职

  ![](../../../TyporaImage/image-1719411164462.png)

  ![](../../../TyporaImage/image-1719411175737.png)

## 四、安装步骤

1.  下载关于Docker的依赖环境 

   ```shell
   [root@CentOS201 sunsh]# yum -y install yum-utils device-mapper-persistent-datalvm2
   ```

2.  设置一下下载Docker的镜像源 

   ```java
   [root@CentOS201 sunsh]# yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
   ```

3.  首先，将软件包信息提前在本地缓存一份，用来提高搜索安装软件的速度 

   ```shell
   [root@CentOS201 sunsh]# yum makecache fast
   ```

4.  提高安装速度以后，安装docker相关的（docker-ce社区版，而ee是企业版） 

   ```shell
   [root@CentOS201 sunsh]# yum install docker-ce docker-ce-cli containerd.io
   ```

5.  启动Docker服务 

   ```shell
   [root@CentOS201 sunsh]# systemctl start docker
   ```

6.  设置开机自动启动 

   ```shell
   [root@CentOS201 sunsh]# systemctl enable docker
   ```

7.  测试 

   ```shell
   [root@CentOS201 sunsh]# docker version
   ```

## 五、卸载步骤

```shell
systemctl stop docker 
yum remove docker-ce docker-ce-cli containerd.io
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

## 六、更换阿里镜像源

1. 登录[阿里网站](https://promotion.aliyun.com/ntms/act/kubernetes.html)

2. 获得加速器地址连接：登录阿里云开发者平台 -> 点击控制台 -> 选择容器镜像服务 -> 镜像工具 -> 获取加速器地址 -> 粘贴网站下方自动生成的脚本直接执行即可

   ```shell
   sudo mkdir -p /etc/docker
   sudo tee /etc/docker/daemon.json <<-'EOF'
   {
     "registry-mirrors": ["https://38h0dfa4.mirror.aliyuncs.com"]
   }
   EOF
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

3. 如果出现以下代码，说明运行成功

   ```shell
   [root@CentOS201 sunsh]# docker run hello-world
   Unable to find image 'hello-world:latest' locally
   latest: Pulling from library/hello-world
   1b930d010525: Pull complete 
   Digest: sha256:0e11c388b664df8a27a901dce21eb89f11d8292f7fca1b3e3c4321bf7897bffe
   Status: Downloaded newer image for hello-world:latest
    
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
    
   To generate this message, Docker took the following steps:
    1. The Docker client contacted the Docker daemon.
    2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
       (amd64)
    3. The Docker daemon created a new container from that image which runs the
       executable that produces the output you are currently reading.
    4. The Docker daemon streamed that output to the Docker client, which sent it
       to your terminal.
    
   To try something more ambitious, you can run an Ubuntu container with:
    $ docker run -it ubuntu bash
    
   Share images, automate workflows, and more with a free Docker ID:
    https://hub.docker.com/
    
   For more examples and ideas, visit:
    https://docs.docker.com/get-started/
   
   ```

   ![](../../../TyporaImage/image-1719413743004.png)

## 七、Docker会比VM虚拟机快的原理

1. docker有着比虚拟机更少的抽象层：由于docker不需要Hypervisor（虚拟机）实现硬件资源虚拟化，运行在docker容器上的程序直接使用的都是实际物理机的硬件资源。因此在CPU、内存利用率上docker将会在效率上有明显优势

2. docker利用的是宿主机的内核，而不需要加载操作系统OS内核。当新建一个容器时，docker不需要和虚拟机一样重新加载一个操作系统内核。进而避免引寻、加载操作系统内核返回等比较费时费资源的过程，当新建一个虚拟机时，虚拟机软件需要加载OS，返回新建过程是分钟级别的。而docker由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个docker容器只需要几秒钟

   ![](../../../TyporaImage/image-1719413908653.png)

   ![](../../../TyporaImage/image-1719413919640.png)

# 三、Docker常用命令