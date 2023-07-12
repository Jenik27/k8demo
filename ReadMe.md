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

# Application

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: notifapp
  namespace: argocd
  annotations:
    notifications.argoproj.io/subscribe.on-sync-succeeded.slack: argocd-channel
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    path: .
    repoURL: https://github.com/Jenik27/k8demo.git
    targetRevison: main
```
