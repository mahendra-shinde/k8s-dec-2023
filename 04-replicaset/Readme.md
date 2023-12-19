# Kubernetes ReplicaSet

- Parent object for Pods
- Designed for `Stateless` applications.
- Provides `Self Healing` (Any child-pod terminated due to any reason would be replaced with new one)
- Provides `Scalability` [ Either updating YAML manifest or using `kubectl scale` command ]
- The Order of pod creation/deletion is `Unpredictable`

> How `ReplicaSet-controller` knows Which pods belong to which replicaset ?

> Pods have `labels` and ReplicaSet have `Label Selector` both should match.

## Replica-Set demo

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: app1
spec:
## Define the "Labels" on Child Pods
  selector:
    matchLabels:
      set: app1
  # Desired replica count
  replicas: 3
  # Pods shall receive traffic only after 5 seconds [ Optional ]
  minReadySeconds: 5
  template:
    metadata:
      labels:
        set: app1 ## MUST MATCH WITH line# 9
    spec:
      containers:
      - name: web
        image: mahendrshinde/myweb:1
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: '150m'
            memory: '64Mi'
```

## Commands

```bash
# Deploy replicaset from YAML manifest 
$ kubectl apply -f 01-replicaset.yml
# List the details of ReplicaSet
$ kubectl get rs app1 -o wide
# Get all the pods
$ kubectl get pods
# Scale the replicaset UP to 5 replicas from original 3
$ kubectl scale rs/app1 --replicas=5
# Check the changes in pods (-w to watch for changes)
$ kubectl get pods -w 
## Press CTRL+C to stop watching
# Scale the replicaset DOWN to 1 replicas from earlier 5
$ kubectl scale rs/app1 --replicas=1
# Check the changes in ReplicaSet (-w to watch for changes)
$ kubectl get rs -w 
## Press CTRL+C to stop watching
## Delete all the pods [ CAUTION: Also deletes non-member pods !!! ]
$ kubectl delete po --all
# Check the changes in ReplicaSet (-w to watch for changes)
$ kubectl get rs -w 
## Press CTRL+C to stop watching
# Delete the replicaset
$ kubectl delete rs app1

```