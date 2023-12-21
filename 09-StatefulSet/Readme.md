# Stateful Set

- Designed for Stateful applications where, application need to maintain user sessions.
- Provides `Ordered` pod creation and deletion
    - Pods are created in incremental order : 0->1->2 .... N
    - Pods are deleted in reverse order : 3 -> 2 -> 1 -> 0
- Requires a Headless Service (ClusterIP service without IP address)
- Each pod uses name of Statefulset as prefix and index as suffix
    Example: web-0, web-1 
- Uses `PersistenceVolumeClaimTemplate` which generates `new` volume claim for each new pod

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
  namespace: mahendra
spec:
  selector:
    matchLabels:
      app: app8
  serviceName: web-service
  replicas: 3
  template:
    metadata:
      labels:
        app: app8
    spec:
      containers:
      - name: myapp
        image: nginx:alpine
        resources:
          limits:
            cpu: '200m'
            memory: '64Mi'
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      storageClassName: azurefile
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
---
apiVersion: v1
kind: Service
metadata:
  name: web-service
  namespace: mahendra
spec:
  type: ClusterIP
  clusterIP: None
  selector:
    app: app8
  ports:
  - port: 80        
    targetPort: 80 # Port Exposed by POD
```


# Demo

```bash
$ kubectl apply -f 01-sts.yml
$ kubectl get po -n mahendra pod -w
# CTRL+C
$ kubectl get pvc -n mahendra
$ kubectl scale sts web -n mahendra --replicas=1
$ kubectl get po -n mahendra pod -w
# CTRL+C
$ kubectl scale sts web -n mahendra --replicas=3
$ kubectl get po -n mahendra pod -w
# CTRL+C
```