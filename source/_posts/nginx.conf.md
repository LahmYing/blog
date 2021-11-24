---
title: nginx.conf
date: 2021-02-26
tags: 网络运维
category: [网络运维]
---

<!-- toc -->

nginx 配置

```nginx
#定义Nginx运行的用户和用户组
#user  nobody;

#nginx进程数，建议设置为等于CPU总核心数。
worker_processes  1;

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#进程文件
#pid        logs/nginx.pid;

events {
    #单个进程最大连接数（最大连接数=连接数*进程数）
    worker_connections  1024;
}


http {
    include       mime.types; #文件扩展名与文件类型映射表
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    # 开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，
    # 对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，
    # 以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    sendfile        on;

    #tcp_nopush     on; #防止网络阻塞

    #keepalive_timeout  0;
    keepalive_timeout  60; #长连接超时时间，单位是秒

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        root   /data/app/nginx/html/dist;

        error_page 405 =200 $uri;

        #access_log  logs/host.access.log  main;

        location / {
            # 尝试读取 root + index，即 /data/app/nginx/html/dist/index.html
            index  index.html;

            # $uri  这个是nginx的一个变量，存放着用户访问的地址,
            # 比如：http://www.xxx.com/index.html, 那么$uri就是 /index.html
            # $uri/ 代表访问的是一个目录，比如：http://www.xxx.com/hello/test/    ，
            # 那么 $uri/ 就是 /hello/test/
            # 都不行则尝试第三个量 /index.html
            try_files $uri $uri/ /index.html;
        }
    }
}

```
