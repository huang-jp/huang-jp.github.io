---
title: Jenkins + Docker + Gitee 实现部署 Maven 项目(1)
---
环境：移动云申请的免费一个使用
坑：移动云 的80、443 … 要备案才能公网访问
目标： docker-compose 一次性部署基础组件，jenkins 一键发布
下篇通过jenkins部署前端vue项目


一、安装docker
1.1、删除之前安装的docker(若之前未安装过，此步骤省略…)
进入centos根目录执行以下命令（\ 是linux系统种命令换行符，如果命令过长，可以用\来换行）
```
  yum remove docker \
  docker-client \
  docker-client-latest \
  docker- common \
  docker-latest \
  docker-latest-logrotate \
  docker-logrotate \
  docker-sqlinux \
  docker-engine-selinux \
  docker-engine \
  docker-ce
```
1.2、虚拟机联网，安装yum工具 （可省略）
在新主机首次安装 Docker Engine-Community之前，需要设置Docker仓库，之后，您可以从仓库安装和更新 Docker。
设置仓库，需要安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。
Device Mapper 是 Linux2.6 内核中支持逻辑卷管理的通用设备映射机制，它为实现用于存储资源管理的块设备驱动提供了一个高度模块化的内核架构。
LVM（Logical Volume Manager）逻辑卷管理。
它是对磁盘分区进行管理的一种机制，建立在硬盘和分区之上的一个逻辑层，用来提高磁盘管理的灵活性。通过LVM可将若干个磁盘分区连接为一个整块的卷组(Volume Group)，形成一个存储池。可以在卷组上随意创建逻辑卷(Logical Volumes)，并进一步在逻辑卷上创建文件系统，与直接使用物理存储在管理上相比，提供了更好灵活性。
device-mapper-persistent-data 和 lvm2 两者都是Device Mapper所需要的。
在这之前还可以更新下 yum linux 版本，毕竟docker 、及其他组件都是比较新的
```
yum update
```
执行以下命令
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```
1.3、设置docker镜像源
执行一下命令
```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
可以先更新下yum 软件包最新
```
 yum makecache fast
```
执行以下命令加粗样式

1.4、安装docker
默认安装最新版
```
yum -y install docker-ce
```
1.5、启动docker前准备
(docker应用需要用到各种端口，逐一设置比较麻烦，建议直接关闭防火墙) 重要的事请说三遍：启动docker前，一定要关闭防火墙、启动docker前，一定要关闭防火墙、启动docker前，一定要关闭防火墙（关闭前可通过查看查看防火墙状态来检验是否关闭）
```
#关闭
systemctl stop firewalld
#禁止开机启动防火墙
systemctl disable firewalld
```

1.6、启动docker
```
systemctl start docker
```

设置开机启动docker
```
systemctl enable docker.service
```

查看是否启动成功有多种方法

```
 #查看状态：
 systemctl status docker
 
 #查看版本
 
 docker -v
 ```
 ![1](/images/DockerAndJitee/1.png)
1.7、设置国内镜像
docker官方镜像仓库网速较差，设置国内镜像，首选阿里云参考阿里云的镜像加速文档: https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors
我们这里选择centOS(如下图，执行图片中命令即可)
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://ds56c2e4.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
1.8.安装docker-compose
选择自己想要安装的版本修改以下语句版本号
```
curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```
大概率失败
可以看这里去取
程序猿一枚
https://blog.csdn.net/zhaozhiqiang1981/article/details/129227141
授权
```
chmod +x /usr/local/bin/docker-compose
```
检查版本2.2.2
```
docker-compose version
```
二、通过docker-compose 安装redis、mysql、jenkins、nginx
备注：一个一个按太麻烦了；后续还要部署前端vue项目 这里一并把nginx部署
mysql 下面文件都要赋予权限

2.1在根目录下创建 相应文件夹 如下图
使用sudo mkdir /docker/**创建更目录文件夹 
chmod -R 777 docker/** 授权文件夹权限不然会失败
 ![2](/images/DockerAndJitee/2.png)
 2.2 给docker分配文件夹权限
重点注意: 一定要确保目录 /docker 及其所有子目录 具有写权限 如果后续出现权限异常问题 重新执行一遍分配权限,每个服务的文件夹都弄下
```
chmod -R 777 /docker
```
2.3 jenkins、redis 、mysql 、nginx 配置文件自己找吧
附上1: nginx nginx.conf
之所以监听端口配置81，主要是移动云 安全规则如此，也是摧残人啊
```
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    # 限制body大小
    client_max_body_size 100m;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    upstream server {
        ip_hash;
        # gateway 地址
        server 127.0.0.1:8080;
        # server 127.0.0.1:8081;
    }

    server {
        listen       81;
        server_name  localhost;
        # localhost修改为服务器地址

        # https配置参考 start
        #listen       443 ssl;

        # 证书直接存放 /docker/nginx/cert/ 目录下即可 更改证书名称即可 无需更改证书路径
        #ssl on;
        #ssl_certificate      /etc/nginx/cert/xxx.local.crt; # /etc/nginx/cert/ 为docker映射路径 不允许更改
        #ssl_certificate_key  /etc/nginx/cert/xxx.local.key; # /etc/nginx/cert/ 为docker映射路径 不允许更改
        #ssl_session_timeout 5m;
        #ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        #ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        #ssl_prefer_server_ciphers on;
        # https配置参考 end

        # 演示环境配置 拦截除 GET POST 之外的所有请求
        # if ($request_method !~* GET|POST) {
        #     rewrite  ^/(.*)$  /403;
        # }

        # location = /403 {
        #     default_type application/json;
        #     return 200 '{"msg":"演示模式，不允许操作","code":500}';
        # }

        # 限制外网访问内网 actuator 相关路径
        location ~ ^(/[^/]*)?/actuator(/.*)?$ {
            return 403;
        }

        location / {
            root   /usr/share/nginx/html;
            try_files $uri $uri/ /index.html;
            index  index.html index.htm;
        }

        location /prod-api/ {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http:服务器地址/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```
附件2：redis.conf
```
#redis 密码
requirepass 123456

#key 监听器配置
#notify-keyspace-events Ex

#配置持久化文件存储路径
dir /redis/data
#配置rdb
#15分钟内有至少1个key被更改则进行快照
save 900 1
#5分钟内有至少10个key被更改则进行快照
save 300 10
#1分钟内有至少10000个key被更改则进行快照
save 60 10000
#开启压缩
rdbcompression yes
#rdb文件名 用默认的即可
dbfilename dump.rdb

#开启aof
appendonly yes
#文件名
appendfilename "appendonly.aof"
#持久化策略,no:不同步,everysec:每秒一次,always:总是同步,速度比较慢
#appendfsync always
appendfsync everysec
#appendfsync no
```
2.4 配置 docker-compose.yml
下载好的 docker-compose 一般是放在 /usr/local/bin 下
在同级目录下 创建 docker-compose.yml 内容如下

注意：network_mode: “host” 外网直接访问服务端口，如此 映射的端口 实际上无效的，直接访问容器配置文件的 端口，比如 -p 5767:80 实际上访问地址ip:80 而不是ip:5756 详细查询compose网络模式

(版本可以自选)
```
version: '3'

services:
  mysql:
    image: mysql:8.0.31
    restart: always
    container_name: mysql
    environment:
      # 时区上海
      TZ: Asia/Shanghai
      # root 密码
      MYSQL_ROOT_PASSWORD: 123456
      # 初始化数据库
      MYSQL_DATABASE: ry-vue-zhnc
    ports:
      - "3306:3306"
    volumes:
      # 数据挂载
      - /docker/mysql/data/:/var/lib/mysql/
      # 配置挂载
      - /docker/mysql/conf/:/etc/mysql/conf.d/
    command:
      # 将mysql8.0默认密码策略 修改为 原先 策略 (mysql8.0对其默认策略做了更改 会导致密码无法匹配)
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_general_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
    privileged: true
    

  jenkins:
    image: jenkins/jenkins:2.419
    container_name: jenkins
    restart: always
    user: root
    ports:
    - "8081:8080"
    - "50000:50000"
    volumes:
    - /docker/jenkins_home:/var/jenkins_home
    - /etc/localtime:/etc/localtime:ro
    - /usr/bin/docker:/usr/bin/docker 
    - /var/run/docker.sock:/var/run/docker.sock

  redis:
    image: redis:6.2.6
    container_name: redis
    restart: always
    ports:
      - "6379:6379"
    environment:
      # 时区上海
      TZ: Asia/Shanghai
    volumes:
      # 配置文件
      - /docker/redis/conf:/redis/config
      # 数据文件
      - /docker/redis/data/:/redis/data/
    command: "redis-server /redis/config/redis.conf"
    privileged: true



  nginx:
    image: nginx:latest
    container_name: nginx
	  restart: always
    environment:
      # 时区上海
      TZ: Asia/Shanghai
    ports:
      - "81:81"
    volumes:
      # 证书映射
      - /docker/nginx/cert:/etc/nginx/cert
      # 配置文件映射
      - /docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf
      # 页面目录
      - /docker/nginx/html:/usr/share/nginx/html
      # 日志目录
      - /docker/nginx/log:/var/log/nginx
    privileged: true 
  ```
  ```
#nginx不会自动创建需要手动复制文件
#先用docker创建nginx，复制对应文件
mkdir -p nginx/conf nginx/conf.d && cd nginx
docker run --name nginx-demo -d nginx
#复制文件
docker cp nginx-demo:/etc/nginx/nginx.conf ./conf/nginx.conf
docker cp nginx-demo:/etc/nginx/conf.d/default.conf ./conf.d/default.conf
docker cp nginx-demo:/usr/share/nginx/html .
#删除容器
docker stop nginx-demo && docker rm nginx-demo
```
2.5 在 /usr/local/bin 目录执行
```
 docker-compose up -d
```
成功如下图：
 ![3](/images/DockerAndJitee/3.png)
 注意1：镜像版本尽可能指定，本人在测试安装jenkins 每次拉取最新的，但是 都是2.381的版本 这个版本在安装插件的时候 只有两三个成功，现在之所以选择2.4X 也是无意中拉取后 后续操作没有问题*

注意2：docker的运行包括以下三部分：
开始看别帖子 没有注意到这里要说明什么，直到后面遇到

Jenkins执行shell脚本报错：docker: command not found

1.docker-cli -------docker客户端
2.docker.sock --------docker中间件
3.docker-server ---------docker服务器
查看docker在宿主机的位置

which docker

docker在宿主机的位置:/usr/bin/docker

查看docker.sock在宿主机的位置

ls /var/run/docker.sock


使用docker的话，只需要docker-cli和docker.sock即可。如果要在jenkins容器内部使用宿主机的docker,只需要将宿主机中的/usr/bin/docker和/var/run/docker.sock挂载到jenkins容器即可。

进入jenkins容器内部
```
docker exec -it jenkins bash
```
验证执行docker 命令
```
docker ps -a
```
退出jenkins容器
```
exit
```
 ![4](/images/DockerAndJitee/4.png)
 2.6 日志查看 然后copy下 密钥（后面要用密钥）
```
docker logs jenkins
```
 ![5](/images/DockerAndJitee/5.png)
 查看容器
```
docker ps -a
```
访问Jenkins

在浏览器中输入:http://serverIp:port/访问jenkins，serverIp为docker宿主机的ip，port即为宿主机映射的端口
输入上面复制的密钥

安装推荐插件
 ![6](/images/DockerAndJitee/6.png)
 因为网络原因，需要将插件源设置为国内的，这样才可以安装插件。进入宿主机目录 /home/jenkins_home/，编辑文件 hudson.model.UpdateCenter.xml

1.进入宿主机目录 /docker/jenkins_home/
```
cd /docker/jenkins_home/
```
2.查看 hudson.model.UpdateCenter.xml
```
cat hudson.model.UpdateCenter.xml 
```
 ![7](/images/DockerAndJitee/7.png)
 3.编辑 hudson.model.UpdateCenter.xml
```
vim hudson.model.UpdateCenter.xml
```
输入E,进入编辑模式，将 url 内容修改为 https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json（清华大学官方镜像）
 ![8](/images/DockerAndJitee/8.png)
 修改完成后，按ESC键，退出编辑模式，再输入: wq,强制保存并退出

3.重启容器及配置环境、插件
```
docker restart jenkins
```
访问Jenkins

在浏览器中输入:http://serverIp:port/访问jenkins，serverIp为docker宿主机的ip，port即为宿主机映射的端口
输入上面复制的密钥

 ![10](/images/DockerAndJitee/10.png)
  安装推荐插件

 ![11](/images/DockerAndJitee/11.png)
 ![12](/images/DockerAndJitee/12.png)
  插件安装完成，创建管理员用户

 ![13](/images/DockerAndJitee/13.png)
  最后进入jenkins ui

 ![15](/images/DockerAndJitee/15.png)
  3.1Jenkins插件安装、环境配置
Gitee
 ![16](/images/DockerAndJitee/16.png)
 ![17](/images/DockerAndJitee/17.png)
 ![18](/images/DockerAndJitee/18.png)
 ![19](/images/DockerAndJitee/19.png)
   以上个插件从Available plugins 中 install

 ![20](/images/DockerAndJitee/20.png)
    
   重启jenkins 中文就来了
```
docker restart jenkins
```
1
配置maven （直接使用jenkins自带的，问题就是第一构建比较慢）
 ![21](/images/DockerAndJitee/21.png)
 ![22](/images/DockerAndJitee/22.png)
 ![23](/images/DockerAndJitee/23.png)
 ![24](/images/DockerAndJitee/24.png)
 ![25](/images/DockerAndJitee/25.png)
 ![26](/images/DockerAndJitee/26.png)
   以上基础配置差不多了

3.2新建一个maven项目
 ![27](/images/DockerAndJitee/27.png)
 配置源码 gitee 项目
Credentials 还是需要填写GItee 账号密码
 ![28](/images/DockerAndJitee/28.png)
 ![29](/images/DockerAndJitee/29.png)
 ![30](/images/DockerAndJitee/30.png)
 ![31](/images/DockerAndJitee/31.png)
 ![32](/images/DockerAndJitee/32.png)
 ![33](/images/DockerAndJitee/33.png)
     Build

pom.xml
```
clean package -Dmaven.test.skip=true
```
 ![34](/images/DockerAndJitee/34.png)
 ![35](/images/DockerAndJitee/35.png)
 ![38](/images/DockerAndJitee/38.png)
 shell 脚本
 ```
 #!/bin/bash
 #服务名称
 SERVER_NAME=zhnc
 #镜像tag
 IMAGE_TAG=latest
 #镜像名称
 IMAGE_NAME=$SERVER_NAME:$IMAGE_TAG
 echo "------ 开始构建镜像：${SERVER_NAME} ------"
 docker build -t ${IMAGE_NAME} .
 echo "------ 镜像构建结束：${IMAGE_NAME} ------"
 ISFLAG=$(docker ps -q -f "name=${SERVER_NAME}")
 if [ -n  "$ISFLAG" ];then
     echo "------ 容器正在运行：${SERVER_NAME} ------"
     echo "------ 停止容器：$SERVER_NAME ------"
     docker stop $SERVER_NAME
     echo "------ 删除容器：$SERVER_NAME ------"
     docker rm $SERVER_NAME
 else
     echo "------ 容器未在运行：${SERVER_NAME} ------"
 fi

 echo "------ 开始运行容器：$SERVER_NAME ------"
 docker run -d --name $SERVER_NAME --restart always -p 7530:7530 $IMAGE_NAME
 echo "------ 清理虚悬镜像 ------"
 if [ -n   "$(docker images | grep "none" | awk '{print $3}')" ];then
     docker rmi -f $(docker images | grep "none" | awk '{print $3}')
 fi
```
看不懂自己百度
保存

DockerFile
注意dockerfile位置
 ![36](/images/DockerAndJitee/36.png)
 ```
 FROM anapsix/alpine-java:8_server-jre_unlimited

RUN mkdir -p /ruoyi/server/logs \
    /ruoyi/server/temp \
    /ruoyi/skywalking/agent

WORKDIR /ruoyi/server

ENV SERVER_PORT=7530

EXPOSE ${SERVER_PORT}

ADD ruoyi-admin/target/*.jar ./app.jar

ENTRYPOINT ["java", \
            "-Djava.security.egd=file:/dev/./urandom", \
            "-Dserver.port=${SERVER_PORT}", \
            # 应用名称 如果想区分集群节点监控 改成不同的名称即可
#            "-Dskywalking.agent.service_name=ruoyi-server", \
#            "-javaagent:/ruoyi/skywalking/agent/skywalking-agent.jar", \
            "-jar", "app.jar"]
```
 ![37](/images/DockerAndJitee/37.png)
