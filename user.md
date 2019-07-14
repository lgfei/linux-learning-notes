# 创建用户mayun并指定用户根路径为/home/mayun，且在/home/mayun目录下有读写权限
<pre>
新建用户
useradd -d /home/mayun mayun
设置密码
passwd mayun
用户授权
chown -R mayun:mayun /home/mayun
</pre>
