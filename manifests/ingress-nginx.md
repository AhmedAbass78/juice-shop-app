For deploying ingress-nginx file use the following command:

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml
```
And then delete the validatingwebhookconfiguration:

```
kubectl delete validatingwebhookconfiguration ingress-nginx-admission
```
