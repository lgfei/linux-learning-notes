# linux-learning-notes (linux 学习笔记)

查看centos版本
```
cat /etc/redhat-*
```
重启
```
reboot
```
查看服务日志
```
journalctl -xefu xxx
```
查看端口占用
```
netstat -lnp|grep 8080
```
windows系统编写的sh文件再linux执行不了，需要用dos2unix进行格式转化
```
yum install -y dos2unix
dos2unix xxx.sh
```
查看selinux配置
```
cat /etc/selinux/config
```
解压缩命令
```
tar -zcvf 压缩文件名 .tar.gz 被压缩文件名
tar -zxvf 压缩文件名.tar.gz
```
