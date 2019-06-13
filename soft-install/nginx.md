# Nginx包可在EPEL存储库中找到，如果您没有安装EPEL存储库，可以运行以下命令
yum install epel-release

# 安装
yum install nginx

# 启用，禁用Nginx服务
<pre><code>
systemctl enable nginx
systemctl disable nginx
</code></pre>

# 启动，停止，重启，重新加载配置
<pre><code>
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
</code></pre>

# 如果您正在运行防火墙，则还需要打开端口80和443
<pre><code>
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --permanent --zone=public --add-service=https
firewall-cmd --reload
</code></pre>

# 检查Nginx服务的状态
systemctl status nginx

# 检查Nginx版本
nginx -v
