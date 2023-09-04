---
title: Jenkins + Docker + Github 实现自动化部署 Maven 项目
---
一、Jenkins 介绍
看到这篇文章的你，或多或少都已经对 Jenkins 有过一定了解，就算没有也一定已经听过它的相关话题。

在我们学习阶段，常会听到持续集成和持续部署这样的词语，有些小伙伴们已经亲手实践过，还有些没有过，今天就让我们一起对 Jenkins 做一个了解吧。

1、持续集成和持续部署是什么
持续集成：CI 是一种开发实践，其中开发人员一天几次将代码集成到共享存储库中。当有人将新代码推送到共享存储库中时，测试会在非开发人员（测试人员）的计算机上自动运行。这种纯手动的构建测试,效率非常的低,开发人员必须等待测试人员的反馈后才知道结果,如果错了,还要修改 bug , 这个过程一方面需要沟通成本,另外一方面效率是非常低的.

持续部署：我们都知道，项目最终是会部署到服务器上去，在没用 Jenkins 之前，大都是我们或专业的运维将项目进行部署。如果项目非常多或者部署完后出现 bug，需要人手动的一个个部署或者能力强些的大佬，就是用脚本文件部署，但是看起来还是非常麻烦.

2、关于 Jenkins
Jenkins 是一个用 Java 编写的开源自动化工具，带有用于持续集成的插件。

Jenkins 用于持续构建和测试您的软件项目，从而使开发人员更容易将更改集成到项目中，并使用户更容易获得新的构建。它还允许您通过与大量测试和部署技术集成来持续交付软件。

Jenkins 集成了各种开发生命周期过程，包括构建、文档、测试、打包、模拟、部署、静态分析等等。

Jenkins 借助插件实现了持续集成。插件允许集成各种 DevOps 阶段。如果要集成特定工具，则需要安装该工具的插件。例如 Git、Maven、Node 项目等。

3、Jenkins 的工作流程
我画了一张简易的 Jenkins 的工作流程图，希望能带给你一些帮助。
![0](/images/DockerAndJenkins/top1.PNG)
（图片说明：Jenkins 一项配置简单的工作流程图）

流程说明：

开发者在本地开发，然后提交到 Source Respository 中，
触发 GitHub 或者 GitLab 配置的钩子函数程序，继而通知 Jenkins
Jenkins 收到通知，会通过 Git/SVN 插件，重新从项目配置中的代码仓库中拉取最新代码，放置于 Workspace （Jenkins 存放任务的目录）
之后重新触发构建任务，Jenkins 有很多的构建的插件，Java 常用的 Maven 、Gradle，前端的 Node 等
如果有安装发送邮件的插件并且进行了配置，那么可以在项目中进行配置，构建失败或者成功都可以选择是否给开发者发送邮件
构建成功后，Jenkins 会通过一个 SSH 插件，来远程执行 Shell 命令，发布项目，在项目中可以配置多台服务器，也就可以一次性部署到多台服务器上去。
补充：当然很多时候，构建成功后，并不会直接部署到服务器上，而是打包到另外一台服务器上存储（应用服务器）或者存储为软件仓库中的一个新版本。
原因是一方面为了更好的回退版本，出现错误可以及时恢复，因为一个大型项目，它的构建过程时间说不上短；
另外一方面也是为了更好的扩展，如果出现紧急情况，需要横向扩展，可以在备用机器上，直接进行拉取部署即可。
一个简易的自动部署化的过程，大致是如此的。

但其实中间还有不少东西的，例如代码审查和 Jenkins 自动化测试等等，对一门技术了解的越多，不知道的也就越多了。

二、Docker 安装 Jenkins
Jenkins 其实支持各个系统安装，Windows 、Liunx 、Mac 都可以的。选择 Docker 是方便哈，因为我其他的环境都是用 Docker 搭建的~~ 所以我这里介绍的也是 Docker 安装 Jenkins，后续的文章也都是基于此。

我目前的环境：Jenkins 2.346.2、阿里云服务器 centos7、Docker version 20.10.7

2.1、搜索 Jenkins 镜像
 docker search jenkins
![1](/images/DockerAndJenkins/1.png)
deprecated 是弃用的意思，第一条搜索记录就是告诉我们 jenkins 镜像已经弃用，让我们使用 jenkins/jenkins:lts 镜像名进行拉取。

2.2、拉取 Jenkins 镜像
  docker pull jenkins/jenkins:lts
 docker images #查看镜像
复制
既然是学习，就得上手最新的啦，错了再降。
![2](/images/DockerAndJenkins/2.PNG)
2.3、启动 Jenkins 容器
在宿主机创建挂载目录

 mkdir -p /home/jenkins/workspace
复制
启动 Jenkins 容器

 docker run -uroot -d --restart=always -p 9001:8080 \
 -v /home/jenkins/workspace/:/var/jenkins_home/workspace \
 -v /var/run/docker.sock:/var/run/docker.sock \
 --name jenkins jenkins/jenkins:lts
![3](/images/DockerAndJenkins/3.PNG)
2.4、使用 Jenkins
这个时候就可以直接访问了。
![4](/images/DockerAndJenkins/4.PNG)
会看到这样的一个界面，我们就要进入容器，去拿到这个密码。

 docker exec -it -uroot jenkins bash # -uroot 是以管理员身份登入容器
![5](/images/DockerAndJenkins/5.PNG)
然后复制粘贴上去后，会看到这样的一个界面。

![6](/images/DockerAndJenkins/6.PNG)
如果和我一样是个小白的话，直接安装推荐的插件吧，稍微省事点，不然很多插件都需要自己一个个的查。
![7](/images/DockerAndJenkins/7.PNG)
耐心等待它下载完吧

大家根据自己的需求进行操作，后续也是根据自己的想法一直点击下去就好了，反正咱们还在学习，无妨的。
![8](/images/DockerAndJenkins/8.PNG)
主界面
![9](/images/DockerAndJenkins/9.PNG)
一些简单页面，大家点进去都能看的明白，我就不再多嘴了。

着重说一下 Manger Jenkins 界面一些东西
![10](/images/DockerAndJenkins/10.PNG)
2.5、配置 Jenkins 密钥
其实在很多时候你可以把 Jenkins 容器看成一台独立的服务器，因为你运行项目的那些环境，都可以安装在它的内部。

还是先说说配置密钥，配置密钥主要作用就是为了去 Github、Gitee、GitLab 上拉取代码，这点相信大家都是能够理解的吧。

生成密钥：之前我们已经进入 Jenkins 的终端，直接输入下面的命令就好了。

 ssh-keygen -t rsa -C "root"  #输入完一直回车就结束了
 cat /root/.ssh/id_rsa.pub #查看公钥
![11](/images/DockerAndJenkins/11.PNG)
如果没有的话，就先输入 下面命令进入 Jenkins 容器终端

 docker exec -it -uroot jenkins bash # jenkins 是我启动的容器名 换成容器id 也可以的
复制
拿到 Jenkins 公钥后，就放到 Github、Gitee 或者是 GitLab 上去，我放的 Github，如下：
![12](/images/DockerAndJenkins/12.PNG)
![13](/images/DockerAndJenkins/13.PNG)
这样就算是添加完成了。

到这一步，可以进行测试一下，是否已经可以从 Github 上拉取项目
![14](/images/DockerAndJenkins/14.PNG)
三、 Jenkins 插件安装、添加凭据、系统配置、全局工具配置
实际上 Jenkins 的功能基本上都是依靠插件来完成，所以不同项目也会要安装不同的插件。

3.1、插件安装
我演示的是一个 SpringBoot 后端项目的部署，中间也没有穿插复杂的操作，所以装的插件也不多哈。
![15](/images/DockerAndJenkins/15.png)
安装的插件的名称：

Maven Integration ：Maven 项目打包工具
SSH ： SSH 连接工具
Publish Over SSH ：SSH 发布工具
如果要运行前端 Vue 项目，记得下载一个 NodeJS 的插件（我会的前端就只有 Vue 哈）
![16](/images/DockerAndJenkins/16.PNG)
![17](/images/DockerAndJenkins/17.PNG)
等待下载完即可

3.2、添加凭据
凭据其实就是账号密码，你访问 Github、远程服务器都需要账号密码才行，这里的凭据就是相应的账号密码。
![18](/images/DockerAndJenkins/18.PNG)
![19](/images/DockerAndJenkins/19.PNG)
添加 github 的账号密码
![20](/images/DockerAndJenkins/20.PNG)
添加服务器的登录账号和密码
![21](/images/DockerAndJenkins/21.PNG)
补充：这些都是可以填加多个的。

最后就是这样的：
![22](/images/DockerAndJenkins/22.PNG)
稍后在项目中都是需要用到的。

3.3、系统配置
![23](/images/DockerAndJenkins/23.PNG)
找到两个配置：

1、SSH remote hosts

2、SSH Servers
![24](/images/DockerAndJenkins/24.PNG)
![25](/images/DockerAndJenkins/25.PNG)
![26](/images/DockerAndJenkins/26.PNG)
对了记得点击保存哈，不然又得重现填写。

3.4、全局工具配置
我上文有提到很多时候我们可以把 Jenkins 看成一台单独的电脑，尤其是在工具设置的时候。
![27](/images/DockerAndJenkins/27.PNG)
在这里其实就是配置一些项目中需要用到的环境，如 JDK、Maven、NodeJS 等等
![28](/images/DockerAndJenkins/28.PNG)
1、Maven 配置
这里可以用默认的，也可以用宿主机文件系统中的。
![29](/images/DockerAndJenkins/29.PNG)
我这里用的是默认的，因为我 Liunx 服务器上的环境全部都是基于 Docker 搭建的。

选择默认的话，就需要在 Jenkins 中新增一个，然后 Jenkins 在你构建项目的时候，如果是选择默认的话，没有的 Maven 情况下，它会主动给你下载一个 Maven
![30](/images/DockerAndJenkins/30.PNG)
2、JDK 配置
JDK 也是可以选择自动安装和使用宿主机原有的 JDK 两种方案。
![31](/images/DockerAndJenkins/31.PNG)
点击 Please enter your username/password蓝色小字后，会跳转至下面的界面
![32](/images/DockerAndJenkins/32.PNG)
(图片说明：在这里输入 Oracle 官网的账号密码即可)

没有账号需要去注册 Oracle 账号

补充：如果这里选择 Oracle 官网下载 JDK，最高支持 JDK 版本为 9，如果要选择更高的稳定的 JDK 版本，一个是使用宿主机的 JDK，另外就是使用压缩包方式，然后解压等。只要想用，互联网应该还是能够满足的。

关于 Git ，我们直接用 Jenkins 默认的即可，在安装 Jenkins 推荐的插件的时候，其中就有 Git，当然 Git，这里 Jenkins 也允许你使用 宿主机的 Git。

最后记得点击保存

四、Jenkins 部署 Maven 项目
相关要求：

本地需要 Java、Git 环境
需要有一个 Github/Gitee 账号
不管在那里安装的 Jenkins 都要确保它能够访问网络
4.1、本地创建一个 Maven 项目
创建一个 SpringBoot 项目，controller，自己看着写就好了
![33](/images/DockerAndJenkins/33.PNG)
pom.xml

    <parent>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-parent</artifactId>
         <version>2.5.1</version>
         <relativePath/> <!-- lookup parent from repository -->
     </parent>
 
     <properties>
         <maven.compiler.source>8</maven.compiler.source>
         <maven.compiler.target>8</maven.compiler.target>
     </properties>
 
     <dependencies>
         <dependency>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter</artifactId>
             <version>2.5.1</version>
         </dependency>
         <dependency>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter-web</artifactId>
             <version>2.5.1</version>
         </dependency>
     </dependencies>
 
     <!-- 这个插件，可以将应用打包成一个可执行的jar包  -->
     <build>
         <plugins>
             <plugin>
                 <groupId>org.springframework.boot</groupId>
                 <artifactId>spring-boot-maven-plugin</artifactId>
             </plugin>
         </plugins>
     </build>
复制
Dockerfile 文件

 FROM adoptopenjdk/openjdk8
 
 WORKDIR app  #切换到镜像中的指定路径，设置工作目录
 
 COPY target/*.jar app.jar  #会将宿主机的target/*.jar文件复制到 镜像的工作目录 /app/ 下 
 
 CMD ["java", "-jar", "app.jar"]  #执行java -jar 命令
复制
因为不是本文关注的重点，更多详情可能还需朋友们去查询。

4.2、推送到远程仓库 github 上
首先在 Github 上创建一个仓库
然后点击 idea 中下方菜单栏 Terminal 命令行终端中输入
   git init 
   git add .  #这里 . 默认提交所有修改文件
   git commit -m "init"
   git branch -m main # 因为gitgub 仓库默认主分支为main ，而我们本地初始化时 主分支为 master 
   git remote add  远程仓库地址 # 添加远程仓库
   git push origin main  # push 上去
复制
然后刷新 github 界面，就可以看到已经 push 成功了
4.3、Jenkins 项目配置
新建任务
![34](/images/DockerAndJenkins/34.PNG)
选择构建一个 Maven 项目
![35](/images/DockerAndJenkins/35.PNG)
项目配置
![36](/images/DockerAndJenkins/36.PNG)
源码管理
![37](/images/DockerAndJenkins/37.PNG)
![38](/images/DockerAndJenkins/38.PNG)
构建环境没啥说的
![39](/images/DockerAndJenkins/39.PNG)
Build 那不用改啥，用默认的就可以。

然后来到 Post Steps
![40](/images/DockerAndJenkins/40.PNG)
选择这个 send files or execute commands over SSH 就是通过SSH发送文件或执行命令

我们要将构建好的 jar/war 发送至相应的服务器，然后执行相关的命令，进行部署。
![41](/images/DockerAndJenkins/41.PNG)
![42](/images/DockerAndJenkins/42.PNG)
（图片说明：那个映射的端口应为 8080，我写漏了）

#/bin/bash
# 注意 其实在这里输入的命令，就是在服务器上的命令，我们所处于的位置就是当前登录用户的根目录下 
echo ">>>>>>>>>>>>>cd 到宿主机映射 Jenkins 的项目路径下>>>>>>>>>>>>>"

cd /home/jenkins/workspace/hello-springboot

echo ">>>>>>>>>>>>>停止容器>>>>>>>>>>>>>"

docker stop hellospringboot

echo ">>>>>>>>>>>>>删除容器>>>>>>>?>>>2>22"

docker rm hellospringboot

echo ">>>>>>>>>>>>>删除镜像>>>>>>>>>>>> >"

docker rmi nzc/hellospringboot:1.0

echo ">>>>>>>>>>>>>制作镜像>>>>>>>>>>>>>"

docker build -f Dockerfile -t nzc/hellospringboot:1.0  .

echo ">>>>>>>>>>>>>启动容器>>>>>>>>>>>>>"

docker run -p 8080:8080 --name hellospringboot -d nzc/hellospringboot:1.0

echo ">>>>>>>>>>>>自动部署结束>>>>>>>>>>>>>"
复制
记得点击保存

4.4、部署和测试
然后点击立即构建就好了，但因为是第一次构建，要从 github 拉取代码，下载 jdk、maven 等，还有相应 jar 包等，所以时间会相对久一些。
![43](/images/DockerAndJenkins/43.PNG)
点击这个，可以看到控制台输出
![44](/images/DockerAndJenkins/44.PNG)
控制台输出，日志比较多，就挑了一点
![45](/images/DockerAndJenkins/45.PNG)
![46](/images/DockerAndJenkins/46.PNG)
末尾是Finished: SUCCESS 就证明是构建成功啦。

我们来看看 Jenkins 的工作空间
![47](/images/DockerAndJenkins/47.PNG)
这里已经是存在项目啦。

我们再去看看 Docker 镜像有没有构建成功
![48](/images/DockerAndJenkins/48.PNG)
再去看一眼 有没有在运行
![49](/images/DockerAndJenkins/49.PNG)
最后就是看看能不能访问到了
![50](/images/DockerAndJenkins/50.PNG)
已经是可以访问到了。

当然现在的话，还没有做到自动化部署，就是我提交完，jenkins 就能自己知道，然后进行构建，我们现在要做的就是把手动构建修改成 github 更新或合并就构建。

五、GitHub 提交代码时触发 Jenkins 自动构建
其实主要就三步，因为前面我们已经搭建好了，所以就只要修改一下即可。

5.1、GitHub 上配置 Jenkins 的 webhook
![51](/images/DockerAndJenkins/51.PNG)
![52](/images/DockerAndJenkins/52.PNG)
像我 jenkins 是部署在服务器上的 我的 地址就是服务器 IP:port/github-webhook/

5.2、GitHub 上创建一个 access token
Jenkins 做一些需要权限的操作的时候就用这个 access token 去鉴权
![53](/images/DockerAndJenkins/53.PNG)
![54](/images/DockerAndJenkins/54.PNG)
就是命名，然后勾选你需要的权限就可以了

最后完成的时候，记得复制
![55](/images/DockerAndJenkins/55.PNG)
5.3、修改 Jenkins 配置
首先先要修改一下系统配置 Configure System
![56](/images/DockerAndJenkins/56.PNG)
点添加的时候，会弹出一个框
![57](/images/DockerAndJenkins/57.PNG)
![58](/images/DockerAndJenkins/58.PNG)
描述自己写就行~~
![59](/images/DockerAndJenkins/59.PNG)
第一步完成，记得点击保存，接下来去修改一下项目配置

找到项目，点击配置

把构建触发器中的 GitHub hook trigger for GITScm polling 勾上
![60](/images/DockerAndJenkins/60.PNG)
![61](/images/DockerAndJenkins/61.PNG)
记得点击保存。

然后就可以进行测试啦。

5.4、Push 测试
![62](/images/DockerAndJenkins/62.PNG)
这是我本地还没更新的代码，现在 push 上去哈。
![63](/images/DockerAndJenkins/63.PNG)
push 成功后，刷新一下就可以看到构建任务正在执行了。
![64](/images/DockerAndJenkins/64.PNG)
在构建日志中也可以看到 最新 的提交记录
![65](/images/DockerAndJenkins/65.PNG)
![66](/images/DockerAndJenkins/66.PNG)
六、总结
文章的脉络大致：

首先是介绍了 Jenkins，以及 Jenkins 的简单工作流程；

而后是教大家如何用 Docker 安装 Jenkins ；

然后再是对 Jenkins 进行一些插件安装和配置；

再以 Jenkins 部署 Maven 项目为案例，讲解如何使用 Jenkins；

最后是对上一步操作的改早，Jenkins + Github + Docker 实现自动化部署。






https://cloud.tencent.com/developer/news/918888