# General helper commands

Enter to pod to do something...
```
kubectl exec -it <pod_name> <container_name> -- [command]
```

eq, run shell in the container to do ... `kubectl exec -it <pod_name> <container_name> -- sh` (`/bin/bash` in Debian).

# Export existing configuration

````
kubectl get pod app -n elastic-stack -o yaml > app.yaml
```