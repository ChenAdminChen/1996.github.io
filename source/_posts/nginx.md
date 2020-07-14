---
title: nginx
date: 2020.02.10 22:35:34
tags:
  - load balancing
  - proxy
---

## nginx

nginx学习地址: http://nginx.org/

### install nginx

#### first method

```shell script
chen>sudo apt install nginx
```

#### second method

download: http://nginx.org/en/download.html

example: nginx-1.14.0

1. configuration module

```shell script

nginx-1.14.0>./configure --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-GkiujU/nginx-1.14.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_geoip_module=dynamic --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream=static --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module --with-stream_realip_module --without-http_rewrite_module

```

2. make

```shell script
nginx-1.14.0>make
```

3. make install

```shell script
nginx-1.14.0>sudo make install

nginx-1.14.0> ll  /usr/share/nginx

```

### nginx information

```shell script
nginx-1.14.0>./objs/nginx -V

nginx version: nginx/1.14.0
built by gcc 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1) 
built with OpenSSL 1.1.1  11 Sep 2018
TLS SNI support enabled
configure arguments: --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-GkiujU/nginx-1.14.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_geoip_module=dynamic --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream=dynamic --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module --with-stream_realip_module

```

### nginx start/stop/reload

```shell script
nginx-1.14.0>./objs/nginx  #start

nginx-1.14.0>./objs/nginx -s stop #stop

nginx-1.14.0>./objs/nginx -s reload #reload

```

### nginx http load-balancing

configuration default.conf
```shell script
nginx-1.14.0>cat /etc/nginx/sites-available/default

server {
	listen 80 default_server;
	listen [::]:80 default_server;
	
#	root /var/www/html;

	# Add index.php to the list if you are using PHP
#	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		proxy_pass http://backend;
#		try_files $uri $uri/ =404;
	}

}


upstream backend {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
}
```

### nginx tcp/udp load-balancing

TCP/UDP load-balancing use stream at nginx 1.9.0 version or last version

nginx add ngx-stream-core-module, it config --with-stream=dynamic

```shell script
nginx-1.14.0>cat /etc/nginx/nginx.conf

user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

# load ngx_stream_module.so
load_module /usr/lib/nginx/modules/ngx_stream_module.so;

...

stream {

#  默认的方式: 轮询的方式
  upstream tcpserver {
    server 127.0.0.1:6661;
    server 127.0.0.1:6662;
    server 127.0.0.1:6663;
  }

#   least_conn 最少连接数配置,采用一种固定的负载策略,最少连接数策略,也就是选择连接数最少的后台服务
  upstream tcpserver_least_conn {
    least_conn;
    server 127.0.0.1:6664;
    server 127.0.0.1:6665;
    server 127.0.0.1:6666;
  }

#   权轮询配置,在轮询策略的基础上给每个后台服务加上权重，权重越大，访问的概率越大
  upstream tcpserver_weight {
     server 127.0.0.1:6667 weight=5;
     server 127.0.0.1:6668;
     server 127.0.0.1:6669;
   }

#  最低平均延时配置(商业订阅的一部分提供)
#上面策略配置表示的是对于每个请求，可通过最低平均延时来选择后台服务，而这个最低平均延时则是看 least_time 指令中指定的参数计算出来的，主要有下面三个参数：
 
# connect：连接到后台服务花的时间
# first_byte：接收到第一个字节花的时间
# last_byte：接收到最后一个字节花的时间，也就是全部接收完的时间
 upstream tcpserver_least_time {
     hash $remote_addr consistent;
     server 127.0.0.1:6670;
     server 127.0.0.1:6671;
     server 127.0.0.1:6672;
   }

#  采用的是 hash 算法策略，对客户端的 IP 进行 hash，让每台客户端固定访问到后台的一台服务。
  upstream tcpserver_hash {
     hash $remote_addr consistent;
     server 127.0.0.1:6673;
     server 127.0.0.1:6674;
     server 127.0.0.1:6675;
   }

  server {
    listen 7777;
    proxy_connect_timeout 10s;
    proxy_timeout 5m;
    proxy_pass tcpserver;
  }
  
}

```

### nginx configuration test

```shell script
nginx-1.14.0>sudo ./objs/nginx -t

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

```

### nginx平滑添加模块

1、先查看nginx版本和已支持的模块
2、官网下载相同版本nginx
3、添加模块,<a href="nginx http load-balancing">重复 `nginx http load-balancing`</a>
4、编译成功后make，记住千万不要make install,这样会覆盖你以前的nginx
5、备份nginx

```shell script
mv /usr/local/nginx /usr/local/nginx.bak

```

6、复制编译目录下的nginx启动文件

```shell script
cp ./objs/nginx /usr/local/nginx/sbin/

```

7、启动测试
```shell script
./nginx -
```

8、重启
```shell script
./nginx -s reload
```

### nginx learn address

ref1: https://ciphertrick.com/load-balancing-apis-using-nginx-with-example/
ref2: http://nginx.org/en/docs/stream/stream_processing.html
ref3: http://rookiezhou.top/nginx-%E5%AE%9E%E7%8E%B0-TCP-%E4%BB%A3%E7%90%86%E5%8F%8A%E5%85%B6%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E4%B9%8B-stream-%E6%A8%A1%E5%9D%97/
ref4: https://bVlog.csdn.net/slovyz/article/details/53839488