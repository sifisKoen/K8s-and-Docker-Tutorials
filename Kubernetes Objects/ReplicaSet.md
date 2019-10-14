Replicas and create a replication controler:

--> You need to write a .ylm file
like :


apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
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

  replicas: 6

  selector:
    matchLabels:
      app: myapp
      costcenter: iosif
      location: UK
      type: frontend




Te apiVersion is specific to what we are
creating. In this case replication controller is supported in kubernetes apiVersion v1.
So we will write it as v1.
The kind as we know is ReplicationController.

                                        
So far, it has been very similar to howiii we created a POD in the previous section



The next is the most crucial part of the
definition file and that is the specification written as spec. For any kubernetes
definition file, the spec section defines what’s inside the object we are creating. In
this case we know that the replication controller creates multiple instances of a POD.
But what POD? We create a template section under spec to provide a POD template
to be used by the replication controller to create replicas. Now how do we DEFINE
the POD template



Some Commands : 

Create the replica Set:
                    < the name of your .yml file >
>kubectl create –f replicaset-definition.yml

Get your replica Controllers:
>kubectl get replicationcontroller
 
See all your pods:
>kubectl get pods

To scale up your app / for more PODs :

First:
You can go to your file --> vim / (a path) /<filename>
and go to change the replicas field.
Second:
Save your change and go aout from vim: <Esc>, :wq! 
Third:
type the comand:
>kubectl replace -f replicaset-definition.yml

Second Way: #Don't_do_it

The second way to do it is to run the kubectl scale command. Use the replicas
parameter to provide the new number of replicas and specify the same file as input.
You may either input the definition file or provide the replicaset name in the TYPE
Name format. However, Remember that using the file name as input will not result in
the number of replicas being updated automatically in the file. In otherwords, the
number of replicas in the replicaset-definition file will still be 3 even though you
scaled your replicaset to have 6 replicas using the kubectl scale command and the file
as input.

The comands:
>kubectl scale -–replicas=6 –f replicaset-definition.yml

>kubectl scale -–replicas=6 replicaset myapp-replicaset

ONLY THE COMMANDS
=======================================================================================
  - kubectl create –f replicaset-definition.yml
  - kubectl get replicaset
  - kubectl delete replicaset myapp-replicaset   #Also deletes all underlying PODs
  - kubectl replace -f replicaset-definition.yml
  - kubectl scale –replicas=6 -f replicaset-definition.yml



