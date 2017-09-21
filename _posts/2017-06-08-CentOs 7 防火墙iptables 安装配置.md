---
layout: post
title:  "CentOs 7 防火墙iptables 安装配置"
date:   2017-06-08 
categories: Linux
tags: Linux
excerpt: CentOS7 中安装防火墙iptables并配置
---
* content
{:toc}


## CentOS 7 中使用iptables的问题

>本来觉得跟阿里云设置一样，结果因为centos 7 与centos 6还是有些差别的，遇到了不少坑





#### 远程连接MySQL

1. 设置账号

   ```mysql
   GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'password'
   ```

2. 防火墙禁用了3306端口，需要打开3306端口开放

   ```shell
   iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
   ```

   或

   ```shell
   vi /etc/sysconfig/iptables
   ```

   增加下面一行

   ```shell
   -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 3306-j ACCEPT
   ```

   这是问题就来了！

   CentOs 7 没有iptables！！没有iptables！！没有iptables！！

   默认的防火墙是firewalle，这里改成iptables来操作

3. 安装iptable iptable-service

   ```shell
   service iptables status
   yum install -y iptables
   yum update iptables
   yum install iptables-services
   systemctl mask firewalld  # 记得禁用firewalld！
   systemctl enable iptables
   systemctl enable ip6tables
   systemctl stop firewalld
   systemctl start iptables
   systemctl start ip6tables 
   ```

   Bingo~~



#### 附上iptables设置选项

1. 禁用/停止自带的firewalld服务

   ```shell
   # 停止firewalld服务
   systemctl stop firewalld
   # 禁用firewalld服务
   systemctl mask firewalld
   ```

2. 设置现有规则

   ```shell
   # 查看iptables现有规则
   iptables -L -n
   # 先允许所有,不然有可能会杯具
   iptables -P INPUT ACCEPT
   # 清空所有默认规则
   iptables -F
   # 清空所有自定义规则
   iptables -X
   # 所有计数器归0
   iptables -Z
   # 允许来自于lo接口的数据包(本地访问)
   iptables -A INPUT -i lo -j ACCEPT
   # 开放22端口
   iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   # 开放21端口(FTP)
   iptables -A INPUT -p tcp --dport 21 -j ACCEPT
   # 开放80端口(HTTP)
   iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   # 开放443端口(HTTPS)
   iptables -A INPUT -p tcp --dport 443 -j ACCEPT
   # 允许ping
   iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT
   # 允许接受本机请求之后的返回数据 RELATED,是为FTP设置的
   iptables -A INPUT -m state --state  RELATED,ESTABLISHED -j ACCEPT
   # 其他入站一律丢弃
   iptables -P INPUT DROP
   # 所有出站一律绿灯
   iptables -P OUTPUT ACCEPT
   # 所有转发一律丢弃
   iptables -P FORWARD DROP
   ```

3. 其他规则设定

   ```shell
   # 如果要添加内网ip信任（接受其所有TCP请求）
   iptables -A INPUT -p tcp -s 45.96.174.68 -j ACCEPT
   # 过滤所有非以上规则的请求
   iptables -P INPUT DROP
   # 要封停一个IP，使用下面这条命令：
   iptables -I INPUT -s ***.***.***.*** -j DROP
   # 要解封一个IP，使用下面这条命令:
   iptables -D INPUT -s ***.***.***.*** -j DROP
   ```

4. 保存规则设定

   ```shell
   # 保存上述规则
   service iptables save
   ```

5. 开启iptables服务

   ```shell
   # 注册iptables服务
   # 相当于以前的chkconfig iptables on
   systemctl enable iptables.service
   # 开启服务
   systemctl start iptables.service
   # 查看状态
   systemctl status iptables.service
   ```

6. 完整设置脚本

   ```shell
   #!/bin/sh
   iptables -P INPUT ACCEPT
   iptables -F
   iptables -X
   iptables -Z
   iptables -A INPUT -i lo -j ACCEPT
   iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   iptables -A INPUT -p tcp --dport 21 -j ACCEPT
   iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   iptables -A INPUT -p tcp --dport 443 -j ACCEPT
   iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT
   iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
   iptables -P INPUT DROP
   iptables -P OUTPUT ACCEPT
   iptables -P FORWARD DROP
   service iptables save
   systemctl restart iptables.service
   ```




--2017-09-21更新--

如果是阿里云centos7.3版本，远程连接不上，出现错误（2003-Can't connect to MySQL server(10060)），就到阿里云Ecs实例安全组配置里新建一个配置吧，坑死了...