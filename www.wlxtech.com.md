# 部署路飞项目

参考文档[https://www.cnblogs.com/pyyu/p/10160874.html](https://www.cnblogs.com/pyyu/p/10160874.html)

## 搭建环境

### 服务器

纯净的阿里云linux ubuntu18.04 64位 服务器

ubuntu18.04自带python3.6.9和python2.7.17

### 域名

阿里云域名wlxtech.com

### 创建python虚拟环境

* **这里为了方便看目录结构，安装一个树形看目录结构的东西\(tree -a查看隐藏文件\)**

`apt update`

`apt install tree`

![](.gitbook/assets/image%20%288%29.png)

* **然后因为是原生环境，自带python2，所以python默认路径是python2，这里要改成python3，具体操作如下**

`update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1`

`update-alternatives --install /usr/bin/python python /usr/bin/python3.6 1`

我们列出可用的 Python 替代版本

`update-alternatives --list python`

然后进行配置

`update-alternatives --config python`

然后输入2，即可把python命令的默认版本变成python3.6，具体操作如下图

![](.gitbook/assets/image%20%285%29.png)

**正式安装虚拟环境**

1  先更新pip和setuptools

`pip3 install --upgrade setuptools`

`pip3 install --upgrade pip`

2  安装依赖

`sudo pip install pbr`

3  安装虚拟环境virtualenv

`pip install virtualenv`

4 安装统一管理虚拟环境软件virtualenvwrapper

`pip install virtualenvwrapper`

执行下面两句（下一句是配置，将所有虚拟环境目录都放在`~/.virtualenvs`）

`export WORKON_HOME='~/.virtualenvs'` 

`source /usr/local/bin/virtualenvwrapper.sh`

然后将上面两行添加到~/.bashrc 中，这样每次启动终端的时候都会自动运行，

`source ~/.bashrc`   修改环境变量之后立即生效

5 创建并使用虚拟环境

创建虚拟环境\(mkvirtualenv -p python语言版本的路径 虚拟环境的名称\)

`mkvirtualenv -p /usr/bin/python3.6 luffy`

创建的虚拟环境在/root/.virtualenvs也就意味着，在该虚拟环境中安装的python相关的包，也在这

不在虚拟环境中安装的原生的python相关的包在哪里呢？ 在/usr/local/lib/python3.6/dist-packages

虚拟环境的一些操作（下面四个别执行）

进入虚拟环境命令：workon 虚拟环境名称

退出虚拟环境命令：deactivate

删除虚拟环境命令：rmvirtualenv  虚拟环境名称

列出所有环境命令：workon 或者 lsvirtualenv -b

6 在~目录下创建两个文件夹当项目文件夹，一个是正式用的，一个是测试的

`mkdir luffy_project`

`cd luffy_project`

不报错的话，到目前为止都OK

## 将代码搞到服务器上

在linux上直接下载 

`wget https://files.cnblogs.com/files/pyyu/luffy_boy.zip`

`wget https://files.cnblogs.com/files/pyyu/07-luffy_project_01.zip`

下载解压工具包unzip

`apt install unzip`

开始解压

`unzip 07-luffy_project_01.zip`

`unzip luffy_boy.zip`

此时ls目录应该是这样的

![](.gitbook/assets/image%20%2825%29.png)

前后端代码都解压好了，目录为/root/luffy\_project

## 安装Node环境

**要在服务器上，编译打包vue项目，必须得有node环境，安装node环境**

下载node二进制包，此包已经包含node，不需要再编译\(要等一会\)

`wget https://nodejs.org/download/release/v8.6.0/node-v8.6.0-linux-x64.tar.gz`

解压缩 

`tar -zxvf node-v8.6.0-linux-x64.tar.gz`

解压缩之后执行一些命令，看看是否安装完成

![](.gitbook/assets/image%20%2820%29.png)

配置环境变量，使linux全局能访问node，PATH变量，修改/etc/profile（全局配置文件，每次登陆加载）

`vim /etc/profile`，在末尾加上下面一句

`PATH=$PATH:/root/luffy_project/node-v8.6.0-linux-x64/bin`

然后使其立刻生效

`source /etc/profile`

然后执行几行命令，验证上述操作生效

![](.gitbook/assets/image%20%2816%29.png)

至此node环境安装完成

## 打包Vue代码

进入vue源码目录

`cd ~/luffy_project/07-luffy_project_01`

先全局安装一下webpack，否则npm install可能会出错

`npm install webpack -g（这句好像可以不执行）`

安装vue模块，默认去装package.json的模块内容，如果出现模块安装失败，手动再装

`npm install`

**出现bug：sh: 1: node: Permission denied（权限问题）**

解决：执行如下两句

`npm config set user 0` 

`npm config set unsafe-perm true`

然后再执行**`npm install`**

准备编译打包vue项目，替换配置文件所有地址，改为服务器\(47.94.7.122\)地址

`sed -i "s/127.0.0.1/47.94.7.122/g" ~/luffy_project/07-luffy_project_01/src/restful/api.js`

此时打包vue项目，生成一个dist静态文件夹 

`cd ~/luffy_project/07-luffy_project_01`

`npm run build`

检查dist文件夹

`ls dist/`

正常结果是index.html static

## 配置后端代码

使用上面创建好的虚拟环境

`workon luffy`

创建项目依赖文件，安装项目依赖

`cd ~/luffy_project/luffy_boy`

`vim allenv.txt`

在文件中粘贴如下信息

```text
certifi==2018.11.29
chardet==3.0.4
crypto==1.4.1
Django==2.1.4
django-redis==4.10.0
django-rest-framework==0.1.0
djangorestframework==3.9.0
idna==2.8
Naked==0.1.31
pycrypto==2.6.1
pytz==2018.7
PyYAML==3.13
redis==3.0.1
requests==2.21.0
shellescape==3.4.1
urllib3==1.24.1
uWSGI==2.0.17.1
```

显示如下：

![](.gitbook/assets/image%20%2812%29.png)

进行依赖的安装：

`pip install -r allenv.txt`

然后成功安装如下（使用pip list可以看安装的包有哪些）

![](.gitbook/assets/image%20%2823%29.png)

查看是否能正常运行：

`python manage.py runserver`

结果应如下：（如果显示端口被占用，kill掉占用的进程）

![](.gitbook/assets/image%20%2814%29.png)

## 配置redis

安装redis

 `apt-get install redis-server`

安装后自动已经启动了redis服务，查看redis服务启动情况

`ps -ef|grep redis`

![](.gitbook/assets/image%20%2831%29.png)

试试连接redis服务看看行不行redis-cli进行连接

![](.gitbook/assets/image%20%2829%29.png)

成功启动

## 部署uwsgi

进入项目目录

`cd ~/luffy_project/luffy_boy`

进入虚拟环境

`workon luffy`

因为在安装项目依赖时已经安装了uwsgi，所以这里只要创建配置文件即可

`touch uwsgi.ini`

编辑uwsgi.ini添加如下内容

```text
[uwsgi]
# Django-related settings
# the base directory (full path)
chdir           = /root/luffy_project/luffy_boy
# Django's wsgi file
module          = luffy_boy.wsgi
# the virtualenv (full path)
home            = /root/.virtualenvs/luffy
# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 1
# the socket (use the full path to be safe)
# 结合nginx，使用的参数，将uwsgi运行在一个socket链接上
socket          = 0.0.0.0:9000
# 不用nginx的话，直接访问到后端的数据页面用http
# http = 0.0.0.0:9000
# clear environment on exit
vacuum          = true
```

启动uwsgi

`uwsgi --ini uwsgi.ini`

![](.gitbook/assets/image%20%2817%29.png)

后端在这里卡着就行

## 配置nginx

安装nginx，因为后端的连接在卡着，所以新建一个终端

![](.gitbook/assets/image%20%2813%29.png)

直接在终端安装，不用装在虚拟环境中，输入以下命令：

`apt-get update`

![](.gitbook/assets/image%20%289%29.png)

`apt-get install nginx`

安装成功后，用浏览器访问你的阿里云IP地址，可以看到以下提示 ：

![](.gitbook/assets/image%20%2822%29.png)

### 修改nginx配置文件

查找nginx配置文件的位置：[https://www.cnblogs.com/qianpangzi/p/10922420.html](https://www.cnblogs.com/qianpangzi/p/10922420.html)

我这配置文件在：/etc/nginx/nginx.conf

修改配置文件，在http最末尾添加server，只需要粘贴server即可，其他的代码用于做对比

```text
http {
    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    ##
    # Gzip Settings
    ##

    gzip on;

    # gzip_vary on;
    # gzip_proxied any;
    # gzip_comp_level 6;
    # gzip_buffers 16 8k;
    # gzip_http_version 1.1;
    # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    ##
    # Virtual Host Configs
    ##

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;

    server {
                listen       80;
                # server_name  47.94.7.122;
                server_name  www.wlxtech.com;
                # 只要请求来自于47.94.7.122/
                location / {
                        root /root/luffy_project/07-luffy_project_01/dist;
                        index index.html;
                        try_files $uri $uri/ /index.html;
                }
                error_page   500 502 503 504  /50x.html;
                location = /50x.html {
                        root   html;
                }
        }
}
```

然后还要将权限修改一下：

将配置文件的第一行`user www-data;` 修改为 `user root;`

重新加载nginx

/usr/sbin/nginx -s reload

然后可以访问[www.wlxtech.c](http://www.wlxtech.com)进行验证，看看有没有页面

然后在上一个server下面再加一个server

```text
    server {
            listen       8000;
            server_name  www.wlxtech.com;
            location / {
                    uwsgi_pass 0.0.0.0:9000;
                    include /etc/nginx/uwsgi_params;
            }
            location /static {
                    alias /opt/static;
            }

    }
```

大功告成

















































然后nginx配置：打开配置文件default，路径/etc/nginx/sites-available/default，设置以下内容。一个是server\_name后面换成你的阿里云公网IP，有的文章说不换也行。关键是下面2个location，第一个location是设置的和uWSGI的关联。第二个location /static是设置的静态文件的路径。如果你的项目还有media文件夹，那还要加一个location /media，把路径设置上。注意：location 和alias后面有空格。 

```text
server_name 47.94.7.122;
location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    # try_files $uri $uri/ =404;
    include  uwsgi_params;
    uwsgi_pass  127.0.0.1:8000;
}
```

**uWSGI安装：**

这个是安装在虚拟环境中，先workon blogs\_test进入虚拟环境，安装uWSGI前需要先安装依赖，输入以下命令完成安装

apt-get install build-essential python

apt-get install python-dev

pip install uwsgi

配置uWSGI：在django项目的根目录\(myblogs\)下，新建两个文件，

uwsgi.ini和run.log 。

touch uwsgi.ini

touch run.log 

第一个是uWSGI的配置文件，第二个是日志记录文件。设置uwsgi.ini文件如下：

\[uwsgi\] chdir = /home/myweb  
module = myweb.wsgi:application socket = 127.0.0.1:8000  
master = true  
daemonize = /home/myweb/run.log disable-logging = true wsgi-file = /home/myweb/myweb/wsgi.py pidfile=/home/myweb/uwsgi.pid chdir是django项目所在目录，socket后面的地址是和上面nginx配置文件中的地址uwsgi\_pass 127.0.0.1:8000对应的，规定nginx和uWSGI之间的通信端口。daemonize就是日志文件的路径。disable-logging = true 表示不记录正常信息，只记录错误信息。wsgi-file是你django项目根目录下项目同名目录中有一个wsgi.py文件的路径。pidfile是uwsgi.pid文件的路径，这个文件是uWSGI运行后自动生成的，里面记录了uWSGI的进程号，可以用来重启uWSGI。但是我的uwsgi.pid文件记录的进程号老是不对，用不了。



uWSGI基本命令：

```text
启动：uwsgi --ini uwsgi.ini停止：uwsgi --stop uwsgi.pid重启：uwsgi --reload uwsgi.pid
```

全部配置好后，重启nginx和uWSGI，因该就可以用浏览器访问你的项目了。





```text
apt install uwsgi-plugin-python3
```



\`\`

