# Deploy app to k8s cluster

1. start minikube

```
minikube start
```

2. create yaml files describing the deployment and service
3. apply files to k8s cluster
4. check if pods are running
5. get minikube ip

```bash
minikube ip
```

6. view it from the browser using the ip and port or log it by using

```bash
kubectl logs <pod-name>
```
