# Django 应用部署与监控

## 1、部署准备
一台云服务器，初级版（1核1G）即可，安装CentOS7.2版本操作系统，建议最好使用[阿里云](https://www.aliyun.com/)；一个已经完成[ICP备案](https://help.aliyun.com/knowledge_detail/36922.html)的域名。

## 2、环境搭建
### 2.1、新增用户
操作CentOS服务器，最好不要使用root根用户，其一是防止误操作；其二是避免在部署的过程中出现权限相关问题。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ adduser yves  # 添加yves用户
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ passwd yves  # 设置yves的操作密码
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ usermod -aG wheel yves  # 将yves添加到超级权限组
```

### 2.2、Python环境
安装Python环境之前，需要在操作系统上安装必要软件并更新yum源。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum update  # 更新yum源
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum upgrade
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum install -y openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel gcc  # 安装必要软件
```
这个步骤需要一定时间，耐心等待完成之后再进行下一步的安装[Python-3.6.4](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz)。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ mkdir src  # 家目录下新建src目录存放下载文件
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd src  # 进入src目录
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ wget https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz  # 下载Python-3.6.4
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ tar -zxvf Python-3.6.4.tgz  # 解压
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd Python-3.6.4  # 进入解压目录
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ ./configure LD_RUN_PATH=/usr/local/lib LDFLAGS="-L/usr/local/lib" CPPFLAGS="-I/usr/local/include"
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ make LD_RUN_PATH=/usr/local/lib  # 编译
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo make install  # 安装
```
上述下载、编译、安装等操作完成后，需要验证是否安装成功，出现下列版本信息即表示安装成功。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ python3 -V
Python 3.6.4
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pip3 -V
pip 19.3.1 from /usr/local/lib/python3.6/site-packages/pip (python 3.6)
```
安装最好用的python虚拟环境和包管理工具[pipenv](https://cloud.tencent.com/developer/article/1328471)。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo pip3 install pipenv
```
问题：sudo: pip3: command not found

在环境变量中添加 alias sudo='sudo env PATH=$PATH' 并使其生效即可。

### 2.3、MySQL数据库安装
CentOS默认安装mariadb数据库，首先你需要卸载.
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ yum remove mariadb-libs.x86_64
```
安装MySQL数据库。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd ~/src/  # 进入src目录
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm  # 下载安装依赖文件
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum localinstall mysql57-community-release-el7-11.noarch.rpm
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum install mysql-community-server  # 安装MyS数据库
```
MySQL数据库的相关操作。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ service mysqld start  # 启动
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ service mysqld stop  # 停止
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ service mysqld restart  # 重启
```
查看MySQL数据库的初始密码。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cat /var/log/mysqld.log | grep password
```
登录MySQL数据库。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ mysql -uroot -p
```
修改MySQL数据库密码。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ SET PASSWORD = PASSWORD('123456');
```
开启远程连接
```
mysql> show databases;
mysql> use mysql;
mysql> show tables;
mysql> select Host, User from user \G;
mysql> update user set host = '%' where user = 'root';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```
开启genelog
```
mysql> set global general_log_file="/tmp/general.log"; 
mysql> set global general_log=on;
```
创建用户
```
mysql> create user 'imooc'@'%' identified by '123456';
```
赋于用户权限
```
mysql> grant all privileges on *.* to 'imooc'@'%' identified by '123456' with mysql> grant option;  # 所有权限
mysql> grant select on *.* to 'imooc'@'%' identified by '123456' with grant option;  # 查询权限
mysql> revoke all privileges on *.* from imooc;  # 收回权限
mysql> flush privileges;
```
忘记密码
```
[yves@iz2ze0mhixialmdhi9pn5vz ~]$ sudo vim /etc/my.cnf  # 添加skip-grant-tables
mysql> show databases;
mysql> user mysql;
mysql> update user set authentication_string = password("123456") where user = 'root';
[yves@iz2ze0mhixialmdhi9pn5vz ~]$ sudo vim /etc/my.cnf  # 去除skip-grant-tables
[yves@iz2ze0mhixialmdhi9pn5vz ~]$ sudo service mysqld restart
```

## 3、部署代码
将项目代码上传到部署目录下，方法比较多，推荐使用[Git](https://www.runoob.com/git/git-basic-operations.html)进行代码版本的管理，首先需要在服务器上安装相关应用并从远程仓库拉取代码，拉取完成后修改成生产环境的配置。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum install git  # 安装git应用
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ mkdir ~/apps/  # 创建项目部署目录
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd ~/apps/
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ git clone '远程仓库地址'  # 拉取代码
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd '项目目录'
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pipenv install --deploy --ignore-pipfile  # 安装所需依赖
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pipenv run python manage.py migrate  # 创建数据库
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pipenv run python manage.py runserver 0.0.0.0:8000  # 启动服务
```
服务启动后，即可以通过公网IP:8000端口访问应用，注意：务必在阿里云管理控制台放开8000端口，否则无法访问。

## 4、Gunicorn安装与使用
直接使用runserver命令启动的开发服务器并不适用与生产环境，因此使用[Gunicorn](https://pypi.org/project/gunicorn/)作为生产环境服务器。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pipenv install gunicorn  # 安装gunicorn
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pipenv run gunicorn projectname.wsgi -w 2 -k gthread -b 0.0.0.0:8000  # 项目目录下启动
```
启动服务后，即可以通过公网IP:8000端口访问应用，但此时css样式全部未加载导致页面乱的一塌糊涂，这并非bug，而是由于处理静态文件请求并不是Gunicorn擅长的事情，应该交由更专业的Nginx去做。

## 5、Nginx服务器安装与使用
Nginx是一个高性能的HTTP和反向代理web服务器，它的功能非常多，这里主要用它来处理静态文件以及将非静态文件的请求反向代理给Gunicorn。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum install epel-release -y
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo yum install nginx -y  # 安装Nginx
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo systemctl start nginx  # 启动nginx
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo systemctl stop nginx  # 停止nginx
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ sudo systemctl restart nginx  # 重启nginx
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ 
```
直接在浏览器输入公网IP，看到nginx欢迎页面即表示安装并启动成功。下面就是修改应用中的settings.py配置文件，推荐[Djecrety](https://djecrety.ir/)生成一个一个线上环境的SECRET_KEY。
```
DEBUG = False
ALLOWED_HOSTS = ['127.0.0.1', 'localhost ', '公网IP或域名']
SECRET_KEY = ’***‘
STATICFILES_DIRS  # 注释
STATIC_ROOT = os.path.join(BASE_DIR, 'static')  # 新增
```
将项目的静态资源统一收集到static目录下。
```
pipenv run python manage.py collectstatic
```
Nginx的配置位于/etc/nginx/nginx.conf文件中，其中项目的配置文件可以在/etc/nginx/conf.d/目录下新增，但是必须以.conf后缀结尾。
```
server {
    charset utf-8;
    listen 80;
    server_name 公网IP或域名;

    location /static {
        alias 绝对路径;
    }

    location /media {
        alias 绝对路径;
    }

    location / {
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:8000;
    }
}
```
配置文件新增完成之后，重启Nginx即可访问应用，至此基本完成Nginx与Gunicorn部署Django应用的目标。

## 6、Supervisor安装与使用
由于服务器与网络存在不稳定的情况，因此直接在控制台启动应用的方式存在宕机的风险，并且没办法对相关进程进行监控，因此使用Supervisor来管理Gunicorn进程，这样当服务器重新启动或者Gunicorn进程意外崩溃后，Supervisor会帮我们自动重启Gunicorn。
由于Supervisor目前还不支持Python3，因此需要使用CentOS系统自带的python2版本进行安装。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ pip install supervisor
```
在家目录下新建相关的文件夹。
```
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ mkdir -p ~/etc/supervisor/conf.d  # 创建配置文件夹
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ mkdir -p ~/etc/supervisor/var/log  # 创建日志文件夹
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ cd ~/etc/
[yves@iZ2zefeybcvjrhsdgz4uymZ ~]$ echo_supervisord_conf > supervisord.conf  # 生成supervisor配置文件
```
修改supervisor.conf，让Supervisor进程产生的一些文件生成到上面我们创建的目录下，而不是其默认指定的地方。
```
[unix_http_server]
file=/home/yves/etc/supervisor/var/supervisor.sock
logfile=/home/yves/etc/supervisor/var/log/supervisord.log
pidfile=/home/yves/etc/supervisor/var/supervisord.pid
user=yves
[supervisorctl] 
serverurl=unix:///home/yves/etc/supervisor/var/supervisor.sock
files=/home/yves/etc/supervisor/conf.d/*.ini
```
配置修改完成之后需要在conf.d文件夹下新增应用的配置文件，注意文件必须以.ini结尾。
```
[program:projectname]
command=pipenv run gunicorn projectname.wsgi -w 2 -k gthread -b 127.0.0.1:8000
directory=/home/yves/apps/projectname
autostart=true
autorestart=unexpected
user=yves
stdout_logfile=/home/yves/etc/supervisor/var/log/projectname-stdout.log
stderr_logfile=/home/yves/etc/supervisor/var/log/projectname-stderr.log
```
启动supervisor。
```
supervisord -c ~/etc/supervisord.conf  # -c 表示根据配置文件启动
```
进入supervisor管理控制台。
```
supervisorctl -c ~/etc/supervisord.conf
```

## 7、问题与解决
### 7.1、后台管理系统图片上传报500错误
这是由于Nginx的权限问题导致的，网上大多数资料说将'Chmod 777 /media'执行就好，但是我试了不行，后来将/etc/nginx/nginx.conf中的user改成root就好了。