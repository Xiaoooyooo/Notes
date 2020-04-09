# CentOS的Nginx环境搭建





## 1.NodeJS 环境安装

> 环境变量配置：
>
> 	vim /etc/profile
> 	在末尾添加:
> 		export NODE_PATH=[Nodejs路径]	//Node根目录
> 		export PATH=$NODE_PATH/bin:$PATH



## 2.Nginx环境

### 2.1Nginx安装准备

> **2.1.1**安装make
> yum -y install gcc automake autoconf libtool make
>
> **2.1.2**安装g++
> yum install gcc gcc-c++

### 2.2安装辅助软件

> **2.2.1**安装PCRE库
> wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.43.tar.gz
>
> ```
> tar -zxvf pcre-8.43.tar.gz
> cd pcre-8.43
> ./configure
> make && make install
> ```
>
> **2.2.2**安装zlib库
>
> wget http://zlib.net/zlib-1.2.11.tar.gz
>
> ```
> tar -zxvf zlib-1.2.11.tar.gz
> cd zlib-1.2.11
> ./configure
> make && make install
> ```
>
> **2.2.3**安装ssl
>
> wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
>
> ```
> tar -zxvf openssl-1.0.1t.tar.gz
> //无后续操作
> ```



### 2.3 安装nginx

wget http://nginx.org/download/nginx-1.16.0.tar.gz

```
tar -zxvf nginx-1.16.0.tar.gz
cd nginx-1.16.0

./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-pcre=/usr/local/nginx_src/pcre-8.43 --with-zlib=/usr/local/nginx_src/zlib-1.2.11 --with-openssl=/usr/local/nginx_src/openssl-1.1.1c

make && make install
```



### 3.将Nginx注册为服务随系统启动

> **3.1**创建服务文件
>
> vim /lib/systemd/system/nginx.service
>
> **3.2**在nginx.service中写入以下内容
>
> ```
> [Unit]
> Description=nginx service
> After=network.target 
>    
> [Service] 
> Type=forking 
> ExecStart=/usr/local/nginx/sbin/nginx
> ExecReload=/usr/local/nginx/sbin/nginx -s reload
> ExecStop=/usr/local/nginx/sbin/nginx -s quit
> PrivateTmp=true 
>    
> [Install] 
> WantedBy=multi-user.target
> ```
>
> **3.3**常用操作
>
> **service nginx start	启动服务**
>
> **service nginx stop	关闭服务**
>
> **service nginx reload	重启服务**
>
> systemctl start nginx.service　         启动nginx服务
> systemctl stop nginx.service　          停止服务
> systemctl restart nginx.service　       重新启动服务
> systemctl list-units --type=service     查看所有已启动的服务
> systemctl status nginx.service          查看服务当前状态
> systemctl enable nginx.service          设置开机自启动
> systemctl disable nginx.service         停止开机自启动



#### 参考文献

[CentOS7下设置nginx开机自动启动](https://www.jianshu.com/p/d238fe91d8f5)

[Nginx安装](http://www.nginx.cn/install)