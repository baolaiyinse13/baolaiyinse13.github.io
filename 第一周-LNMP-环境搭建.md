#                                             第一周-LNMP-环境搭建

## **0x00 环境准备：**

操作系统：       centos7 X64位系统

Nginx版本：     nginx-1.12.2

MySQL版本：   mysql 5.7.27

PHP版本：        php-7.2.19

 

## **0x01 安装centos7操作系统：**

启动镜像，选择第一个选项，回车

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps1.jpg) 

选择，中文---简体中文

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps2.jpg) 

选择系统--安装位置，

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps3.jpg) 

选择，我要配置分区，点击完成

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps4.jpg) 

选择标准分区

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps5.jpg) 

然后单击 创建新的分区，分区提前规划好， /boot 分区 200M，一般 swap 分区为物理内存的1.5-2 倍，当物理机内存多于16G 后，swap 分区给 8-16G 都可以。 /根分区 10G，实际工作中可以创建数据分区，一般把数据和系统分开。

创建boot分区：

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps6.jpg) 

创建swap分区：

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps7.jpg) 

创建/分区：

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps8.jpg) 

分区创建完成后，点击完成按钮，选择接受更改

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps9.jpg) 

关闭 KDUMP功能

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps10.jpg) 

设置网络，选择打开，可以看到获得到IP地址，修改主机名称，点完成。如果为静态IP，需要在配置选项里手动配置IP地址，DNS等信息

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps11.jpg) 

全部配置完成后，点击开始安装

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps12.jpg) 

创建root密码

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps13.jpg) 

安装完成后，点重启，进入到系统

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps14.jpg) 

**l** **优化配置**

查看是否安装SSH

rpm -qa |grep  ssh

如果未安装   进行安装 yum -y  install   openssh-server   vim   wget

配置SSH：vim /etc/ssh/sshd_config      去掉port 22 前面的#号

 

**l** **解决中文乱码问题**

安装语言包：

yum install kde-l10n-Chinese yum reinstall glibc-common

修改为中文：

vi   /etc/locale.conf

LANG="zh_CN.UTF-8"

source /etc/locale.conf

echo $LANG

 

**l** **更换阿里源**

cd /etc/yum.repos.d

mv CentOS-Base.repo CentOS-Base.repo.bak

Wget    -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

Wget -O/etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

yum clean all

yum makecache

更新所有源   yum update   

 

**l** **关闭SELinux**

​      如果您想永久关闭SELinux，输入命令vi /etc/selinux/config编辑SELinux配置文件。回车后，把光标移动到SELINUX=enforcing这一行，按下i键进入编辑模式，修改为SELINUX=disabled， 按下Esc键，然后输入:wq并回车以保存并关闭SELinux配置文件。

 

**l** **关闭防火墙**

systemctl stop firewalld

systemctl disable firewalld

注：不关闭防火墙web界面打不开，解析不了

 

## **0x02 安装nginx：**

运行如下命令进行安装，

yum -y  install nginx

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps15.jpg) 

安装完成后，查看其版本  nginx -v ,显示版本，说明安装成功

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps16.jpg) 

 

## **0x03 安装Mysql数据库：**

将mysql更新到源中

Rpm -Uvh http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps17.jpg) 

使用yum 安装mysql数据库，时间稍长

yum -y install mysql-community-server

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps18.jpg) 

查看版本，mysql -V ，显示版本安装成功

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps19.jpg) 

## **0x04 安装PHP：**

更新YUM 源

Yum install -y 

http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-15.ius.centos7.noarch.rpm

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps20.jpg) 

rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps21.jpg) 

使用yum 安装PHP ，

yum -y install php72w-devel php72w.x86_64 php72w-cli.x86_64 php72w-common.x86_64 php72w-gd.x86_64 php72w-ldap.x86_64 php72w-mbstring.x86_64 php72w-mcrypt.x86_64  php72w-pdo.x86_64   php72w-mysqlnd  php72w-fpm php72w-opcache php72w-pecl-redis php72w-pecl-mongo

 

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps22.jpg) 

查看版本 php -v  显示版本号为安装成功

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps23.jpg) 

 

## **0x05 配置Nginx**

先将nginx配置文件备份一下（更改linux系统的配置文件，都要养成备份的好习惯）

**命令**：cp /etc/nginx/nginx.conf  /etc/nginx/nginx.conf.bak

使用 vim 命令打开 nginx配置文件   命令：vim /etc/nginx/nginx.conf

编辑内容，放到server { 编辑的内容 }

内容：

location / {             

index index.php index.html index.htm;         

}         #配置Nginx通过fastcgi方式处理您的PHP请求   

  

location ~ .php$ {             

root /usr/share/nginx/html;             

fastcgi_pass 	127.0.0.1:9000; #Nginx通过本机的9000端口将PHP请求转发给PHP-FPM进行处理。          

fastcgi_index index.php;

**include fastcgi_params;**  #Nginx调用fastcgi接口处理PHP请求             	     

fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;             }   

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps24.jpg) 

启动 nginx 

systemctl start nginx

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps25.jpg) 

设置开机自动启动 nginx

systemctl enable nginx

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps26.jpg) 

 

用浏览器访问 IP地址 显示以下内容,nginx正常

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps27.jpg) 

## **0x06 配置MySQL数据库**

启动数据库服务

**命令：** systemctl start mysqld

设置开机自动启动

**命令：**systemctl enable mysqld

查看安装时mysql日志文件，找到初始数据库密码

**命令：**grep 'temporary password' /var/log/mysqld.log

红色标注为初始数据库密码

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps28.jpg) 

**配置Mysql安全性**

**命令：**mysql_secure_installation

重置密码

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps29.jpg) 

 

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps30.jpg) 

删除匿名用户帐号

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps31.jpg) 

禁止root帐号选程登陆

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps32.jpg) 

删除test数据库

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps33.jpg) 

加载授权表

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps34.jpg) 

 

## **0x07 配置PHP**

启动 php-fpm 

   **命令：**systemctl start php-fpm

设置开机自动启动

   **命令：**systemctl enable php-fpm

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps35.jpg) 

输入 netstat -tunlp ，显示以下端口开启，说明配置成功

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps36.jpg) 

## **0x08 测试**

#### nginx解析php

将a.php脚本放到  /usr/share/nginx/html/ 目录下，在nginx配置里设置的路径下

a.php 脚本内容: <?php  echo phpinfo(); ?>

在浏览器上输入 IP地址/a.php，显示出phpinfo信息，说明解析正常

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps37.jpg) 

 

#### php连接mysql

将b.php脚本放到/usr/share/nginx/html/ 目录下，

b.php 脚本内容：

<?php

header("Content-type:text/html;charset=utf-8"); 

// 创建连接

$conn=mysqli_connect('localhost','root','1qaz@WSX');//三个参数分别对应服务器 名，账号，密码

// 检测连接

if (!$conn) { 

​    die("连接服务器失败: " . mysql_connect_error());//连接服务器失败退出程序

}

else {

​	echo "Successful connection";

}

mysql_close($conn);//关闭连接

?>

在浏览器中输入IP地址/b.php，数据库连接成功显示：Successful connection

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps38.jpg) 

## **0x09** **nginx加固**

#### nginx.conf 配置文件

**·** **日志格式配置**

(http标签内，在所有的server标签内可以调用)： 

log_format main '$remote_addr - $remote_user [$time_local] "$request" ' '$status $body_bytes_sent "$http_referer" ' '"$http_user_agent" "$http_x_forwarded_for"';

 

**·** **限制HTTP请求方法**

if ($request_method !~ ^(GET|HEAD|POST)$ ) { return 444; }

if ($http_range ~ "\d{9,}") { return 444; }

 

**·** **设置超时时间**

client_body_timeout 10; #设置客户端请求主体读取超时时间 client_header_timeout 10; #设置客户端请求头读取超时时间 keepalive_timeout 5 5; #第一个参数指定客户端连接保持活动的超时时间，第二个参数是可选的，它指定了消息头保持活动的有效时间 send_timeout10; #指定响应客户端的超时时间

 

**·** **屏蔽IP地址（不全面）**

if ( $geoip_country_code !~ ^(CN|US)$ ) { return 403; }

**·** **封掉不正常的user-agent (不全面)**

if ($http_user_agent ~* "java|python|perl|ruby|curl|bash|echo|uname|base64|decode|md5sum|select|concat|httprequest|httpclient|nmap|scan" ) { return 403; } if ($http_user_agent ~* "" ) { return 403; }

 

**·** **强制使用域名访问**

if ( $host !~* 'XXXX.com' ) { return 403; }

 

**·** **url 参数过滤敏感字（可以自行添加）**

if ($query_string ~* "union.*select.*\(") { rewrite ^/(.*)$ $host permanent; } if ($query_string ~* "concat.*\(") { rewrite ^/(.*)$ $host permanent; }

 

**·** **强制要求referer访问**

if ($http_referer = "" ) ｛ return 403; ｝

 

####  **http {}模块**

**·** **禁止目录浏览**

autoindex off;

 

**·** **错误页面重定向**

http { ... 

fastcgi_intercept_errors on; 

error_page 401 /401.html; 

error_page 402 /402.html; 

error_page 403 /403.html; 

error_page 404 /404.html; 

error_page 405 /405.html; 

error_page 500 /500.html; 

... } 

修改内容： 

ErrorDocument 400 /custom400.html 

ErrorDocument 401 /custom401.html 

ErrorDocument 403 /custom403.html 

ErrorDocument 404 /custom404.html 

ErrorDocument 405 /custom405.html 

ErrorDocument 500 /custom500.html 

其中401.html、402.html、403.html、404.html、405.html、500.html 为要指定的错误提示页面。

 

**·** **隐藏版本信息**

server_tokens off;

 

**·** **定义日志路径**

\# access_log /var/log/nginx/access.log main;

![img](file:///C:\Users\shc\AppData\Local\Temp\ksohtml12724\wps39.jpg) 

 

#### server {}模块

**·** **限制IP访问**

location / { 

deny 192.168.1.1; #拒绝IP 

allow 192.168.1.0/24; #允许IP 

allow 10.1.1.0/16; #允许IP 

deny all; #拒绝其他所有IP 

}

 

**·** **限制并发和速度**

limit_zone one $binary_remote_addr 10m; 

server { 

listen 80; 

server_name XXX.XXXX.com; 

index index.html index.htm index.php; 

root /usr/share/nginx/html; #Zone limit; 

location / { limit_conn one 1; 

​                    limit_rate 20k; } 

​                    }

 



 