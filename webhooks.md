# webex webhook config

## setup argocd notification

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl get pods -n argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/notifications_catalog/install.yaml

## create secret file and apply it

vi webhook-secret.yaml

apiVersion: v1
kind: Secret
metadata:
name: argocd-notifications-secret
stringData:
webhook-token: xoxb-5567118602466-5580425657345-QLuc6a3iAvka9YNPy0OSVPed

kubectl apply -f webhook-secret.yaml -n argocd

## access argo UI

kubectl get svc -n argocd

kubectl port-forward -n argocd svc/argocd-server 8080:443

### power shell pws

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($\_)) }

## create apps

kubectl apply -f app.yaml -n argocd

(or in UI or VScode kubernetes plugin)

## edit argocd-notofications-cm configmap

kubectl edit cm/argocd-notifications-cm -n argocd

service.webhook.notif-hook: |
url: https://webexapis.com/v1/webhooks/incoming/Y2lzY29zcGFyazovL3VzL1dFQkhPT0svYjM4NjVhMWQtOGE2OC00OTQzLWE0ZjUtZmYxNjlkMTEyZTAw
headers: - name: Content-Type
value: application/json - name: Authorization
value: Bearer MjYxMTE5MGMtNjUxNS00ZjA1LTkxMWQtZjlhMTU5Njk5NTQ2NmViYjdlODQtZDg4_PF84_c096eb54-
template.custom-template: |
webhook:
notif-hook:
method: POST
body: |
{
"markdown": "### Application sync success!"
}
trigger.custom-trigger: | - when: app.status.operationState.phase in ['Succeeded']
send: [custom-template]

## add annotations to app (or? )

metadata:
name: auto-sync-app
namespace: argocd
annotations:
notifications.argoproj.io/subscribe.custom-trigger.notif-hook: ""

kubectl apply -f app.yaml -n argocd
kubectl get application -n argocd
