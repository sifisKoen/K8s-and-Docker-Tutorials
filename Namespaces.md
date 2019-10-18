Namespaces
========


You can create a namespace.

 1)
   Create a YAML file ```Namespace-dev.yml```.
   
 2)
   ```
    kubectl create -f Namespace-dev.yml
   ```
OR
```
  kubectl create namespace dev
  			<name of namespace you want to create>
```

If you want to see the pods of other namespaces.

```
kubectl get pods –namespace=<name of the namespace>
```

If you want you want to put a pod into a namespace.

You can create a pod definition file and after go to your command line and exec the command:

```
kubectl create -f <name of pod definition file YAML> --namespace=<name of the namespace>
```

OR

```
kubectl run redis –image=redis –generator=run-pod/v1 --namespace=finance
```

Now if you want you can insert the namespace into to your YAML pod file. 

Example :

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    name: myapp-pod
    namespace: dev #the namespace
spec:
  containers:
    - name: nginx-container
      image: nginx
```

And now create the namespace.

```
kubectl create -f <name of pod definition file YAML>
```


List all pods in all namespaces:

```
kubectl get pods --all-namespaces  
```


For default you are at the default namespace.
Now if you want to change to another namespace you need to exec this command.

```
kubectl config set-context $(kubectl config current-context) - - namespace=dev 
									<the name of namespace>
```

To limit the resources of the namespaces you need to write Quota. 

Also if you want to create a resource quota you need to write a YAML file.


```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota 
  namespace: dev #specify the namespace for witch you create quota.
spec:
  hard: # Provide your limits.
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
  
```
