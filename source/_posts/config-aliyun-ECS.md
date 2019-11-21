---
title: 阿里云ECS服务器java环境搭建
date: 4/23/2018 1:16:23 PM 
tags: 随笔
---

## jdk 配置
- 下载jdk 到指定的目录 /temp
- 解压到指定的目录：tar -zxvf jdk-8u161-linux-x64.tar.gz -C /usr/java
- 执行命令：cd /usr/java/jdk1.8.0_161
- 配置环境变量 ，执行 vim /etc/profile
- 在/etc/profile末尾加上

    export JAVA_HOME=/usr/java/jdk1.8.0_161
    
    export CLASSPATH=$JAVA_HOME/lib/
    
    export PATH=$PATH:$JAVA_HOME/bin
    
    export JAVA_HOME PATH CLASSPATH
    
- 保存配置后，执行命令:source /etc/profile

- 查看是否jdk是否配置成功，执行命令：java -version 如果出现以下，说明配置成功。

   ![jdk8](/assets/blogImg/jdk8.jpg)
 
    
## tomcat安装和运行
- 下载apache-tomcat 到指定的目录 /temp
- 解压到指定的目录：tar -zxvf apache-tomcat-8.5.29.tar.gz -C /usr/tomcat
- 执行命令：cd /usr/tomcat/apache-tomcat-8.5.29
- 修改端口（也可以不操作），cd conf/,然后执行 vim server.xml,修改8080端口为9999，如下图：

 ![tomcat_port](/assets/blogImg/tomcat_port.JPG)

- 启动tomcat 服务，执行命令 cd ../bin--->./startup.sh
- 验证是否启动成功，执行命令：curl localhost:9999,如果正常返回html，说明成功了。
- 配置外网访问，需要在ECS配置入网规则，允许外网访问9999端口通过。操作过程：

 控制台-->实例-->管理-->本实例安全组-->安全组列表-->配置规则-->入方向-->添加安全组规则，如下图:

 ![add_security_group](/assets/blogImg/add_security_group.JPG)

 点击确定，添加安全组成功后，就可以通过外网ip:9999访问tomcat了。
## mysql安装和配置 
- 检查一下原来是否安装过mysql,执行命令：yum list installed |grep mysql,如果结果为空，说明还没有安装，如果已经安装了，就卸载重新安装。
- 下载mysql安装包，执行命令：rpm -ivh http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
- 安装mysql,执行命令：yum install -y mysql-server,如果显示 Complete! 说明安装成功。
- 设置开机自启动服务：systemctl enable mysqld.service
- 查看开机自启动是否生效：systemctl |grep mysqld,如果显示如下内容，表示生效了

    mysqld.service    loaded active running   MySQL Server

- 设置开启服务 systemctl start mysqld.service
- 查看mysql的root用户初始化密码：grep 'temporary password' /var/log/mysqld.log 
- 登录mysql Server : mysql -uroot -pXXXXXX
- mysql 登录成功后，修改密码：mysql>set PASSWORD=PASSWORD('new password');
- 为root登录开启远程登录：mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'xxxxx' WITH GRANT OPTION;
- 命令立即执行生效：mysql>flush privileges;
- 查看mysql服务状态：systemctl status mysqld;
- 启动mysql服务：systemctl start mysqld;
- 停止mysql服务：systemctl stop mysqld;
- 通过本地的mysql客户端，例如navicat 连接ECS服务器上的mysql,需要配置入网访问规则，运行默认端口3306可以通过外网访问，设置参考tomcat配置规则。
