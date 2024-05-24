Label notes `kubectl label nodes <node-name> <label-key>=<value>`
Example: `kubectl label nodes node1 size=Large`

Node selector applied to a POD.

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
  nodeSelector:
    size: Large
```
