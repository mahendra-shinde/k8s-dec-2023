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