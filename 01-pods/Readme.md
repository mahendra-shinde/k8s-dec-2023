# Pods 

- Smallest deployable Unit which uses system resources
- Group of co-hosted containers, sharing same host, network, IP and volumes (storage)
- Atomic unit : either all the containers are deployed or NONE are deployed !

## Quality of Service (QoS)

Condition |  QoS     | Result | Example
----------|-----------|------|------
When BOTH "limits" and "requests" defined (and requests is LESSER than limits) | Burstable | Initial resources based on "requests" and Max limit based on "Limits" | [Burstable](./01-burstable.yml)
When only  Limits are defined | Gauranteed | Containers cant use more resources than LIMIT | [Gauranteed](./02-gauranteed.yml)
When neither "limits" nor "requests" for resources defined | BestEffor  | Container MIGHT consume all available resources on Node | [BestEffort](./03-besteffort.yml)


## Test

1. To deploy a pod use command  `kubectl create -f FILENAME`
2. To describe a pod use command `kubectl describe -f FILENAME`
3. To delete a pod use command `kubectl delete -f FILENAME`

> Replace the `FILENAME` with actual filename of pod


## Other Commands

1. View the container Logs

```
# Multi Container Pod
$ kubectl logs POD-NAME -c CONTAINER_NAME

# Single container pod
$ kubectl logs POD-NAME
```

1. Using EXEC to debug containers

```
# Multi Container Pod
$ kubectl exec -it POD-NAME -c CONTAINER_NAME -- SH

# Single container pod
$ kubectl exec -it POD-NAME  -- SH
```

## Multi Container Pod

Refer to [Demo](./04-multi-container.yml)

## Init Containers

- Small containers to perform initialization and till then all the main containers would be kept UNAVAILABLE to rest of cluster.

- [Demo](./05-init-container.yml)

### Commands :

```
$ kubectl create -f 05-init-container.yml 
# Keep WATCHING pod status change
$ kubectl get po pod2 -w
## PRESS CTRL+C to stop WATCHING
```