# Deployment Object
 
- Kubernetes recommends creating `Deployment` instead of `ReplicaSet`
- Deployment created `ReplicaSet` on deployment
- Every Deployment has ONE `Current` ReplicaSet with Non-Zero replica count
- Every Deployment will have multiple `Old Replicas` with Zero replica count
- Group of replicas is called `Revision History`
- Default Revision History limit is `10`
- Kubernetes manage all the deployments using `deployment-controller`

## Example of Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app2
spec:
  replicas: 3
  revisionHistoryLimit: 4
  selector:
    matchLabels:
      app: app2
  template:
    metadata:
      labels:
        app: app2
    spec:
      containers:
      - name: myapp
        image: mahendrshinde/myweb:1
        resources:
          limits:
            memory: "32Mi"
            cpu: "100m"
        ports:
        - containerPort: 80
```

## Commands

```bash
$ kubectl apply -f 01-deploy.yml
$ kubectl get deploy app2
$ kubectl describe deploy app2
$ kubectl get rs -l app=app2
$ kubectl rollout history
## Update/deploy NEW version of application
$ kubectl set image deploy/app2 myapp=mahendrshinde/myweb:2 --record
$ kubectl get rs -l app=app2 -w
## Expected : Two replica-sets 
## Update/deploy NEW version of application
$ kubectl set image deploy/app2 myapp=mahendrshinde/myweb:3 --record
$ kubectl get rs -l app=app2 -w
## Expected : Thress replica-sets 
$ kubectl rollout history deploy/app2
## Undo to last version (2)
$ kubectl rollout undo deploy/app2
$ kubectl rollout history deploy/app2
## Undo to last version (3)
$ kubectl rollout undo deploy/app2
$ kubectl rollout history deploy/app2
## Delete the deployment
$ kubectl delete -f 01-deploy.yml
```


## Update Strategy

- Kubernetes provide TWO strategies for Application updates
    1. Rolling Update [Default]
    1. Recreate


Sr | Rolling Update | Recreate
---|------------|----------------
1 | New Pods are deployed First | All of old pods are deleted first
2 | One old pod is replace with One new pod | All the pods are replaced at once which results in downtime
3 | While `Rolling Updates` are being processed, cluster need more resources to accomodate both OLD and NEW pods | Cluster do not need any additional resources.
4 | Use for `incremental change` or `Non breaking change` | When change can `break` certain functionality or is not compatible with old version

## Example:

> Rolling Update configuration

```yaml
  strategy:
    rollingUpdate:
      maxSurge: 2
      maxUnavailable: 2
```

> Recreate configuration

```yaml
  strategy:
    type: Recreate
```


