Deploy a POD :
	
	kubectl run nginx --image=nginx

See all pods:
	
	kubectl get pods

See all pods + Node + IP address
	
	kubectl get pods -o wide

Full describe of all pods :

	kubectl describe pods

For exapmle
```
Name:         nginx-6db489d4b7-p7vlq
Namespace:    default
Priority:     0
Node:         fognode3/10.70.26.188
Start Time:   Tue, 08 Oct 2019 15:54:12 +0100
Labels:       pod-template-hash=6db489d4b7
              run=nginx
Annotations:  <none>
Status:       Running
IP:           10.244.2.2
IPs:
  IP:           10.244.2.2
Controlled By:  ReplicaSet/nginx-6db489d4b7
Containers:
  nginx:
    Container ID:   docker://191e41e44bf255f64c059f832efbaecd4cbf466f444309ce4e7d4a7e33587b21
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:aeded0f2a861747f43a01cf1018cf9efe2bdd02afd57d2b11fcc7fcadc16ccd1
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Tue, 08 Oct 2019 15:54:42 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-ttjpd (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-ttjpd:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-ttjpd
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:          <none>
```

Create your own pod :
	
	kubectl create -f <name of yml file you create> e.x. --> pod-definition.yml

Run :

	kubectl get pods ( to see if your pod is created )

