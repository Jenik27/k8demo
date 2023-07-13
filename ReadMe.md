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

# argocd notifications

1. install notifications and setup a bot with its token
2. create a secret and apply it to the argocd namespace

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: argocd-notifications-secret
stringData:
  slack-token: <auth-token>
```

3. put
   data:
   service.slack: |
   token: $slack-token
   to your config

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
    targetRevision: main
```

    - name: Authorization
      value: Bearer $webex-token

trigger.on-sync-succeeded: | - description: Application syncing has succeeded
send: - app-sync-succeeded
when: app.status.operationState.phase in ['Succeeded']
