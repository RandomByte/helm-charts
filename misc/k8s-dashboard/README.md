### Install Dashboard
```sh
helm repo update
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --namespace kubernetes-dashboard --create-namespace
```

### Create a User
```sh
kubectl apply -f dashboard-adminuser.yaml
```

### Get Token
```sh
kubectl -n kubernetes-dashboard create token admin-user
```

### Access
```sh
kubectl proxy
```

Open http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:443/proxy/#/login
