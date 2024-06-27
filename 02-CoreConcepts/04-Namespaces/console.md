```
controlplane ~ ➜  kubectl run nginx-pod --image=nginx:alpine
pod/nginx-pod created

controlplane ~ ➜  kubectl run redis --image=redis:alpine --labels="tier=db"
pod/redis created

controlplane ~ ➜  kubectl expose pod redis --port=6379 --name redis-service
service/redis-service exposed

controlplane ~ ➜  kubectl create deployment webapp --image=kodekloud/webapp-color --replicas=3
deployment.apps/webapp created

controlplane ~ ➜  kubectl run custom-nginx --image=nginx --port=8080
pod/custom-nginx created

controlplane ~ ➜  kubectl create namespace dev-ns
namespace/dev-ns created

controlplane ~ ➜  kubectl create deployment redis-deploy --namespace=dev-ns --image=redis --replicas=2
deployment.apps/redis-deploy created`
```

Create a pod called httpd using the image httpd:alpine in the default namespace. Next, create a service of type ClusterIP by the same name (httpd). The target port for the service should be 80.
Try to do this with as few steps as possible.

> The key is use the pod label `app=httpd`, because the `kubectl create service clusterip XXX` expects that the pod is labeled with `app=XXX`.

```
controlplane ~ ➜  kubectl run httpd --image=httpd:alpine --labels="app=httpd" & kubectl create service clusterip httpd --tcp=80:80
[1] 12460
service/httpd created
pod/httpd created
```

However, the shorter version is the following.

```
controlplane ~ ➜  kubectl run httpd --image=httpd:alpine --port=80 --expose=true
service/httpd created
pod/httpd created
```
