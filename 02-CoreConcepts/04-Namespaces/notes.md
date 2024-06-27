# 04 Namespaces Lab Notes

To reach the service in the namespace use only the service name `db-service`. To reach the service in a different namespace use the DNS name `db-service.dev.svc.cluster.local`, the `dev` is the other namespace.

```
kubectl get pods --namespace=kube-system
```

To define a namespace in the Kubernetes definition file, use `namespace` field in the `metadata` section.
To create namespace, use `kubectl create/apply -f .template.yaml` or `kubectl create namespace dev`.

To set namespace permanently, use the command 
`kubectl config set-context $(kubectl config current-context) --namespace=dev`.

To see resources in all namespaces use `--all-namespaces` option for `kubectl` command.

## Limit namespace resources
Create resource quota resource.

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```