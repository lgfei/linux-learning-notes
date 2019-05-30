下载地址 https://www.mongodb.com/download-center#community
# 创建安装目录
mkdir /usr/local/mongodb  
mkdir /usr/local/mongodb/mongodbserver  

# 解压安装包
tar -zxvf mongodb-linux-x86_64-3.4.0.tgz -C /usr/local/mongodb/mongodbserver

# 添加配置
mkdir /usr/local/mongodb/mongodbserver/data  
mkdir /usr/local/mongodb/mongodbserver/log  
mkdir /usr/local/mongodb/mongodbserver/conf  
cd /usr/local/mongodb/mongodbserver/conf  
echo '' > mongodb.conf

# mongodb.conf 的内容
dbpath=/usr/local/mongodb/mongodbserver/data  
logpath=/usr/local/mongodb/mongodbserver/log/mongodb.log  
port=27017  
fork=true  
journal=false  
storageEngine=mmapv1  

# 启动
cd /usr/local/mongodb/mongodbserver/bin    
./mongod --config /usr/local/mongodb/mongodbserver/conf/mongodb.conf  

# 浏览器访问
http://ip:27017/  
页面返回 It looks like you are trying to access MongoDB over HTTP on the native driver port.  
说明启动成功

# 添加管理用户(mongoDB 没有无敌用户root，只有能管理用户的用户 userAdminAnyDatabase)，利用mongo命令连接mongoDB服务器端
cd /usr/local/mongodb/mongodbserver/bin  
./mongo  
> use admin  
switched to db admin  
> db.createUser( {user: "pfnieadmin",pwd: "123456",roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]});  
添加完用户后可以使用show users或db.system.users.find()查看已有用户.  
添加完管理用户后，使用db.shutdownServer()关闭MongoDB，并使用权限方式再次开启MongoDB，这里注意不要使用kill直接去杀掉mongodb进程，（如果这样做了，请去data/db目录下删除mongo.lock文件） 
出现以下错误，是因为用户权限问题 
Error: shutdownServer failed: {  
"ok" : 0,  
"errmsg" : "not authorized on admin to execute command { shutdown: 1.0 }",  
"code" : 13  
} :  
_getErrorWithCode@src/mongo/shell/utils.js:25:13  
DB.prototype.shutdownServer@src/mongo/shell/db.js:302:1  
@(shell):1:1  
修改用户权限  
>db.updateUser(  
 "pfnieadmin",  
        {  
           roles : [  
                     {"role" : "userAdminAnyDatabase","db" : "admin"},  
                     {"role" : "dbOwner","db" : "admin"},  
                     {"role" : "clusterAdmin", "db": "admin"}  
                   ]  
        }  
 )  

# 使用权限方式启动MongoDB
在配置文件中添加：auth=true , 然后启动  
进入mongo shell，使用admin数据库并进行验证，如果不验证，是做不了任何操作的  
> use admin  
> db.auth("pfnieadmin","123456")   #认证，返回1表示成功  

# 将mongod路径添加到系统路径中，方便随处执行mongod命令
vim /etc/profile  
添加   
export PATH=$PATH:/usr/local/mongoDB/mongodbserver/bin 
使系统环境变量立即生效  
source /etc/profile  

# 将mongo路径软链到/usr/bin路径下，方便随处执行mongo命令
ln -s /usr/local/mongobd/mongodbserver/bin/mongo  /usr/bin/mongo  

# MongoDB设置为系统服务并且设置开机启动
vim /etc/rc.d/init.d/mongod  
内容如下：  
start() {  
/usr/local/mongodb/mongodbserver/bin/mongod  --config /usr/local/mongodb/mongodbserver/conf/mongodb.conf 
}  
  
stop() {  
/usr/local/mongodb/mongodbserver/bin/mongod --config /usr/local/mongodb/mongodbserver/conf/mongodb.conf --shutdown  
}  
case "$1" in  
  start)  
 start  
 ;;  
  
stop)  
 stop  
 ;;  
  
restart)  
 stop  
 start  
 ;;  
  *)  
 echo  
$"Usage: $0 {start|stop|restart}"  
 exit 1  
esac

保存完成之后，添加脚本执行权限，命令如下：chmod +x /etc/rc.d/init.d/mongod  
使用下面命令启动，停止，重启  
service mongod start  
service mongod stop  
service mongod restart  
