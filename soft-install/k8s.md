# 关闭centos自带的防火墙
systemctl disable firewalld  
systemctl stop firewalld  

# 安装etcd和kubernetes软件（会自动安装docker）
yum install -y etcd kubernetes  

# 修改配置
1、修改Docker配置文件/etc/sysconfig/docker，OPTIONS='--selinux-enabled=false --insecure-registry gcr.io'  
2、修改配置文件/etc/kubernetes/apiserver，把–admission_control参数中的ServiceAccount删除

# 按顺序启动所有服务
systemctl start etcd  
systemctl start docker  
systemctl start kube-apiserver  
systemctl start kube-controller-manager  
systemctl start kube-scheduler  
systemctl start kubelet  
systemctl start kube-proxy  

# 查看版本
kubectl version
