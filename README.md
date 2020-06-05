# linux-learning-notes (linux 学习笔记)

## 查看centos版本
```
cat /etc/redhat-*
```
## 查看系统是多少位的(如果有x86_64就是64位的，没有就是32位的,后面是X686或X86_64则内核是64位的，i686或i386则内核是32位的)
```
uname -a
```
## 重启
```
reboot
```
## 查看服务日志
```
journalctl -xefu xxx
```
## 查看端口占用
```
netstat -lnp|grep 8080
```
## windows系统编写的sh文件再linux执行不了，需要用dos2unix进行格式转化
```
yum install -y dos2unix
dos2unix xxx.sh
```
## 查看selinux配置
```
cat /etc/selinux/config
```
## 解压缩命令
```
tar -zcvf 压缩文件名 .tar.gz 被压缩文件名
tar -zxvf 压缩文件名.tar.gz

```
## 修改ssh端口
```
sed -i 's/Port 22/Port 2222/' /etc/ssh/sshd_config
service sshd restart
```
## 查看cpu
```
# 总核数 = 物理CPU个数 X 每颗物理CPU的核数 
# 总逻辑CPU数 = 物理CPU个数 X 每颗物理CPU的核数 X 超线程数
# 查看物理CPU个数
cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l
# 查看每个物理CPU中core的个数(即核数)
cat /proc/cpuinfo| grep "cpu cores"| uniq
# 查看逻辑CPU的个数
cat /proc/cpuinfo| grep "processor"| wc -l
```
## 查看是否虚拟机
```
dmidecode -s system-product-name
```
