---
title: consul-upsync-nginx
date: 2020.02.10 22:35:34
tags:
  - consul
  - upsync
  - nginx
---

## consul-upsync-nginx
如果 Nginx 遇到大流量和高负载，修改配置文件重启可能并不总是那么方便，因为恢复 Nginx 并重载配置会进一步增加系统负载，并很可能暂时降低性能。而一个个修改配置文件也是很容易出错和费时间的操作。
consul+nginx-upsync-module 实现 Nginx 的动态负载。

## consul

### install consul

download: https://www.consul.io/downloads.html

+ start consul
```shell script
>./consul agent -dev 
```

+ stop consul

```shell script
>./consul stop
```

+ reload consul

```shell script
>./consul reload
```

+ stop consul
```shell script
 ./consul leave

```


### UI

ui web address: http://localhost:8500/

### http api add service

curl -X PUT http://192.168.212.131:8500/v1/catalog/register

json body:
```json
{"Datacenter": "dc1",
 "Node":"tomcat", "Address":"192.168.5.165",
 "Service": {
    "Id" :"192.168.5.165:8080", 
    "Service": "itmayiedu","tags": ["dev"], "Port": 8080
 }
}
```

Datacenter指定数据中心，Address指定服务IP，Service.Id指定服务唯一标识，
Service.Service指定服务分组，Service.tags指定服务标签（如测试环境、预发环境等）
，Service.Port指定服务端口。

ui web address: http://localhost:8500/ui/dc1/services

### http api add key/value

curl -X PUT http://127.0.0.1:8500/v1/kv/upstreams/tcp/127.0.0.1:6662

ui web address: http://localhost:8500/ui/dc1/kv/upstreams


### add service configuration file in consul

ref: https://learn.hashicorp.com/consul/getting-started/services

```shell script
>mkdir ./consul.d

>echo '{"service":
   {"name": "web",
    "tags": ["rails"],
    "port": 80
   }
 }' > ./consul.d/web.json

>./consul agent -dev -config-dir=./consul.d
```

## nginx-upsync-module

+ nginx version

nginx version is 1.14.0

+ nginx-stream-upsync-module version

nginx-stream-upsync-module version is 1.2.2

+ nginx-upsync-modul version

nginx-upsync-modul version is 2.1.2

+ afresh configuration nginx

from github clone nginx-stream-upsync-module: https://github.com/xiaokai-wang/nginx-stream-upsync-module/releases  
from github clone nginx-upsync-module: https://github.com/weibocom/nginx-upsync-module/releases  


```shell script
nginx-1.14.0> mkdir addons
nginx-1.14.0>mv nginx-stream-upsync-module addnos/
nginx-1.14.0>mv nginx-upsync-module addons/

# --with-stream is not dynamic
nginx-1.14.0>./configure --with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-GkiujU/nginx-1.14.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=/var/log/nginx/error.log --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_geoip_module=dynamic --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_xslt_module=dynamic --with-stream_ssl_module --with-mail=dynamic --with-mail_ssl_module --with-stream_realip_module --without-http_rewrite_module --add-module=addons/nginx-upsync-module-2.1.2 --add-module=addons/nginx-stream-upsync-module-1.2.2 --with-stream

```

+ make && make install

+ configuration file

++ http upstream  
upsteram name is web in consul

>nano /etc/nginx/sites-available/default
```shell script
upstream backend {
    server 127.0.0.1:3001;
    server 127.0.0.1:3002;
    upsync 127.0.0.1:8500/v1/kv/upstreams/web upsync_timeout=6m upsync_interval=500ms upsync_type=consul strong_dependency=off;

    upsync_dump_path /usr/local/nginx/conf/servers/servers_test.con;
}

```

++ tcp upstream

upsteram name is tcp in consul

>nano /etc/nginx/nginx.conf

```shell script
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

stream {
  upstream tcpserver {
    server 127.0.0.1:6661;
    server 127.0.0.1:6662;

    upsync 127.0.0.1:8500/v1/kv/upstreams/tcp upsync_timeout=6m upsync_interval=500ms upsync_type=consul strong_dependency=off;

    upsync_dump_path /usr/local/nginx/conf/servers/servers_tcp.con;

  }
 
  server {
    listen 7777;
    proxy_connect_timeout 10s;
    proxy_timeout 5m;
    proxy_pass tcpserver;
  }
  
}

```

+ reload nginx