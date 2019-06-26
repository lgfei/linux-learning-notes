http://m.unixhot.com/kubernetes/kubernetes-aliyun.html 
https://www.kubernetes.org.cn/5462.html
<pre>
<code>
cd /etc/yum.repos.d/
wget https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum -y install docker-ce-18.06.1.ce-3.el7
systemctl enable docker && systemctl start docker
docker --version
vim /etc/yum.repos.d/kubernetes.repo
yum install -y kubelet kubeadm kubectl
</code>
</pre>
