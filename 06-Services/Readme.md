# Services 

## Challenge with Pods

1. Pods are Unreliable !
2. IP Based communication is Unreliable (ref 1)
3. Communication between microservices "need" stable endpoint


> Kubernetes TWO controllers to provide service discovery


1. Endpoint controller

	- Identify the "Target" pods for the incoming traffic
	- Target pods are grouped in
		1. Ready Endpoints
		2. NotReady Endpoints
	- Use "Health Probe" 

2. Service Controller
	- Traffic movement !!
	- Accept the incoming request and forwad it to ONE OF THE AVAILABLE endpoints
	- Internal Load Balancing / Load Distribution


## Probes

- Verify if target pod is available to handle request.
- Endpoint controller is responsible to `flag` target pod as `Not Ready` if the probe fails
- Three kind of probes:

    1. Startup probe:   Put all other probes `on hold` till success.
    2. Liveness probe:  Restart the pod (recreate containers) when probe fails.
    3. Readiness probe: Flag the pod as unavailable, but no restart.

## Probe methods

- The actual method for probe (test), can be used in all three probe types : startup, liveness and readiness.

1.  HTTP Probe:

    - Make a HTTP request and expect SUCCESS (200), Fails on ERROR (400 - 599)
    - Ideal for liveness probes

    Syntax:

    ```yaml
    livenessProbe:
      httpGet:
            path: /
            scheme: HTTP
            port: 80
          failureThreshold: 2
          periodSeconds: 10  # Every 10 seconds, repeat the probe
          timeoutSeconds: 5 # Expect Response in 5 seconds
    ```

1.  TCP probe:

    Send few bytes (packet) of data to target IP:PORT and expect it to bounce back.
    
    ```yaml
    livenessProbe:
      tcpSocket:
        port: 80
      failureThreshold: 2
      timeoutSeconds: 5
      periodSeconds: 10
    ```

1.  Command probe:

    - Run a any command which should result in SUCCESS (ERRORLEVEL 0)
    - Ideal for startup probes

    ```yaml
    startupProbe:
      exec:
        command:
        - ls -l /var/nginx/access.log
      failureThreshold: 5
      periodSeconds: 10
      timeoutSeconds: 2
    ```

## Test the liveness probes

```
$ kubectl apply -f 01-deploy.yml
$ kubectl apply -f 02-service.yml
# List all the pods 
$ kubectl get po -l app=app7
# List all the endpoints
$ kubectl get ep web
$ kubectl exec -it PODNAME -- sh
$ cd /usr/share/nginx/html
$ mv index.html index.php
$ exit
## Check the endpoints and pods
$ kubectl get ep,pod -l app=app7
# Check the pod logs
$ kubectl log PODNAME 
```