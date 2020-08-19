# Installing kubernetes
**prerequisite for kubernetes installation**


1. create master and worker nodes environment
2. disable SElinux and SWAP on all nodes
3. install kubeadm, kubectl, kubelet, docker on all nodes
4. initialize the master node (kubeinit)
5. configure pod network(calico)
6. join worker nodes to the cluster(kubeadm join)

### Do this in all the machines including worker and master

**installing docker**
- yum update
- yum install -y -q yum-utils device-mapper-persistent-data lvm2 > /dev/null 2>&1
- yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo > /dev/null 2>&1
- yum install -y -q docker-ce >/dev/null 2>&1
- systemctl enable docker
- systemctl start docker

**Disable SELinux**

- setenforce 0
- sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux

**Disable swap**

- sed -i '/swap/d' /etc/fstab
- swapoff -a


**Disable Firewall**

- systemctl disable firewalld
- systemctl stop firewalld

**To fix  issue where traffic is routed incorrectly due to ip table bypass(only on RHEL/CentOS)**

- cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

- sysctl --system



**Kubernetes Setup**
**Add yum repository**

- cat >>/etc/yum.repos.d/kubernetes.repo<<EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

**Install Kubeadm, kubectl, kubelet**

- yum install -y kubeadm-1.15.6-0.x86_64 kubelet-1.15.6-0.x86_64 kubectl-1.15.6-0.x86_64

**Enable and Start kubelet service**

- systemctl enable kubelet
- systemctl start kubelet

**To check if kubelet is installed and running**
-systemctl status kubelet

>Enable inbound rules for all traffic in ec2 machines
