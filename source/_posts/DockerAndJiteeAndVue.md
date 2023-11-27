---
title: Jenkins + Docker + Gitee 部署前端vue(2)
---

在上篇已经通过docker-compose部署好了nginx
注意：前端部署
方案一：采用docker部署
方案二：采用jenkins编译，然后传到服务器nginx上(本文采取)

之所以采取方案二，为了能方便配置nginx

开始（有些步骤已经在上篇做过了，继续往下看）
1.jenkins 安装 Publish Over SSH 插件(远程传送压缩文件)以及nodeJS

![1](/images/DockerAndJiteeAndVue/1.png)
![2](/images/DockerAndJiteeAndVue/2.png)
![3](/images/DockerAndJiteeAndVue/3.png)
测试不通过可以在Publish over SSH的Passphrase添加root

2.linux 安装 unzip
使用yum命令安装unzip
```
sudo yum install unzip
```
3. 新建项目
![4](/images/DockerAndJiteeAndVue/4.png)
![5](/images/DockerAndJiteeAndVue/5.png)
![6](/images/DockerAndJiteeAndVue/6.png)
![7](/images/DockerAndJiteeAndVue/7.png)
![8](/images/DockerAndJiteeAndVue/8.png)
![9](/images/DockerAndJiteeAndVue/9.png)
![10](/images/DockerAndJiteeAndVue/10.png)
![11](/images/DockerAndJiteeAndVue/11.png)
```
npm -v
node -v
npm install --registry=https://registry.npmmirror.com
echo "************* 删除历史dist****************"
rm -rf ./dist/* 
#打包命令
npm run build:prod
#打印容器目录
echo ${PWD}
echo "************* 准备压缩dist****************"
cd dist
zip -q -r dist.zip *
echo "************* 完成压缩dist****************"
echo "************* 打包成功****************"
```
![12](/images/DockerAndJiteeAndVue/12.png)
![13](/images/DockerAndJiteeAndVue/13.png)
```
cd /docker/nginx/html/manage/
rm -rf dist/
mkdir dist
mv dist.zip dist/
cd dist
unzip -o dist.zip
```
如果是 仅仅部署一个项目 只需要cd 到
```
cd /docker/nginx/html
```
4.保存
4.1接下来配置nginx
先创建文件夹按下图目录（各位自便）
![14](/images/DockerAndJiteeAndVue/14.png)
在/docker/nginx/conf 文件夹下 创建nginx.conf 文件
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
        listen       80;
        server_name  localhost;

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
            proxy_pass http://server/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```
4.2 重启nginx
```
docker restart nginx
```
跑起来后查看下
```
docker ps
```
因为考虑nginx 部署多个前端 改好的如下：
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
        listen       80;
        server_name  localhost;

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
      
            index  index.html index.htm;
        }
     # 多个项目（后台管理系统）
        location /manage {
            alias  /usr/share/nginx/html/manage/dist;
            index  index.html;
        }
     # 多个项目（前端H5客户端）
        location /webapp {
            alias  /usr/share/nginx/html/webapp/dist;
            index  index.html;
        }

       

        location /prod-api/ {
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://XXXXXXXXXX:7530/;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```
详细参考这个博主
同样 docker/nginx/html 目录下创建相应的文件夹
![15](/images/DockerAndJiteeAndVue/15.png)
这是jenkins那边上传成功后的

![16](/images/DockerAndJiteeAndVue/16.png)
特别注意 1前端vite项目里有个这个，可以按我的这个配置来，体验下就知道怎么回事了

![17](/images/DockerAndJiteeAndVue/17.png)
finally
jenkins构建项目


https://blog.csdn.net/qq_22654727/article/details/132385989
https://blog.csdn.net/qq_22654727/article/details/132421317