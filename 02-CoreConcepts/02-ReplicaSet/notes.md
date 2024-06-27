# 02 ReplicaSet Lab Notes

In the cluster, there already exist a replicaset however, with wrong container definition. - The container has wrong image name.

To get definition for ReplicaSet into the YAML file use command

```
kubectl get replicaset new-replica-set -o yaml > new-replica-fix.yaml
```

Then, update the `spec.template.spec.containers[0].image` with correct image name.

```
kubectl replace -f new-replica-fix.yaml
```

> **Now I have to delete all pods in ReplicaSet to force K8s to use updated pods!** The different way would use `kubectl edit`, however, it uses Vim 🤢 or delete replicaset and create it again.
