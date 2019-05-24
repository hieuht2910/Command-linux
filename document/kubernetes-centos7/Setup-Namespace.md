# Setup Namespace

Get list current namespaces
```sh
$ kubectl get namespaces
```

Create a new namespaces
Cretae a new YAML file: my-namespace.yaml
```sh
apiVersion: v1
kind: Namespace
metadata:
  name: <insert-namespace-name-here>
```

Then run
```sh
$ kubectl create -f ./my-namespace.yaml
```