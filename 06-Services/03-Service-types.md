# Service Types

## Role of `kube-proxy`

1. Kube proxy is a mandatory component for all nodes in cluster.
2. Kube-proxy does `routing` request to the `pods`


## Role of DNS (core-dns)

1. Assign `domain-name` to each `service` 
1. Translate `IP` Addresses to `domain-name` and `domain-names` to `IP addresses`

## Demo 1 : ClusterIP Service

```bash
# Deploy the Application and Service
$ kubectl apply -f .\04-app.yml
# Check the pods and endpoints
$ kubectl get po -l app=app8 -o wide
$ kubectl describe ep app
# Get the service ip
$ kubectl get svc app
# Lets TEST the service
# Create a TEST pod
$ kubectl run test --image=nginx:alpine --port=80
# Get INSIDE the test pod
$ kubectl exec -it test -- sh
# Inside TEST pod, try accessing service THREE times
$ curl http://app
# Try again but with IP address of service instead "app"
$ curl http:[SERVICE-IP]
```

> Notice `Internal Load Balancing`, every request made by `curl` is handled by a different `pod`.