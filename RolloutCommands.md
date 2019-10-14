Rollout
=======

```
> kubectl rollout status deployment/myapp-deployment
Waiting for rollout to finish: 0 of 10 updated replicas are available...
Waiting for rollout to finish: 1 of 10 updated replicas are available...
Waiting for rollout to finish: 2 of 10 updated replicas are available...
Waiting for rollout to finish: 3 of 10 updated replicas are available...
Waiting for rollout to finish: 4 of 10 updated replicas are available...
Waiting for rollout to finish: 5 of 10 updated replicas are available...
Waiting for rollout to finish: 6 of 10 updated replicas are available...
Waiting for rollout to finish: 7 of 10 updated replicas are available...
Waiting for rollout to finish: 8 of 10 updated replicas are available...
Waiting for rollout to finish: 9 of 10 updated replicas are available...
deployment "myapp-deployment" successfully rolled out
```

History command:
```
kubectl rollout history deployment/myapp-deployment
```
```
deployments "myapp-deployment"
REVISION CHANGE-CAUSE
1        <none>
2        kubectl apply --filename=deployment-definition.yml --record=true
```


Ok now if we go to the `.yml` file of deploymet --> `vim /path/filename.yml`.

And for excapmle we change the image vertion --> `image: nginx to image: nginx:1.7.1` #<-- random version

We can applt the change :
```kubectl apply –f deployment-definition.yml```

#### Or :

```kubectl set image deployment/myapp-deployment \nginx=nginx:1.7.1```

But remember, if you do it in this way
tha result in the `deployment-definition file` will have a different configuration. So you
must be careful when using the same definition file to make changes in the future.

You can Rollback to an older deployment:
``` kubectl rollout undo deployment/myapp-deployment```


THE COMMANDS:

Create  ```kubectl create –f deployment-definition.yml```

Get     ```kubectl get deployments```

Update  ```kubectl apply –f deployment-definition.yml```
        ```kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1```

Status  ```kubectl rollout status deployment/myapp-deployment```
	```kubectl rollout history deployment/myapp-deployment```

Rollback ```kubectl rollout undo deployment/myapp-deployment```



