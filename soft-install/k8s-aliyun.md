# 安装docker
<pre>
cd /etc/yum.repos.d/
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum list docker-ce.x86_64 --showduplicates | sort -r
yum -y install docker-ce-18.06.1.ce-3.el7
systemctl enable docker && systemctl start docker
docker --version
</pre>

# 安装kubernetes
vim /etc/yum.repos.d/kubernetes.repo
<pre>
[kubernetes]

name=Kubernetes

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/

enabled=1

gpgcheck=1

repo_gpgcheck=1

gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
</pre>
yum install -y kubelet kubeadm kubectl  
vim /etc/sysconfig/kubelet
<pre>
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
</pre>
vim /etc/sysctl.d/k8s.conf
<pre>
net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1

vm.swappiness=0
</pre>
sysctl --system  
systemctl enable kubelet && systemctl start kubelet

# 部署Master节点
初始化
<pre>
kubeadm init --apiserver-advertise-address=yourIP --image-repository registry.aliyuncs.com/google_containers --kubernetes-version yourK8sVersion --service-cidr=10.1.0.0/16 --pod-network-cidr=10.2.0.0/16 --ignore-preflight-errors=NumCPU
</pre>
为kubectl准备Kubeconfig文件
<pre>
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
</pre>
部署网络插件canal
<pre>
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/rbac.yaml
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/canal.yaml
</pre>
消除master节点的隔离（可选）
<pre>
默认情况下，k8s是不会在master节点上自动部署业务上需要的应用的，如果是测试环境机器数量比较少，可以将这个隔离给去掉
kubectl taint nodes --all node-role.kubernetes.io/master-
会看到类似这样的输出
node/izm5e9951st9peq42t8fkxz untainted
error: taint "node-role.kubernetes.io/master:" not found
说明已经消除了隔离
</pre>

# 部署Node节点
加入集群（Master节点初始化成功后输出如下）
<pre>
kubeadm join yourIP:6443 --token b6a7mm.lhsec1zstpv4b9o6 \
    --discovery-token-ca-cert-hash sha256:cbcc28444595bd712b4a638e52861e6b0c12d089f1a8f9086d95802c1985fb27
</pre>
标签管理
<pre>
kubectl label nodes yourNodeName node-role.kubernetes.io/node=  // 标记为node节点
kubectl label nodes yourNodeName k8s.lgfei.com/namespace=kube-lgfei-test // 添加标签
kubectl label nodes yourNodeName k8s.lgfei.com/namespace=kube-lgfei-prd --overwrite // 修改标签
kubectl label nodes yourNodeName k8s.lgfei.com/namespace- // 删除标签
</pre>
配置admin.conf
<pre>
复制主节点的/etc/kubernetes/admin.conf到从节点的相同目录下
echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> ~/.bash_profile
source ~/.bash_profile
</pre>

# 查看部署结果
<pre>
kubectl get cs
kubectl get node --show-labels
kubectl get pods --all-namespaces
</pre>

# 镜像仓库Harbor部署
<pre>
https://github.com/vmware/harbor/releases
建议下载offline的压缩包，里面包含了harbor启动所用的所有docker镜像，可以快速的部署harbor
cd /usr/local/src
wget https://storage.googleapis.com/harbor-releases/release-1.8.0/harbor-offline-installer-v1.8.1.tgz
tar zxf harbor-offline-installer-v1.8.1.tgz

Harbor的每个组件都是以Docker容器的形式构建的，使用Docker Compose来对它进行部署，你可以查看docker-compose.yml文件
Docker Compose安装
yum install -y docker-compose
docker-compose --version

通过docker-compose.yml修改端口
通过harbor.yml修改hostname
如果使用http的方式配置harbor需要为所有Docker添加信任配置。
vim /etc/docker/daemon.json
{

"registry-mirrors": ["https://tdimi5q1.mirror.aliyuncs.com"],

"insecure-registries" : ["http://192.168.10.1"]

}
systemctl restart docker

安装Harbor
cd /usr/local/src/harbor/
./install.sh
docker-compose ps

使用Harbor管理Registry 
web登录：http://192.168.10.1  默认用户名密码  admin/Harbor12345
后台登录：docker login http://192.168.10.1

提交镜像到Registry
docker tag centos:latest 192.168.10.1/system/centos:latest
docker push 192.168.10.1/system/centos:latest
</pre>

# 遇见的问题
1. 删除pod后，pod会自动重启，因为rc还存在，应先删除rc
<pre>
kubectl get rc
kubectl delete rc rcname
</pre>

2. 拉取不到最新的镜像
因为当你的版本和上次一样时，可能拉取的镜像不是最新的，在app_deploy_rc.yaml镜像拉取策略有3种
<pre>
imagePullPolicy: Always // 总是从镜像库中拉取
imagePullPolicy: IfNotPresent // 如果本地不存在才从镜像库中拉取
imagePullPolicy: Nerver // 只从本地拉取镜像
</pre>

# 参考来源
[m.unixhot.com](http://m.unixhot.com/kubernetes/kubernetes-aliyun.html)  
[www.kubernetes.org.cn](https://www.kubernetes.org.cn/5462.html)  
[connection to the server localhost:8080 was refused...](https://www.jianshu.com/p/6fa06b9bbf6a)  
