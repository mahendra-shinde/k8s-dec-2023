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


## Node port

- Service is exposed on `All Nodes` on unqiue port number `Node-Port`
- NodePort can be in range 30000-39999
- Every request to `node-port` service is then `forwared` to cluster-ip and then to individual pod.


## Example of NodePort service

1. Modify the `04-app.yml` file. replace the `Service definition` with following lines:


<table>
<tr>
<th>Old `ClusterIP` Service</th>
<th>New `NodePort` Service</th>
</tr>
<tr>
<td>

```yaml
kind: Service
metadata:
  name: app
spec:
  type: ClusterIP
  selector:
    app: app8
  ports:
  - targetPort: 80
    port: 80
```
</td>
<td>

```yaml
kind: Service
metadata:
  name: app
spec:
  type: NodePort
  selector:
    app: app8
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30001
```
</td>
</tr>
</table>

1. Delete the old service and re-apply the YAML manifest

```bash
$ kubectl delete svc app
$ kubectl apply -f 04-app.yml
$ kubectl get svc app
```

1. Test the service using following URL

    `http://kubernetes.docker.internal:30001`


## Load Balancer Service

1. LoadBalancer service is extension of `NodePort` service.
1. LoadBalancer service is `Supported` by kubernetes but `No Default Implementation` available from kubernetes.
1.  `Docker Desktop` uses a custom load-balancer implementation which will use `localhost` as external-ip for every load-balancer type service
1.  On `Managed Kubernetes cluster` like `AKS`, `EKS` or `GKE` the load-balancer implementation is provided by the respective cloud vendor

  - Azure Kubernetes Service (AKS) uses `Azure Load Balancer`
  - Elastic Kubernetes Service (EKS) uses `Elastic Load Balancer`

## Load Balancer Service Demo

1. Modify the `04-app.yml` file. replace the `Service definition` with following lines:


<table>
<tr>
<th>Old `ClusterIP` Service</th>
<th>Old `NodePort` Service</th>
<th>New `Load Balancer` Service</th>
</tr>
<tr>
<td>

```yaml
kind: Service
metadata:
  name: app
spec:
  type: ClusterIP
  selector:
    app: app8
  ports:
  - targetPort: 80
    port: 80
```
</td>
<td>

```yaml
kind: Service
metadata:
  name: app
spec:
  type: NodePort
  selector:
    app: app8
  ports:
  - targetPort: 80
    port: 80
    nodePort: 30001
```
</td>
<td>

```yaml
apiVersion: v1
kind: Service
metadata:
  name: app
spec:
  # ClusterIP is default type
  type: LoadBalancer
  selector:
    app: app8
  ports:
  - targetPort: 80
    port: 8080  # Set the port to 8080
    # Let k8s use random node-port
```
</td>
</tr>
</table>

1. Delete the old service and re-apply the YAML manifest

```bash
$ kubectl delete svc app
$ kubectl apply -f 04-app.yml
$ kubectl get svc app
```

1. Test the service using following URL

    `http://localhost:8080`

