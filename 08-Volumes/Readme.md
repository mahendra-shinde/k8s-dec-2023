# Volumes

- Persistent Volumes help to retain DATA even after POD recreation.
- Kubernetes support TWO methods for managing/provisioning volumes
    1. Static 
    1. Dynamic

# Static Provisioning

1. You must provision/create the backend storage first.
2. Map this storage with the Volume for pod.

# Dynamic Provisioning

1. We do not initialize / provision the backend storage
1. We expect kubernetes ( storage-provisioner) to initialize the backend storage for the volume.
    
    TWO Binding Strategies

    * IMMEDIATE_BINDING
    * FIRST_CONSUMER

1. We also expect storage-provisioner to perform clean !
    
    Three "Retention" strategies

    - RETAIN:  Deleting a volume will NOT delete the backend storage

    - RECYCLE: Deleting a volume will RESET or FORMAT backend storage, make it available for another new volume.

    - DELETE: Delete the backend storage when Volume is deleted.   
