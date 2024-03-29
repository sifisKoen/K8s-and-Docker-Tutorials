Deployment
===========


We first create a deployment definition file. The contents of the deployment-definition file
are exactly similar to the replicaset definition file, except for the kind, which is now
going to be Deployment.



If we walk through the contents of the file it has an apiVersion which is apps/v1,
metadata which has name and labels and a spec that has template, replicas and
selector. The template has a POD definition inside it.



The fields:
```YAML
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: myapp-delpoyment
    labels:
      app: myapp
      type: front-end
  spec:
    template:
      metadata:
        name: myapp-pod
        labels:
          app: myapp
          costcenter: iosif
          location: UK
          type: frontend
      spec:
          containers:
            - name: nginx-container
              image: nginx
     replicas: 3
    selector:
      matchLabels:
        app: myapp
        costcenter: iosif
        location: UK
        type: frontend
```

Once the file is ready run the kubectl create command and specify deployment
definition file. Then run the kubectl get deployments command to see the newly
created deployment. The deployment automatically creates a replica set. So if you
run the kubectl get replcaset command you will be able to see a new replicaset in the
name of the deployment. The replicasets ultimately create pods, so if you run the
kubectl get pods command you will be able to see the pods with the name of the
deployment and the replicaset.

THE COMMANDS:
 - kubectl create –f deployment-definition.yml
 - kubectl get deployments #*
 - kubectl get replicaset
 - kubectl get pods
 - kubectl get all #*2


#* The kubectl get deployments command show you alla yous deployments.

#*2 See all the created objects at once.
