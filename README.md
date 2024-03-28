

# Kubernetes hackathon part-2

## Prerequisites
- Kubectl installation
	- https://kubernetes.io/docs/tasks/tools/
- Minikube installation
	- https://minikube.sigs.k8s.io/docs/start/
- Start the cluster using 
	 ```minikube start```
- Ensure minikube cluster context is set in **kubectl** command


## Task 1
Create a namespace `my-namespace` in your cluster

## Task 2
Create a Pod with name `nginx` with images `nginx:latest`, `grafana/grafana-oss` in `my-namespace` using yaml configurations.

Create a YAML for a Pod with multiple containers with container names `first`, `second` for the given images


Copy the YAML content below
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: my-namespace
spec:
  containers:
  - name: first
    image: nginx:latest
  - name: second
    image: nginx:1.14.2



```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
NAME    READY   STATUS              RESTARTS   AGE
nginx   0/2     ContainerCreating   0          4s
```

## Task 3

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs

get the logs for container `first` in `nginx` pod

```
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/28 08:28:38 [notice] 1#1: using the "epoll" event method
2024/03/28 08:28:38 [notice] 1#1: nginx/1.25.4
2024/03/28 08:28:38 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2024/03/28 08:28:38 [notice] 1#1: OS: Linux 5.15.146.1-microsoft-standard-WSL2
2024/03/28 08:28:38 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/03/28 08:28:38 [notice] 1#1: start worker processes
2024/03/28 08:28:38 [notice] 1#1: start worker process 29
2024/03/28 08:28:38 [notice] 1#1: start worker process 30
2024/03/28 08:28:38 [notice] 1#1: start worker process 31
2024/03/28 08:28:38 [notice] 1#1: start worker process 32
2024/03/28 08:28:38 [notice] 1#1: start worker process 33
2024/03/28 08:28:38 [notice] 1#1: start worker process 34
2024/03/28 08:28:38 [notice] 1#1: start worker process 35
2024/03/28 08:28:38 [notice] 1#1: start worker process 36

```

get the logs for container `second` in `nginx` pod

```
2024/03/28 08:31:52 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
2024/03/28 08:31:52 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
2024/03/28 08:31:52 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
2024/03/28 08:31:52 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
2024/03/28 08:31:52 [emerg] 1#1: bind() to 0.0.0.0:80 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
2024/03/28 08:31:52 [emerg] 1#1: still could not bind()
nginx: [emerg] still could not bind()
```

## Task 4
Delete the nginx pod


## Task 5
Create a replicaset named `nginx-replicaset` for image `nginx:1.42.6` with 3 replicas in the namespace `my-namespace` using YAML configurations

1. Copy the YAML content below
	```
	apiVersion: apps/v1
	kind: ReplicaSet
	metadata:
	  name: nginx-replicaset
	  namespace: my-namespace
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: nginx
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx
	        image: nginx:1.42.6

	```

2. Apply the yaml configurations using `kubectl apply -f <yaml file>`

3. List the replicaset in the namespace `my-namespace` using `kubectl get replicaset -n <namespace>`
	```
	NAME               DESIRED   CURRENT   READY   AGE
	nginx-replicaset   3         3         0       10s

	```

4. List the pods in the namespace `my-namespace` 
	```
	NAME                     READY   STATUS              RESTARTS   AGE
	nginx-replicaset-6n2cb   0/1     ContainerCreating   0          10s
	nginx-replicaset-8jr4s   0/1     ErrImagePull        0          10s
	nginx-replicaset-z5k5b   0/1     ContainerCreating   0          10s
 
	```

5. Find out why the pods are failing in the `nginx-replicaset` replicaset 
	```
	Troubleshooting command : `kubectl describe pods -n my-namespace`

	```

6. Fix the issue in the YAML configuration created  and re-apply configuration using `kubectl apply`

	List the pods in the namespace `my-namespace` 
	```
	NAME                     READY   STATUS             RESTARTS   AGE
	nginx-replicaset-6n2cb   0/1     ImagePullBackOff   0          41m
	nginx-replicaset-8jr4s   0/1     ImagePullBackOff   0          41m
	nginx-replicaset-z5k5b   0/1     ImagePullBackOff   0          41m

	``

7. Do a `kubectl delete -f <file-name>` and apply it again using `kubectl apply -f <file-name>`

	List the pods in the namespace `my-namespace` 
	```
	NAME                     READY   STATUS              RESTARTS   AGE
	nginx-replicaset-k2g4l   0/1     ContainerCreating   0          6s
	nginx-replicaset-p227q   0/1     ContainerCreating   0          6s
	nginx-replicaset-pcx5k   0/1     ContainerCreating   0          6s

	```

8. Explain your observations from steps 6,7

	```
	<Type observation here>

	```


9. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace`

 	Decrease by 1
	```
	NAME                     READY   STATUS              RESTARTS   AGE
	nginx-replicaset-k2g4l   0/1     ContainerCreating   0          57s
	nginx-replicaset-p227q   0/1     Terminating         0          57s
	nginx-replicaset-pcx5k   0/1     ContainerCreating   0          57s
	```

 	Increase by 1
	```
 	NAME                     READY   STATUS    RESTARTS   AGE
	nginx-replicaset-hnv2f   1/1     Running   0          3s
	nginx-replicaset-k2g4l   1/1     Running   0          94s
	nginx-replicaset-pcx5k   1/1     Running   0          94s
	nginx-replicaset-pvdtg   1/1     Running   0          3s
	```
11. Delete the `nginx-replicaset` using `kubectl delete replicaset/<replicaset-name> -n <namespace>`

## Task 6
Create a deployment named `nginx-deployment` for image `nginx:1.42.6` with 3 replicas in the namespace `my-namespace` using YAML configurations

1. Copy the YAML content below
	```
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: nginx-deployment
	  namespace: my-namespace
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: nginx
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx
	        image: nginx:1.42.6

	```

2. Apply the yaml configurations using `kubectl apply -f <yaml file>`

3. List the deployments in the namespace `my-namespace` using `kubectl get deployments -n <namespace>`
	```
	NAME               READY   UP-TO-DATE   AVAILABLE   AGE
	nginx-deployment   0/3     3            0           43s

	```
4. List the replicasets in the namespace `my-namespace` using `kubectl get replicaset -n <namespace>`
	```
	NAME                          DESIRED   CURRENT   READY   AGE
	nginx-deployment-7cf8b9f7c4   3         3         0       43s

	```

5. List the pods in the namespace `my-namespace` 
	```
	NAME                                READY   STATUS             RESTARTS   AGE
	nginx-deployment-7cf8b9f7c4-bdbtf   0/1     ImagePullBackOff   0          44s
	nginx-deployment-7cf8b9f7c4-l2986   0/1     ErrImagePull       0          44s
	nginx-deployment-7cf8b9f7c4-tk2xz   0/1     ImagePullBackOff   0          44s

	```

6. Change the image in YAML to `nginx:1.25.4` and re-apply the yaml using `kubectl apply -f <file-name>`

	```
	deployment.apps/nginx-deployment configured
	```
	List the pods in the namespace `my-namespace` 
	```
	NAME                                READY   STATUS    RESTARTS   AGE
	nginx-deployment-64c74c9f9c-4s55h   1/1     Running   0          10m
	nginx-deployment-64c74c9f9c-h8zgg   1/1     Running   0          10m
	nginx-deployment-64c74c9f9c-lbqst   1/1     Running   0          10m
	```
	  List the replicaset in the namespace `my-namespace` 
	```
	NAME                          DESIRED   CURRENT   READY   AGE
	nginx-deployment-64c74c9f9c   3         3         3       11m
	nginx-deployment-7cf8b9f7c4   0         0         0       14m
	```
7. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace` 
	```
	NAME                                READY   STATUS    RESTARTS   AGE
	nginx-deployment-64c74c9f9c-4s55h   1/1     Running   0          13m
	nginx-deployment-64c74c9f9c-h8zgg   1/1     Running   0          13m
	nginx-deployment-64c74c9f9c-lbqst   1/1     Running   0          13m
	nginx-deployment-64c74c9f9c-nxldp   1/1     Running   0          95s

	```

8. Compare the effort while updating the images in replicaset and deployment
	```
 	Updating the pod in deployment was faster than that in replicaset
	It didnt require any troubleshooting in deployment image.
 	All the pods were Running

	```
9.  Delete the deployment  `nginx-deployment` using kubectl delete command

## Bonus task

Apply the given deployment.yaml file using `kubectl apply -f`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: my-namespace
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 82
        livenessProbe:
          httpGet:
            path: /
            port: 82
```

1. List the pods for deployment
	```
	NAME                                READY   STATUS             RESTARTS       AGE
	nginx-deployment-7b6bf4cfdc-h9tv8   1/1     Running            0              21m
	nginx-deployment-fbd6c6f5f-jbbd6    0/1     CrashLoopBackOff   11 (31s ago)   20m
	```
2. Findout why the pods are not ready
	```
	Changed the readiness and liveness to different port
	
	```
3. Fix the issue and explain your understanding on  `readinessProbe` `livenessProbe`
	
	```
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: nginx-deployment
	  namespace: my-namespace
	spec:
	  replicas: 1
	  selector:
	    matchLabels:
	      app: nginx
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx
	        image: nginx:latest
	        ports:
	        - containerPort: 80
	        readinessProbe:
	          httpGet:
	            path: /
	            port: 81
	        livenessProbe:
	          httpGet:
	            path: /
	            port: 82


	```
	Understanding on readness and liveness probe
	```
	Readiness probe is to check if the container is ready to serve the traffic or not, while liveness probe is to check if the container is running or not. Both the probes check the container using HTTPS request or TCP socket checks or executing a command within the container. If readiness probe fails, container is removed from the service and if liveness probe fails, container is restarted. 

	```

## Cleanup task
Delete the namespace `my-namespace` 
