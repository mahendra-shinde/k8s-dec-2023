# Kubernetes `Secrets`

- Unlike `ConfigMap` secrets internally uses `Base64` encoding to store all config values
- Three kinds of Secrets
    1. Generic Secret :     Like `ConfigMap`, used for Environment variables
    1. TLS Secret :         Used for storing `Digital Certificates`
    1. docker-registry :    Store container registry credentials


## Secrets from Command line

```bash
$ kubectl create secret generic mysecrets --from-literal=uname=mahendra --from-literal=upass=pass1235499
$ kubectl describe secret mysecrets
### Instead of Values, k8s shows number of bytes for each KEY
$ kubectl get secret mysecrets -o yaml
## Now, k8s will share the encoded values
```

## Inject as Environment Variables

### POD 
```yaml
    envFrom:
    - secretRef: 
        name: mysecrets
```

```bash
$ kubectl apply -f 01-pod.yml
$ kubectl exec -it app5 -- sh
$ echo "$uname $upass"
$ exit
```

## Using Private Container Registries

```bash
# Create `registry secret`
$ kubectl create secret docker-registry myregistry --docker-username=mahendrashinde --docker-password=DGlUo/uisEbm8+w4bJBeOkGcvHD9/rGuBDcjozPgD1+ACRDphkSZ --docker-server=mahendrashinde.azurecr.io

$ kubectl apply -f 02-pod-private-image.yml
$ kubectl get po
$ kubectl describe po app6
```