# Issues of Kubernetes

1. Swap off
```sh
$ sudo swapoff -a
```

2. kubelet.service holdoff time over, scheduling restart (fail to start up)
```sh
$ rm /etc/fstab
```

