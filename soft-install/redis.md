安装epel源，CentOS默认的安装源在官方的centos.org上，而redis在第三方的yum源里，因此无法安装。
非官方的yum推荐用fedora的epel仓库。epel (Extra Packages for Enterprise Linux)是基于Fedora的一个项目，该仓库下有非常多的软件，建议安装  
# 安装epel仓库
yum install epel-release

# 安装redis命令
yum install redis

# 查看Redis安装了哪些文件
find / -name "redis*"

# 启动Redis服务
service redis start

# 查看redis启动状态
service redis status

# 打开Redis客户端
redis-cli

# 修改配置文件/etc/redis.conf
1、允许远程访问redis，除需要开放服务器端口号6379，还需将配置文件中的bind 127.0.0.1注释掉  
2、设置连接密码，添加 requirepass xxx(你的密码)  

# 启动失败
异常日志：WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.  
在/etc/rc.local 添加如下代码 
<pre><code>
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then  
echo never > /sys/kernel/mm/transparent_hugepage/enabled  
fi
</code></pre>
