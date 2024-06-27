Setting taints to the node `kubectl taint nodes node-name key=value:taint-effect`.

`taint-effect` - what happens to PODs that do not tolarate this taint.
- `NoSchedule` - 
- `PreferNoSchedule` - 
- `NoExecute` - 

Example: `kubectl taint nodes node1 app=blue:NoSchedule`

Tolerations applies to PODs.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-kubernetes-dashboard
  labels:
    name: my-kubernetes-dashboard
spec:
  containers:
    - name: my-kubernetes-dashboard
      image: my-kubernetes-dashboard
  # disable mounting default service account to the pod
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```

To see taint on master node use `kubectl describe node kubemaster | grep Taint`.