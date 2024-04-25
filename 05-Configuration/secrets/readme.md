Secrets can be defined in pod definition or using a `Secret` resource.
The secrets have to be used in two phases. In the first phase define the secrets and in the second you reference secrets in pod definition.

There a two ways to create a secret.
Using imperativ way `kubectl create secret generic <secret-name> --from-literal=<key>=<value> --from-literal=<key>=<value>` or using a file with configuration `kubectl create secret generic <secret-name> --from-file=<path-to-file>`.

The second approach uses declarative way `kubectl create -f <config-map.yaml>` or `kubectl apply -f <config-map.yaml>`.
Secrets has to be encoded using base64. On unix run `echo -n 'myslq' | base64` --> `bXlzcWw=`.
To decode use `echo -n 'bXlzcWw=' | base64 --decode`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_Host: bXlzcWw=
  DB_User: ...
  DB_Password: ...
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  DB_Host: c3FsMDE=
  DB_User: cm9vdA==
  DB_Password: cGFzc3dvcmQxMjM=
```

```
kubectl get secrets
kubectl describe secrets
kubectl get secret app-secret -o yaml
```

Now we need use secret with POD.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
    - containerPort: 8080
    # inject as environment variable
    envFrom:
    - secretRef:
        name: app-secret
    # single environment variable
    env:
    - name: DB_Host
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: DB_Host
    # file volume
    volumes:
    - name: app-config-volume
      secret:
        name: app-secret
```