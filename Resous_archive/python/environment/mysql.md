# MySQL 数据库安装
### 下载
* [官网下载](https://dev.mysql.com/downloads/mysql/)

### 安装
完成安装文件下载后，直接按照安装提示“**下一步**”，中途需要输入管理员密码。

### 登录
第一种方式是直接在电脑的程序中找到 MySQL 并将其启动；第二种则是通过命令终端进行启动，需要在环境变量中添加：
```
PATH="$PATH":/usr/local/mysql/bin
```
完成添加，刷新环境变量后，输入一下命令进入 MySQL 数据库：
```
yvesdeMacBook-Air:Resous_archive yves$ mysql -u root -p
Enter password: 
```