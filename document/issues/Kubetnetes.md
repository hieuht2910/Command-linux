# Issues of Kubernetes

1. Swap off
```sh
$ sudo swapoff -a
```

2. kubelet.service holdoff time over, scheduling restart (fail to start up)
```sh
$ rm /etc/fstab
```

3. Unable to connect to the server: x509: certificate signed by unknown authority 
(possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
```sh
$ export KUBECONFIG=/etc/kubernetes/admin.conf
```

4. On single cluster k8s, if see error pod Pending because 
'0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.', 

run
```sh
$ kubectl taint nodes --all node-role.kubernetes.io/master-
```
