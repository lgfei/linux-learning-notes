# 查看是否有安装jdk
rpm -qa | grep jdk

# 卸载已安装jdk
rpm -qa | grep java | xargs rpm -e --nodeps

# 输入以下命令，查看可用的jdk软件包列表
yum search java | grep -i --color JDK

# 选择一个版本安装，下面以java-1.8.0-openjdk-devel.x86_64 为例
yum install java-1.8.0-openjdk-devel.x86_64

# 配置全局使用让系统上的所有用户使用java(openjdk)
vim /etc/profile

# 添加下面3行至/etc/profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.71-2.b15.el7_2.x86_64  
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
export PATH=$PATH:$JAVA_HOME/bin

# 让配置生效
source  /etc/profile

# 检查全局变量是否生效
echo $JAVA_HOME  
echo $CLASSPATH  
echo $PATH  

# 切换用户检查jdk版本
java -version  
