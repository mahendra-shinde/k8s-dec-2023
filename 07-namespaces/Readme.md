# Namespaces

1. Logical grouping for workload items like: Pods, ConfigMaps, Secrets, Deployments, Services, ReplicaSet ...
1. Enforce `RBAC` 
1. Enforce Resource Quota


## Demo

```
$ kubectl create ns mahendra
$ kubectl apply -f 02-resourcequota.yml
$ kubectl describe ns mahendra
```

## Test the resource quota
```bash
$ kubectl apply -f 03-deploy.yml
$ kubectl get po -n mahendra
```
