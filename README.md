

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
  namespace: my-namespaces
spec:
  restartPolicy: Never
  containers:
  - name: nginx
    image: nginx:latest
  - name: grafana
    image: grafana/grafana-oss

```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
C:\Users\SajoSam>kubectl get pods -n my-namespaces
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          32s
```

## Task 3

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs

get the logs for container `first` in `nginx` pod

```
PS E:\hackathon> kubectl logs nginx -c nginx -n my-namespaces
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/28 07:42:37 [notice] 1#1: using the "epoll" event method
2024/03/28 07:42:37 [notice] 1#1: nginx/1.25.4
2024/03/28 07:42:37 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/03/28 07:42:37 [notice] 1#1: OS: Linux 6.1.0-17-amd64
2024/03/28 07:42:37 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/03/28 07:42:37 [notice] 1#1: start worker processes
2024/03/28 07:42:37 [notice] 1#1: start worker process 29
2024/03/28 07:42:37 [notice] 1#1: start worker process 30

```

get the logs for container `second` in `nginx` pod

```
kubectl logs nginx -c grafana -n my-namespaces

```

## Task 4
Delete the nginx pod


## Task 5
Create a replicaset named `nginx-replicaset` for image `nginx:1.42.6` with 3 replicas in the namespace `my-namespace` using YAML configurations

1. Copy the YAML content below
	```
	<Paste output here>

	```

2. Apply the yaml configurations using `kubectl apply -f <yaml file>`

3. List the replicaset in the namespace `my-namespace` using `kubectl get replicaset -n <namespace>`
	```
	PS E:\hackathon> kubectl get replicaset -n my-namespaces
	NAME               DESIRED   CURRENT   READY   AGE
	nginx-replicaset   3         3         0       108s

	```

4. List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                     READY   STATUS             RESTARTS   AGE
	nginx-replicaset-9bh6p   0/1     ImagePullBackOff   0          2m12s
	nginx-replicaset-ptm2b   0/1     ImagePullBackOff   0          2m12s
	nginx-replicaset-xqkrn   0/1     ImagePullBackOff   0          2m12s

	```

5. Find out why the pods are failing in the `nginx-replicaset` replicaset 
	```
	1. The image could not be found
 	2. The specified image tag doesn't exist.

	```

6. Fix the issue in the YAML configuration created  and re-apply configuration using `kubectl apply`

	List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                     READY   STATUS             RESTARTS   AGE
	nginx-replicaset-9bh6p   0/1     ImagePullBackOff   0          5m46s
	nginx-replicaset-ptm2b   0/1     ImagePullBackOff   0          5m46s
	nginx-replicaset-xqkrn   0/1     ImagePullBackOff   0          5m46s

	``

7. Do a `kubectl delete -f <file-name>` and apply it again using `kubectl apply -f <file-name>`

	List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                     READY   STATUS    RESTARTS   AGE
	nginx-replicaset-4wtqf   1/1     Running   0          45s
	nginx-replicaset-bmjn8   1/1     Running   0          24s
	nginx-replicaset-z4xvn   1/1     Running   0          11s

	```

8. Explain your observations from steps 6,7

	```
	1. Want to delete the pod for re-apply the configuration

	```


9. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl scale --replicas=4 replicaset/nginx-replicaset -n my-namespaces
	replicaset.apps/nginx-replicaset scaled
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                     READY   STATUS    RESTARTS   AGE
	nginx-replicaset-4wtqf   1/1     Running   0          3m24s
	nginx-replicaset-bmjn8   1/1     Running   0          3m3s
	nginx-replicaset-g64nw   1/1     Running   0          5s
	nginx-replicaset-z4xvn   1/1     Running   0          2m50s
	```

10. Delete the `nginx-replicaset` using `kubectl delete replicaset/<replicaset-name> -n <namespace>`

## Task 6
Create a deployment named `nginx-deployment` for image `nginx:1.42.6` with 3 replicas in the namespace `my-namespace` using YAML configurations

1. Copy the YAML content below
	```
	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: nginx-deployment
	  namespace: my-namespaces
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
	        ports:
	        - containerPort: 80


	```

2. Apply the yaml configurations using `kubectl apply -f <yaml file>`

3. List the deployments in the namespace `my-namespace` using `kubectl get deployments -n <namespace>`
	```
	PS E:\hackathon> kubectl get deployment -n my-namespaces
	NAME               READY   UP-TO-DATE   AVAILABLE   AGE
	nginx-deployment   0/3     3            0           112s

	```
4. List the replicasets in the namespace `my-namespace` using `kubectl get replicaset -n <namespace>`
	```
	PS E:\hackathon> kubectl get replicaset -n my-namespaces
	NAME                         DESIRED   CURRENT   READY   AGE
	nginx-deployment-b5cb4c4fc   3         3         0       2m57s

	```

5. List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                               READY   STATUS             RESTARTS   AGE
	nginx-deployment-b5cb4c4fc-rb7jn   0/1     ImagePullBackOff   0          4m13s
	nginx-deployment-b5cb4c4fc-rvd8c   0/1     ImagePullBackOff   0          4m13s
	nginx-deployment-b5cb4c4fc-xvd96   0/1     ImagePullBackOff   0          4m13s

	```

6. Change the image in YAML to `nginx:1.25.4` and re-apply the yaml using `kubectl apply -f <file-name>`

	```
	PS E:\hackathon> kubectl apply -f .\deployment.yaml     
	deployment.apps/nginx-deployment created

	```
	List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                                READY   STATUS    RESTARTS   AGE
	nginx-deployment-7ffd9c9dbb-752xg   1/1     Running   0          27s
	nginx-deployment-7ffd9c9dbb-jpz9p   1/1     Running   0          27s
	nginx-deployment-7ffd9c9dbb-r8h4q   1/1     Running   0          27s

	```
	  List the replicaset in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get replicaset -n my-namespaces
	NAME                          DESIRED   CURRENT   READY   AGE
	nginx-deployment-7ffd9c9dbb   3         3         3       48s

	```
7. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace` 
	```
	PS E:\hackathon> kubectl get pods -n my-namespaces
	NAME                                READY   STATUS    RESTARTS   AGE
	nginx-deployment-7ffd9c9dbb-8stdb   1/1     Running   0          22s
	nginx-deployment-7ffd9c9dbb-mq5qx   1/1     Running   0          22s
	nginx-deployment-7ffd9c9dbb-pmgn2   1/1     Running   0          22s
	nginx-deployment-7ffd9c9dbb-zmsdj   1/1     Running   0          22s

	```

8. Compare the effort while updating the images in replicaset and deployment
	```
	Replicaset : It maintain a number of pod replicas are running at any given time.
 	Deployment : It enables you to creation,  scaling, replicasets

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
	<Paste output here>

	```
2. Findout why the pods are not ready
	```
	<Explaing the steps you have done to do the debugging>
	
	```
3. Fix the issue and explain your understanding on  `readinessProbe` `livenessProbe`
	
	```
	<Updated deployment yaml>

	```
	Understanding on readness and liveness probe
	```
	<Type your understanding here>

	```

## Cleanup task
Delete the namespace `my-namespace` 
