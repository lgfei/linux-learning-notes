# 安装docker
<pre>
<code>
cd /etc/yum.repos.d/
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum list docker-ce.x86_64 --showduplicates | sort -r
yum -y install docker-ce-18.06.1.ce-3.el7
systemctl enable docker && systemctl start docker
docker --version
</code>
</pre>

# 安装kubernetes
vim /etc/yum.repos.d/kubernetes.repo
<pre>
<code>
[kubernetes]

name=Kubernetes

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/

enabled=1

gpgcheck=1

repo_gpgcheck=1

gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
</code>
</pre>
yum install -y kubelet kubeadm kubectl  
vim /etc/sysconfig/kubelet
<pre>
<code>
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
</code>
</pre>
vim /etc/sysctl.d/k8s.conf
<pre>
<code>
net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1
</code>
</pre>
sysctl --system  
systemctl enable kubelet && systemctl start kubelet

# 部署Master节点
初始化
<pre>
<code>
kubeadm init --apiserver-advertise-address=yourIP --image-repository registry.aliyuncs.com/google_containers --kubernetes-version v1.15.0 --service-cidr=10.1.0.0/16 --pod-network-cidr=10.2.0.0/16 --ignore-preflight-errors=NumCPU
</code>
</pre>
为kubectl准备Kubeconfig文件
<pre>
<code>
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
</code>
</pre>
部署网络插件canal
<pre>
<code>
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/rbac.yaml
kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/canal.yaml
</code>
</pre>

# 部署Node节点
加入集群（Master节点初始化成功后输出如下）
<pre>
<code>
kubeadm join yourIP:6443 --token b6a7mm.lhsec1zstpv4b9o6 \
    --discovery-token-ca-cert-hash sha256:cbcc28444595bd712b4a638e52861e6b0c12d089f1a8f9086d95802c1985fb27
</code>
</pre>
添加标签
<pre>
<code>
kubectl label nodes yourNodeName node-role.kubernetes.io/node=
</code>
</pre>

# 查看部署结果
<pre>
<code>
kubectl get cs
kubectl get node
kubectl get pods --all-namespaces
</code>
</pre>

[m.unixhot.com](http://m.unixhot.com/kubernetes/kubernetes-aliyun.html)  
[www.kubernetes.org.cn](https://www.kubernetes.org.cn/5462.html)  
[connection to the server localhost:8080 was refused...](https://www.jianshu.com/p/6fa06b9bbf6a)  
<pre>
<code>
echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> ~/.bash_profile
source ~/.bash_profile
</code>
</pre>
