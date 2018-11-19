安装配置
-------

>  **Nginx 下载**

*下载地址：* [http://nginx.org/download/](http://nginx.org/download/)


>   **Nginx安装**

_源码包安装_

`cd /usr/local/src
 wget http://nginx.org/download/nginx-1.4.2.tar.gz
 tar -zxvf nginx-1.4.2.tar.gz
 cd nginx-1.4.2`

`./configure --sbin-path=/usr/local/nginx/nginx \
 --conf-path=/usr/local/nginx/nginx.conf \
 --pid-path=/usr/local/nginx/nginx.pid \
 --with-pcre=/opt/app/openet/oetal1/chenhe/pcre-8.37 \
 --with-zlib=/opt/app/openet/oetal1/chenhe/zlib-1.2.8 \
 --with-openssl=/opt/app/openet/oetal1/chenhe/openssl-1.0.1t`

`make
 make install`

_yum安装_

   `yum install nginx`

   操作方式： `service nginx [start|restart|stop|reload]`


Nginx 配置





