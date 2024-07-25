# Rollout

Rolling out strategies:
 - Recreate
   Delete all pods and create them at once - application is not available until the pod are created and healthy.
 - RollingUpdate
   Delete pods one by one and create new pods during the way.

Rollout status
`kubectl rollout status deployment/myapp-deployment`

Rollout history:
`kubectl rollout history deployment/myapp-deployment`

Apply new deployment creates a new rollout:
`kubectl apply -f deployment-definition.yaml`

alternatively, imperative approach:
`kubectl set image deployment/myapp-deployment nginx-container=nginx:1.9.1`; please see `deployment-definition.yaml`.

# Rollback
`kubectl rollout undo deployment/myapp-deployment`.

## Example
In the example below, we will first create a simple deployment and inspect the rollout status and the rollout history.
```
$ kubectl create deployment nginx --image=nginx:1.16
deployment.apps/nginx created
 
$ kubectl rollout status deployment nginx
Waiting for deployment "nginx" rollout to finish: 0 of 1 updated replicas are available...
deployment "nginx" successfully rolled out
 
$ kubectl rollout history deployment nginx
deployment.extensions/nginx
REVISION CHANGE-CAUSE
1     <none>

$
```

## Using the `--revision` flag.
You can check the status of each revision individually by using the --revision flag:

```
$ kubectl rollout history deployment nginx --revision=1
deployment.extensions/nginx with revision #1
 
Pod Template:
 Labels:    app=nginx    pod-template-hash=6454457cdb
 Containers:  nginx:  Image:   nginx:1.16
  Port:    <none>
  Host Port: <none>
  Environment:    <none>
  Mounts:   <none>
 Volumes:   <none>
$ 
```

## Using the `--record` flag
You would have noticed that the "change-cause" field is empty in the rollout history output. We can use the --record flag to save the command used to create/update a deployment against the revision number.

```
$ kubectl set image deployment nginx nginx=nginx:1.17 --record
deployment.extensions/nginx image updated
 
$ kubectl rollout history deployment nginx
deployment.extensions/nginx
 
REVISION CHANGE-CAUSE
1     <none>
2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
$
```


You can now see that the `change-cause` is recorded for the revision 2 of this deployment.
Let's make some more changes. In the example below, we are editing the deployment and changing the image from nginx:1.17 to nginx:latest while making use of the `--record` flag.

```
$ kubectl edit deployments. nginx --record
deployment.extensions/nginx edited
 
$ kubectl rollout history deployment nginx
REVISION CHANGE-CAUSE
1     <none>
2     kubectl set image deployment nginx nginx=nginx:1.17 --record=true
3     kubectl edit deployments. nginx --record=true
 
$ kubectl rollout history deployment nginx --revision=3
deployment.extensions/nginx with revision #3
 
Pod Template: Labels:    app=nginx
    pod-template-hash=df6487dc Annotations: kubernetes.io/change-cause: kubectl edit deployments. nginx --record=true
 
 Containers:
  nginx:
  Image:   nginx:latest
  Port:    <none>
  Host Port: <none>
  Environment:    <none>
  Mounts:   <none>
 Volumes:   <none>
 
$
```

## Undo a change

Lets now rollback to the previous revision:

```
$ kubectl rollout history deployment nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
3         kubectl edit deployments.apps nginx --record=true
4         kubectl set image deployment nginx nginx=nginx:1.17 --record=true
 
 
 
$ kubectl rollout history deployment nginx --revision=3
deployment.apps/nginx with revision #3
Pod Template:
  Labels:       app=nginx
        pod-template-hash=787f54657b
  Annotations:  kubernetes.io/change-cause: kubectl edit deployments.apps nginx --record=true
  Containers:
   nginx:
    Image:      nginx:latest
    Port:      <none> 
    Host Port:  <none>
    Environment: <none>       
    Mounts:     <none>
  Volumes:      
 
$ kubectl describe deployments. nginx | grep -i image:
    Image:        nginx:1.17
```


With this, we have rolled back to the previous version of the deployment with the image = nginx:1.17.
```
$ kubectl rollout history deployment nginx --revision=1
deployment.apps/nginx with revision #1
Pod Template:
  Labels:       app=nginx
        pod-template-hash=78449c65d4
  Containers:
   nginx:
    Image:      nginx:1.16
    Port:       <none> 
    Host Port:  <none>
    Environment: <none>     
    Mounts:     <none>
  Volumes:      
 
$ kubectl rollout undo deployment nginx --to-revision=1
deployment.apps/nginx rolled back
```

To rollback to specific revision we will use the --to-revision flag.
With --to-revision=1, it will be rolled back with the first image we used to create a deployment as we can see in the rollout history output.

```
$ kubectl describe deployments. nginx | grep -i image:
Image: nginx:1.16
```