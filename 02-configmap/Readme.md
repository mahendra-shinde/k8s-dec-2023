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

## ConfigMap Commands

```bash
$ kubectl apply -f 01-config.yml
$ kubectl describe cm dbconfig
```