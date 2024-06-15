---
title: win远程连接Hyper
---
centos如何开通xrdp
在CentOS上开通Xrdp可以通过以下步骤实现：

安装Xrdp和TigerVNC服务器：

sudo yum install -y epel-release
sudo yum install -y xrdp tigervnc-server
启动Xrdp服务并设置开机自启：

sudo systemctl start xrdp
sudo systemctl enable xrdp
配置防火墙以允许Xrdp连接：

sudo firewall-cmd --permanent --add-port=3389/tcp
sudo firewall-cmd --reload
设置SELinux以允许Xrdp：

sudo setsebool -P xrdp_selinux_enabled 1
如果你想使用特定的用户登录，请确保该用户有权限登录：

sudo usermod -aG vncuser <username>
替换 <username> 为你想允许通过Xrdp登录的用户名。

重启Xrdp服务：

sudo systemctl restart xrdp