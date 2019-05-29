# 下载安装包
mkdir /usr/local/zookeeper  
mkdir /usr/local/zookeeper/var  
mkdir /usr/local/zookeeper/var/log  
cd /usr/local/zookeeper  
wget http://apache.org/dist/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz

# 解压
tar -zxvf zookeeper-3.4.14.tar.gz

# 修改配置
cd /usr/local/zookeeper/zookeeper-3.4.14/conf
cp zoo_sample.cfg zoo.cfg
vim zoo.cfg
注释
dataDir=/tmp/zookeeper
添加
dataDir=/usr/local/zookeeper/var
dataLogDir=/usr/local/zookeeper/var/log

# 启动，查看状态，停止，重启
cd /usr/local/zookeeper/zookeeper-3.4.14/bin
启动
./zkServer.sh start
查看状态
./zkServer.sh status
停止
./zkServer.sh stop
重启
./zkServer.sh restart
