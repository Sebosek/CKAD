`kubectl create serviceaccount <name>`
`kubectl get serviceaccount`

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
  serviceAccountName: dashboard-sa
  # disable mounting default service account to the pod
  automountServiceAccountToken: false
```

- A token is created automatically with service account
- Token is stored as stored object
- Secret is linked to the service account
- To view token `kubectl describe secret <name-of-sa-token>`
- This token can be used as Bearer token to REST API calls to the kubernetes
- From version 1.24 pod no longer creates a secret
- To create a token, you have to use `kubectl create token <name-of-service-account>` but it only prints token on stdout
- To create a token as non-expiring token secret you have to create a secret

```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: mysecretname
  annotations:
    kubernetes.io/service-account.name: <name-of-service-account>
```