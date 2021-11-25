#### 安装mysql

sudo  apt install mysql-server

#### 安装保护mysql插件

sudo mysql_secure_installation

[安装过程具体选项信息参考](https://mal-suen.github.io/2018/05/27/MySQL安全设置命令mysql_secure_installation/)

#### 解决远程登录问题

1. ###### 打开防火墙（端口）

2. ###### 创建用户

   CREATE USER 'root'@'%' IDENTIFIED BY 'password'; //创建用户
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'; //设置权限

3. ###### 打开mysqld.cnf文件，取消本地限制

   sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

   在bind-adress = 127.0.0.1前加上#号；

4. ###### 重启mysql服务

   service mysql restart

5. ###### 远程连接MySQL