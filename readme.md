# ArgoCD

## For docker-desktop

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml


## And that is the fun to implement argocd

kubectl create namespace argocd
kubectl apply -f argocd/configmap.yaml
kubectl apply -n argocd -f ./argocd/install.yaml

Sometimes it seems you are to quick so you need to do:

kubectl rollout restart deployment argocd-server -n argocd

### SSl
openssl genrsa -out tls.key 2048
openssl req -new -key tls.key -out tls.csr -subj "/CN=argocd.peter-laudel.com"
openssl x509 -req -in tls.csr -signkey tls.key -out tls.crt -days 365
kubectl create secret tls argocd-tls-secret --cert=tls.crt --key=tls.key --namespace argocd

## Ingress

kubectl apply -f ./argocd/ingress.yaml
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo


## Apps-from-apps

Copy the values-yaml to argocd "create app".