

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
Create a Pod with name `nginx` with images `nginx:latest`, `nginx:1.14.2` in `my-namespace` using yaml configurations.

Create a YAML for a Pod with multiple containers with container names `first`, `second` for the given images


Copy the YAML content below
```
<Paste output here>

```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
<Paste output here>

```

## Task 3

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs

get the logs for container `first` in `nginx` pod

```
<Paste the output of kubectl logs command>

```

get the logs for container `second` in `nginx` pod

```
<Paste the output of kubectl logs command>

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
	<Paste output here>

	```

4. List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	```

5. Find out why the pods are failing in the `nginx-replicaset` replicaset 
	```
	<Explain how you found the issue>

	```

6. Fix the issue in the YAML configuration created  and re-apply configuration using `kubectl apply`

	List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	``

7. Do a `kubectl delete -f <file-name>` and apply it again using `kubectl apply -f <file-name>`

	List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	```

8. Explain your observations from steps 6,7

	```
	<Type observation here>

	```


9. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace` 
	```
	<Paste output here>
	```

10. Delete the `nginx-replicaset` using `kubectl delete replicaset/<replicaset-name> -n <namespace>`

## Task 6
Create a deployment named `nginx-deployment` for image `nginx:apline` with 3 replicas in the namespace `my-namespace` using YAML configurations

1. Copy the YAML content below
	```
	<Paste output here>

	```

2. Apply the yaml configurations using `kubectl apply -f <yaml file>`

3. List the deployments in the namespace `my-namespace` using `kubectl get deployments -n <namespace>`
	```
	<Paste output here>

	```
4. List the replicasets in the namespace `my-namespace` using `kubectl get replicaset -n <namespace>`
	```
	<Paste output here>

	```

5. List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	```

6. Change the image in YAML to `nginx:1.25.4` and re-apply the yaml using `kubectl apply -f <file-name>`

	```
	<Paste output of kubectl apply here>

	```
	List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	```
	  List the replicaset in the namespace `my-namespace` 
	```
	<Paste output here>

	```
7. Increase/dicrease the count of replicas by 1 in the YAML and apply it again 

	List the pods in the namespace `my-namespace` 
	```
	<Paste output here>

	```

8. Compare the effort while updating the images in replicaset and deployment
	```
	Your observation and comparison between 
	a replicaset and deployment

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
