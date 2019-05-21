# Kubernetes Install

### Setup K-master
Task will be installed
- API Server  – It provides kubernetes API using Jason / Yaml over http, states of API objects are stored in etcd
- Scheduler  – It is a program on master node which performs the scheduling tasks like launching containers in worker nodes based on resource availability
- Controller Manager – Main Job of Controller manager is to monitor replication controllers and create pods to maintain desired state.
- etcd – It is a Key value pair data base. It stores configuration data of cluster and cluster state.
- Kubectl utility – It is a command line utility which connects to API Server on port 6443. It is used by administrators to create pods, services etc.

Step 1: Disable SELinux & setup firewall rules
Login to your kubernetes master node and set the hostname and disable selinux using following commands

```sh
$ hostnamectl set-hostname 'k8s-master'
$ exec bash
$ setenforce 0
$ sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
```

Set the following firewall rules.
```sh
$ sudo firewall-cmd --permanent --add-port=6443/tcp
$ sudo firewall-cmd --permanent --add-port=2379-2380/tcp
$ sudo firewall-cmd --permanent --add-port=10250/tcp
$ sudo firewall-cmd --permanent --add-port=10251/tcp
$ sudo firewall-cmd --permanent --add-port=10252/tcp
$ sudo firewall-cmd --permanent --add-port=10255/tcp
$ sudo firewall-cmd --reload
$ sudo modprobe br_netfilter
$ sudo echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

In case you don’t have your own dns server then update /etc/hosts file on master and worker nodes
```sh
$ 192.168.1.30 k8s-master
$ 192.168.1.40 worker-node1
$ 192.168.1.50 worker-node2
```

Step 2: Configure Kubernetes Repository
Kubernetes packages are not available in the default CentOS 7 & RHEL 7 repositories, Use below command to configure its package repositories.

```sh
$ sudo vi /etc/yum.repos.d/kubernetes.repo

[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=0
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg

```

Step 3: Install Kubeadm and Docker

Once the package repositories are configured, run the beneath command to install kubeadm and docker packages.
```sh
$ sudo yum install kubeadm docker -y
```
Start and enable kubectl and docker service

```sh
$ systemctl restart docker && systemctl enable docker
$ systemctl restart kubelet && systemctl enable kubelet
```
Step 4: Initialize Kubernetes Master with ‘kubeadm init’
Run the beneath command to  initialize and setup kubernetes master.

```sh
$ kubeadm init
```

If error swap:
```sh
$ sudo swapoff -a
```

As we can see in the output that kubernetes master has been initialized successfully. Execute the beneath commands to use the cluster as root user.

OUTPUT: 
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

```sh
$ mkdir -p $HOME/.kube
$ cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ chown $(id -u):$(id -g) $HOME/.kube/config
```

Step 5: Deploy pod network to the cluster
```sh
$ kubectl get nodes
```

To make the cluster status ready and kube-dns status running, deploy the pod network so that containers of different host communicated each other.  
POD network is the overlay network between the worker nodes.

```sh
$ export kubever=$(kubectl version | base64 | tr -d '\n')
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
```

### Setup POD
Step 1: Disable SELinux & configure firewall rules on both the nodes
```sh
$ setenforce 0
$ sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
$ firewall-cmd --permanent --add-port=10250/tcp
$ firewall-cmd --permanent --add-port=10255/tcp
$ firewall-cmd --permanent --add-port=30000-32767/tcp
$ firewall-cmd --permanent --add-port=6783/tcp
$ firewall-cmd  --reload
$ echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

Step 2: Configure Kubernetes Repositories on both worker nodes
```sh
$ sudo vi /etc/yum.repos.d/kubernetes.repo

[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=0
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
```

Step 3: Install kubeadm and docker package on both nodes
```sh
$ sudo yum  install kubeadm docker -y
```

Start and enable docker service
```sh
$ systemctl restart docker && systemctl enable docker
$ systemctl restart kubelet && systemctl enable kubelet
```

Step 4: Now Join worker nodes to master node

```sh
$ kubeadm join --token a3bd48.1bc42347c3b35851 192.168.1.30:6443
```

