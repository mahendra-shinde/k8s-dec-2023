# Helm : Kubernetes Package Manager

- An OSS which provide `Package Management` for Kubernetes cluster
- Lots of Community "Helm" Registries
- Use of Helm
    1. Package and Deploy your workload on cluster
    2. Install pre-packaged applications or extensions on cluster

- There is search engine for Helm charts https://artifacthub.io
- Installation of Helm is lot easier


## Pre-requisites
1.  You must have Kubernetes cluster
1.  You must have KUBECONFIG file in its default location 
1.  You must have `kubectl` installed

## Installation

1. Visit https://github.com/helm/helm/releases/tag/v3.13.3 and choose your OS type `Windows AMD64`
1. After the download finishes, extract the "helm.exe" from inside the file to C:\helm folder
1. Add path C:\Helm to your Environment Variable 'PATH'
1. Launch a new terminal (CMD or Powershell) and test using command:
    `helm version`

## Demo 1: Download and Explore a `Helm Chart`

```bash
$ mkdir \helm-demos
$ cd \helm-demos
$ helm fetch --untar oci://registry-1.docker.io/bitnamicharts/mysql
$ cd mysql
$ code .
# Generate the Kubernetes Manifest from Helm Chart
$ helm template mysql . 
# Review the generated YAML manifest
```

## Installing the chart in new namespace

```bash
$ kubectl create ns mydata
$ helm install -n mydata db1 oci://registry-1.docker.io/bitnamicharts/mysql
$ kubectl get all -n mydata
# List all Helm charts installed inside namespace
$ helm list -n mydata
# UNinstall the chart and resources (Except PVC) from namespace
$ helm uninstall -n mydata db1
# Delete the Volume claim 
$ kubectl delete  -n mydata $(kubectl get pvc -n mydata -o name)
```

## Installing Nginx Ingress controller using Helm

```bash
$ kubectl create ns mydata
$ helm install -n mydata nginx  oci://registry-1.docker.io/bitnamicharts/nginx-ingress-controller
$ kubectl get all -n mydata
```

## Deploying a sample application with Three `deployments` and Three `ClusterIP services`

```bash
$ kubectl create ns mahendra
$ kubectl apply -n mahendra -f https://raw.githubusercontent.com/mahendra-shinde/kubernetes-demos/master/14-ingress/deployment.yml
$ kubectl get all -n mahendra
```

## Deploy the Routes [Ingress rules]

```
$ kubectl apply -n mahendra -f 
```


## Installing Monitoring stack

```
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts   
$ helm repo update
$ kubectl create ns monitor
$ helm install -n monitor  monitor prometheus-community/kube-prometheus-stack -f values.yml
```

## Using port-forwarding to access grafana

```bash
$ kubectl port-forward -n monitor svc/monitor-grafana 8080:80
```

> Open web browser and visit `http://localhost:8080`

> Use credentials : admin / Password@1234