# 1 Nginx简介
`Nginx`是俄罗斯人`Igor Sysoev`编写的轻量级Web服务器，它的发音为 `[ˈendʒɪnks]`，它不仅是一个高性能的`HTTP`和反向代理服务器，同时也是一个`IMAP/POP3/SMTP` 代理服务器。

- 在高连接并发的情况下，`Nginx`是`Apache`服务器不错的替代品:
- `Nginx`作为负载均衡服务器: `Nginx` 可以在内部直接支持 `Rails` 和 `PHP`程序对外进行服务, 也可以支持作为 `HTTP`代理服务器对外进行服务. `Nginx`采用`C`进行编写.

目前最新主流版本和稳定[版本](https://www.nginx.cn/nginx-download "版本").
## 1.1 系统支持
`Nginx`支持的大部分的`BSD`衍生版和`Linux`发行版操作系统。
- FreeBSD 3.x, 4.x, 5.x, 6.x i386; FreeBSD 5.x, 6.x amd64;
- Linux 2.2, 2.4, 2.6 i386; Linux 2.6 amd64;
- Solaris 8 i386; Solaris 9 i386 and sun4u; Solaris 10 i386;
- MacOS X (10.4) PPC;
## 1.2 结构扩展
- 一个主进程和多个工作进程。工作进程是单线程的，且不需要特殊授权即可运行；
- kqueue (FreeBSD 4.1+), epoll (Linux 2.6+), rt signals (Linux 2.2.19+), /dev/poll (Solaris 7 11/99+), select, 以及 poll 支持；
- kqueue支持的不同功能包括 EV_CLEAR, EV_DISABLE （临时禁止事件）， NOTE_LOWAT, EV_EOF, 有效数据的数目，错误代码；
- sendfile (FreeBSD 3.1+), sendfile (Linux 2.2+), sendfile64 (Linux 2.4.21+), 和 sendfilev (Solaris 8 7/01+) 支持；
- 输入过滤 (FreeBSD 4.1+) 以及 TCP_DEFER_ACCEPT (Linux 2.4+) 支持；
- 10,000 非活动的 HTTP keep-alive 连接仅需要 2.5M 内存。
- 最小化的数据拷贝操作；

# 2 Nginx安装
本文描述`Nginx`安装主要通过`CentOS`系统环境和`Ubuntu`环境进行描述。环境搭建一般采用编译源码的方式进行安装。所以在环境搭建过程中需要准备`gcc g++`编译环境.

## 2.1 安装GCC编译环境

Ubuntu系统编译环境搭建
```shell
! ubuntu os
apt-get install build-essential
apt-get install libtool
```
CentOS系统编译环境搭建
```shell
yum -y install gcc automake autoconf libtool make
yum install gcc gcc-c++
```

一般我们都需要先装`pcre`, `zlib`，前者为了重写rewrite，后者为了`gzip`压缩。

## 2.2 安装PCRE库
```shell
cd /usr/local/src
wget https://ftp.pcre.org/pub/pcre/pcre-8.44.tar.gz 
tar -zxvf pcre-8.44.tar.gz
cd pcre-8.44
./configure
make
make install
```

## 2.3 安装zlib库
```shell
cd /usr/local/src
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```
## 2.4 安装SSL
```shell
cd /usr/local/src
wget https://www.openssl.org/source/openssl-1.1.1g.tar.gz
tar -zxvf openssl-1.1.1g.tar.gz
```
## 2.5 安装NGINX
```shell
cd /usr/local/src
wget http://nginx.org/download/nginx-1.18.0.tar.gz
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0

./configure --sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-http_gzip_static_module \
--with-http_stub_status_module \
--with-file-aio \
--with-http_realip_module \
--with-http_ssl_module \
--with-pcre=/usr/local/src/pcre-8.44 \
--with-zlib=/usr/local/src/zlib-1.2.11 \
--with-openssl=/usr/local/src/openssl-1.1.1g

make -j2
make install
```
其中`--with-pcre=/usr/local/src/pcre-8.44` 指的是pcre-8.44 的源码路径。`--with-zlib=/usr/local/src/zlib-1.2.11`指的是zlib-1.2.11 的源码路径。

安装后的目录内容如下所示
```shell
fastcgi.conf            koi-win             nginx.conf.default
fastcgi.conf.default    logs                scgi_params
fastcgi_params          mime.types          scgi_params.default
fastcgi_params.default  mime.types.default  uwsgi_params
html                    nginx               uwsgi_params.default
koi-utf                 nginx.conf          win-utf
```
# 3 测试
启动Nginx程序
```shell
sudo /usr/local/nginx/nginx
```
通过浏览器访问对应`ip`，得到`Nginx welcome`页面，说明环境搭建成功。