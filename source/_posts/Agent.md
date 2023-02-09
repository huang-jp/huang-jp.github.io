---
title: 安装Agent
---
把安装Agent的命令行复制进去远程连接的ip地址（需要在远程建立该ip的连接）
然后在远程连接那边转换Window和Linux（即在bash前面输入dos2unix）
需要将远程连接到172.16.1.13这一台机器，将Y:\store\30Cloud\upload\agent的文件拷到本地store文件夹里面，需要执行UpdateAgentSql.sql语句,如果之前是安装失败就要移除（rm -rfv   /tmp/30sCloudAgent/）了再添加
![1](/images/Agent/1.png)
![2](/images/Agent/2.png)
![3](/images/Agent/3.png)
![4](/images/Agent/4.png)