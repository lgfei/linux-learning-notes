# 1
cd /etc/yum.repos.d/
# 2
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 3
yum list docker-ce.x86_64 --showduplicates | sort -r
# 4
yum -y install docker-ce-18.06.1.ce-3.el7
# 5
systemctl enable docker && systemctl start docker
# 6
docker --version
# 7
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
# 8
yum install -y kubelet kubeadm kubectl
# 9
vim /etc/sysconfig/kubelet
<pre>
<code>
KUBELET_EXTRA_ARGS="--fail-swap-on=false"
</code>
</pre>
# 10
<pre>
<code>
cat <<EOF > /etc/sysctl.d/k8s.conf

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1

EOF
</code>
</pre>
# 11
sysctl --system
# 12
systemctl enable kubelet && systemctl start kubelet

# Master节点
<pre>
<code>
kubeadm init --apiserver-advertise-address=172.18.61.63 --image-repository registry.aliyuncs.com/google_containers --kubernetes-version v1.15.0 --service-cidr=10.1.0.0/16 --pod-network-cidr=10.2.0.0/16 --ignore-preflight-errors=NumCPU
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
</code>
</pre>


[m.unixhot.com](http://m.unixhot.com/kubernetes/kubernetes-aliyun.html)  
[www.kubernetes.org.cn](https://www.kubernetes.org.cn/5462.html)  
[connection to the server localhost:8080 was refused...](https://www.jianshu.com/p/6fa06b9bbf6a)  
