# To initialize and work on kubernetes
## **under master**
- kubeadm init --apiserver-advertise-address=172.31.42.131 --pod-network-cidr=192.168.0.0/16
>here the 172.31.42.131 ip is the private ip of of the master ec2 instance not the public

>> **copy the join id generated in a notepad**
kubeadm join 172.31.42.131:6443 --token urqlfe.2ptk4c3ykxdekcyo \--discovery-token-ca-cert-hash sha256:868aca04335d89c1af7c92b6ec455d4b981e74b60a10291a


- mkdir /home/ajay/.kube
- cp -i /etc/kubernetes/admin.conf /home/ajay/.kube/config
- chown -R ajay:ajay /  home/ajay


**to check the config file**

- ls -ltr /home/ajay/.kube/
- more /etc/kubernetes/admin.conf

**create a user ajay in master**
- useradd ajay
- passwd ajay
- su - ajay

**to configure pod calico network**

- kubectl create -f https://docs.projectcalico.org/v3.9/manifests/calico.yaml

**to create join token**
- kubeadm token create --print-join-command         

> fire the join token command in the worker nodes
