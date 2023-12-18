# ConfigMap

- Object for storing `App Configuration` in `Plain Text` format
- Config Data is stored in Key/Value pairs
- Can be injected in multiple pods/containers in same namespace.

## Sample ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: dbconfig
data:
  MYSQL_USER: mahendra
  MYSQL_PASSWORD: Pass!word123
  MYSQL_DATABASE: book_db
  MYSQL_ROOT_PASSWORD: Pass1234word

```

## How to use configmap reference in POD ?

```yaml
  containers:
  - name: myapp
    image: nginx:alpine
    envFrom:
    - configMapRef:
        name: dbconfig
        #optional: true
```

## ConfigMap Commands

```bash
$ kubectl apply -f 01-config.yml
$ kubectl describe cm dbconfig
```

## ConfigMap as Volumes

- Application configuration can be mounted as volume inside container

```bash
$ kubectl apply -f 05-config.yml
$ kubectl describe cm appsettings-demo
$ kubectl apply -f 04-pod.yml
$ kubectl exec -it app4 -- sh
$ cat /vars/appsettings.k8s.json
$ exit
```