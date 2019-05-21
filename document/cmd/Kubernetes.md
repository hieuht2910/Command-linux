# Kubernetes Cmd

Start and enable kubectl and docker service
```sh
$ systemctl restart docker && systemctl enable docker
$ systemctl restart kubelet && systemctl enable kubelet
```

Get nodes
```sh
$ kubectl get nodes

$ kubectl  get pods  --all-namespaces
```

Describe nodes
```sh
$ kubectl describe nodes
```

Check kubelet log
```sh
$ journalctl -u kubelet
```
