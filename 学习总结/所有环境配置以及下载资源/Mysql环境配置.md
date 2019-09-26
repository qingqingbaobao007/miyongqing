mysql创建用户以及下载

1.sudo apt install mysql-server mysql-client  可以分开下载

创建用户：
1.启动数据库并查看数据库状态

​	*systemctl start mysql.service*

​	*ps aux | grep mysql*

2.链接数据库

​	*mysql -u root -p*

3.查看数据库用户

  *use mysql*

4.创建用户可以远程访问

​	grant all privileges on *.* to 'root'@'%' identified by '123456' with

​	grant option；

5.将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里

​	flush privileges;

6.查看数据库

select host,user from mysql.user;