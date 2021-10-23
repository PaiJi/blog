---
title: 全站再次搬迁到阿里云轻量香港区！
tags: []
categories:
  - - Diary
date: 2019-01-14 21:57:52
permalink: 'move-website-to-aliyun/'
---

近来在ping.cat 上看到LightSail的SGP线路炸得不成样子，访问十分缓慢，这怎么可以呢？！毕竟打开太慢的话就根本不想上来更新了！

思索了半天，遂下手买了阿里云轻量实例香港区一台，按照METO聚聚的[《服务器安装ubuntu18.04》](https://i-meto.com/netboot-ubuntu-18-04/)一文将自带的Ubuntu16重装到了18.04LTS版本，部署了LNMP1.6的beta版本，该版本升级了PHP7.3以及TLS3，虽然是beta不过还是咬咬牙上了233。
<!-- more -->
在这个过程中，遇到了Acme.sh签发泛域名证书的一些坑，第一次不知道为什么没有签署为通配符证书，于是删除重建，这次是通配符证书没错了，但是只匹配了

``` bash
*.jipai.moe
```

这就导致直接访问https://jipai.moe 上报了不信任错误，有理有据，你jipai.moe确实不在\*.jipai.moe的匹配范围内嘛。于是删了再来，这次终于签上了

```
*.jipai.moe;jipai.moe
```

匹配的证书！高兴地打滚！接下来就是修改nginx的conf让nginx把所有HTTP请求都转到HTTPS下，同时还要对

```
http://jipai.moe
https://jipai.moe
```

这种情况做301 跳转到带www的。在这个过程中又发生了一件小插曲，目前全站有三个二级域名，每一个conf里都有一个listen 80 然后301到https 的配置，我试图用

```
server_name *.jipai.moe;
return 301 https://$server_name$request_uri;
```

这样的配置来达成一劳永逸的效果，结果失败了...  
nginx直接把 https://\*.jipai.moe扔给了浏览器，当场翻车。

总之，在浪费了大半天时间后，现有的服务都跑在了阿里云香港区上，目前来看食用速度良好，咕咕咕~

最后贴一下自己的nginx配置，存个档！

``` bash
server
    {
        listen 80;
        #listen [::]:80;
        server_name jipai.moe;
        return 301 https://www.$server_name$request_uri;
    }

server
    {
        listen 80;
        #listen [::]:80;
        server_name www.jipai.moe;
        return 301 https://$server_name$request_uri;
    }


server
    {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name jipai.moe;
        return 301 https://www.$server_name$request_uri;

        ssl_certificate /usr/local/nginx/conf/ssl/jipai.moe/fullchain.cer;
        ssl_certificate_key /usr/local/nginx/conf/ssl/jipai.moe/jipai.moe.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;
        # openssl dhparam -out /usr/local/nginx/conf/ssl/dhparam.pem 2048
        ssl_dhparam /usr/local/nginx/conf/ssl/dhparam.pem;

    }

server
    {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name jipai.moe *.jipai.moe;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /home/wwwroot/www.jipai.moe;

        ssl_certificate /usr/local/nginx/conf/ssl/jipai.moe/fullchain.cer;
        ssl_certificate_key /usr/local/nginx/conf/ssl/jipai.moe/jipai.moe.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;
        # openssl dhparam -out /usr/local/nginx/conf/ssl/dhparam.pem 2048
        ssl_dhparam /usr/local/nginx/conf/ssl/dhparam.pem;

        include rewrite/none.conf;
        #error_page   404   /404.html;

        # Deny access to PHP files in specific directory
        #location ~ /(wp-contentuploadswp-includesimages)/.*\.php$ { deny all; }

        include enable-php.conf;

        location ~ .*\.(gifjpgjpegpngbmpswf)$
        {
            expires      30d;
        }

        location ~ .*\.(jscss)?$
        {
            expires      12h;
        }

        location ~ /.well-known {
            allow all;
        }

        location ~ /\.
        {
            deny all;
        }

        access_log  /home/wwwlogs/jipai.moe.log;
    }
```

```
server
    {
        listen 80;
        #listen [::]:80;
        server_name lab.jipai.moe;
        return 301 https://$server_name$request_uri;
    }

server
    {
        listen 443 ssl http2;
        #listen [::]:443 ssl http2;
        server_name blog.jipai.moe;
        index index.html index.htm index.php default.html default.htm default.php;
        root  /home/wwwroot/lab.jipai.moe;

        ssl_certificate /usr/local/nginx/conf/ssl/jipai.moe/fullchain.cer;
        ssl_certificate_key /usr/local/nginx/conf/ssl/jipai.moe/jipai.moe.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers "TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
        ssl_session_cache builtin:1000 shared:SSL:10m;
        # openssl dhparam -out /usr/local/nginx/conf/ssl/dhparam.pem 2048
        ssl_dhparam /usr/local/nginx/conf/ssl/dhparam.pem;

        include rewrite/none.conf;
        #error_page   404   /404.html;

        # Deny access to PHP files in specific directory
        #location ~ /(wp-contentuploadswp-includesimages)/.*\.php$ { deny all; }

        include enable-php.conf;

        location ~ .*\.(gifjpgjpegpngbmpswf)$
        {
            expires      30d;
        }

        location ~ .*\.(jscss)?$
        {
            expires      12h;
        }

        location ~ /.well-known {
            allow all;
        }

        location ~ /\.
        {
            deny all;
        }

        access_log  /home/wwwlogs/lab.jipai.moe.log;
    }
```