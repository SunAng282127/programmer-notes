# 一、Nginx安装部署

## 一、安装VM虚拟机

## 二、安装并设置CentOS7系统

1. 安装CentOS7系统

2. 修改ip为静态ip

   ```shell
   #配置静态ip首先需要打开网卡配置文件
   vi /etc/sysconfig/network-scripts/ifcfg-ens33
   
   TYPE=Ethernet
   PROXY_METHOD=none
   BROWSER_ONLY=no
   BOOTPROTO=static
   DEFROUTE=yes
   IPV4_FAILURE_FATAL=no
   IPV6INIT=yes
   IPV6_AUTOCONF=yes
   IPV6_DEFROUTE=yes
   IPV6_FAILURE_FATAL=no
   IPV6_ADDR_GEN_MODE=stable-privacy
   NAME=ens33
   UUID=10ac735e-0b8f-4b19-9747-ff28b58a1547
   DEVICE=ens33
   ONBOOT=yes
   IPADDR=192.168.35.201
   NETMASK=255.255.255.0
   GATEWAY=192.168.35.1
   DNS1=8.8.8.8
   ```

3. 一些公网DNS服务器

   ```shell
   #阿里
   223.5.5.5
   223.6.6.6
   
   #腾讯
   119.29.29.29
   182.254.118.118
   
   #百度
   180.76.76.76
   
   #114DNS
   114.114.114.114
   114.114.115.115
   
   #谷歌
   8.8.8.8
   8.8.4.4
   ```

## 三、Nginx的安装

1. Nginx版本区别

   - Nginx开源版：http://nginx.org/ 
   - Nginx plus 商业版：https://www.nginx.com 
   - openresty：http://openresty.org/cn/ 
   - Tengine：http://tengine.taobao.org/

2. 编译安装

   - 查看[Nginx官](http://nginx.org/en/download.html)网的最新稳定版本

   ```shell
   #官网直接下载也可，然后上传到CentOS中
   yum install wget
   wget https://nginx.org/download/nginx-1.26.1.tar.gz
   
   tar -zxvf nginx-1.26.1.tar.gz
   
   cd nginx-1.26.1/
   
   ./configure --prefix=/usr/local/nginx
   make
   make install
   ```

3. 如果出现警告或报错

   ```shell
   #提示
   checking for OS
   + Linux 3.10.0-693.el7.x86_64 x86_64
   checking for C compiler ... not found
   ./configure: error: C compiler cc is not found
   
   #安装gcc
   yum install -y gcc
   
   #提示
   ./configure: error: the HTTP rewrite module requires the PCRE library.
   You can either disable the module by using --without-http_rewrite_module
   option, or install the PCRE library into the system, or build the PCRE library
   statically from the source with nginx by using --with-pcre=<path> option.
   
   #安装perl库
   yum install -y pcre pcre-devel
   
   #提示
   ./configure: error: the HTTP gzip module requires the zlib library.
   You can either disable the module by using --without-http_gzip_module
   option, or install the zlib library into the system, or build the zlib library
   statically from the source with nginx by using --with-zlib=<path> option.
   
   #安装zlib库
   yum install -y zlib zlib-devel
   make
   make install
   ```

4. 启动Nginx

   ```shell
   #进入安装好的目录 /usr/local/nginx/sbin
   ./nginx 启动
   ./nginx -s stop 快速停止
   ./nginx -s quit 优雅关闭，在退出前完成已经接受的连接请求
   ./nginx -s reload 重新加载配置
   ```

5. 关于防火墙

   ```shell
   关闭防火墙
   systemctl stop firewalld.service
   禁止防火墙开机启动
   systemctl disable firewalld.service
   放行端口和http
   firewall-cmd --add-service=http --permanent 
   firewall-cmd --zone=public --add-port=80/tcp --permanent
   重启防火墙
   firewall-cmd --reload
   ```

6. 安装成系统服务

   ```shell
   #创建服务脚本
   vi /usr/lib/systemd/system/nginx.service
   
   #服务脚本内容
   [Unit]
   Description=nginx - web server
   After=network.target remote-fs.target nss-lookup.target
   [Service]
   Type=forking
   PIDFile=/usr/local/nginx/logs/nginx.pid
   ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
   ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
   ExecReload=/usr/local/nginx/sbin/nginx -s reload
   ExecStop=/usr/local/nginx/sbin/nginx -s stop
   ExecQuit=/usr/local/nginx/sbin/nginx -s quit
   PrivateTmp=true
   [Install]
   WantedBy=multi-user.target
   
   #重新加载系统服务
   systemctl daemon-reload
   
   #启动服务
   systemctl start nginx.service
   
   #开机启动
   systemctl enable nginx.service
   ```

   