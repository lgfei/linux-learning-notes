# 从最新版本的linux系统开始，默认的是 Mariadb而不是mysql！
# 需进行下面操作安装mysql
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm  
rpm -ivh mysql-community-release-el7-5.noarch.rpm  
yum install mysql-server  

# 启动mysql
service mysqld start

# 查看是否启动
service mysqld status

# root用户登录（默认没有密码）
mysql -u root

# 重置root密码
<pre><code>
use mysql;  
update user set password=password('yourpassword') where user='root' and host='localhost';  
flush privileges;  
</code></pre>

# 授权root用户远程登录
<pre><code>
grant all privileges on *.* to 'root'@'%' identified by 'yourpassword' with grant option;  
flush privileges;
</code></pre>
