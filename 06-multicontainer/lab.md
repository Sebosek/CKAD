Step 3
------

Create a multi-container pod with 2 containers.
Use the spec given below:
If the pod goes into the crashloopbackoff then add the command sleep 1000 in the lemon container.
 - Name: yellow
 - Container 1 Name: lemon
 - Container 1 Image: busybox
 - Container 2 Name: gold
 - Container 2 Image: redis

Using imperative command: `kubectl run yellow --image=busybox --dry-run=client -o yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: yellow
  labels:
    name: yellow
spec:
  containers:
    - name: lemon
      image: busybox
      command: [ "sleep", "1000" ]
    - name: gold
      image: redis
```

Step 8
------

Edit the pod in the elastic-stack namespace to add a sidecar container to send logs to Elastic Search. Mount the log volume to the sidecar container.
Only add a new container. Do not modify anything else. Use the spec provided below.

> Note: State persistence concepts are discussed in detail later in this course. For now please make use of the below documentation link for updating the concerning pod.

https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/

1. Get the current pod definition `kubectl get pod app -n elastic-stack -o yaml > app.yaml`
2. Edit the pod definition and add new container with mounted volume
3. Try to apply changes `kubectl apply -f app.yaml`
4. But it will fail with unability to update pod to add or remove containers
5. Apply force replace `k replace --force -f app.yaml`