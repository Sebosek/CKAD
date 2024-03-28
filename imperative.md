# Imperative commands

Create an NGINX pod by running `kubectl run nginx --image=nginx`.
Generate manifest by running `kubectl run nginx --image=nginx --dry-run=client -o yaml`.

Create a deployment by running `kubectl create deployment nginx --image=nginx`.
Generate manifest by running `kubectl create deployment nginx --image=nginx --dry-run -o yaml`.

Generate Deployment with 4 Replicas by running `kubectl create deployment nginx --image=nginx --replicas=4`, or scale deployment using `kubectl scale deployment nginx --replicas=4`.

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379 using `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`, which will automatically use the pod's labels as selectors. Or using `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml` however, this will not use the pod's labels as selectors, instead it will assume selectors as app=redis.

Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes using `kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml`, again, it automatically use the pod's labels as selectors but you can't specify the node port. The other way would be `kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`, but this time it will not use the pod's labels as selectors.

[Conventions](https://kubernetes.io/docs/reference/kubectl/conventions/)
