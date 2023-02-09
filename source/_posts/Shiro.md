---
title:  Shiro入门教程
---
第一步我们首先需要了解一下shiro框架是java的一种安全框架，具有认证，授权，加密和会话功能，是一个权限安全框架，经常用于登录权限和用户角色权限分配，如下图所示：
![1](/images/Shiro/1.png)
第二步我们可以看一下shiro的完整架构图，主要包括Authentication和Authorization两个用户权限认证，Subject，SecurityManager等一些组件，如下图所示：
![2](/images/Shiro/2.jpg)
第三步下面通过shiro的登录权限认证和角色权限分配来进行shiro框架的入门，首先创建数据库，这里创建一个用户表user，如下图所示：
![3](/images/Shiro/3.jpg)

第四步创建一张角色表t_role，并创建一张用户和角色的关系表t_user_role，以及一张权限控制表t_permission，如下图所示：
![4](/images/Shiro/4.jpg)
第五步创建完数据库之后，开始编写逻辑业务操作的service层，这里通过用户名称去获取对象和权限资源，主要代码如下图所示：
![5](/images/Shiro/5.jpg)
第六步开始编写shiro组件自己定义的Realm，通过pc.fromRealm(getName()).iterator().next()代码获取授权信息，资源的url信息，获取认证信息等，如下图所示：
![6](/images/Shiro/6.jpg)

第七步编写程序的controller层，实现shiro框架控制用户登录的权限和角色的权限控制等， UsernamePasswordToken token = new UsernamePasswordToken（）；存进shiro token 里面实现权限控制，如下图所示：
![7](/images/Shiro/7.jpg)
第八步编写用户登录成功之后的跳转页面，这里只写了一个简单的登录成功四个字的页面，如下图所示：
![8](/images/Shiro/8.jpg)

第九步最后我们需要对shiro.xml文件进行配置，用户登录和授权都需要在位置文件进行配置，通过 /toLogin = authc 和/home = authc, perms[/home]来控制权限访问，如下图所示：
![9](/images/Shiro/9.jpg)

第十步我们配置好shiro.xml文件后，在浏览器中运行项目，可以看到进去登录界面，数据库添加一个用户之后，输入用户名和密码，可以看到跳转到登录成功的界面，这样成功用shiro框架实现了一个基本的用户登录授权，认证，权限控制的入门例子，如下图所示:
![10](/images/Shiro/10.png)
