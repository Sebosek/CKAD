Configuration can be defined in pod definition or using a `ConfigMap`, which is Kubernetes resource.
The config map has to be used in two phases. In the first phase define the config map and in the second you reference the config map in pod definition.

There a two ways to create a config map.
Using imperativ way `kubectl create configmap <config-name> --from-literal=<key>=<value> --from-literal=<key>=<value>` or using a file with configuration `kubectl create configmap <config-name> --from-file=<path-to-file>`.

The second approach uses declarative way `kubectl create -f <config-map.yaml>` or `kubectl apply -f <config-map.yaml>`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```

```
kubectl get configmaps
kubectl describe configmaps
```

Now we need use config map with POD.

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
    - configMapRef:
        name: app-config
    # single environment variable
    env:
    - name: APP_COLOR
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_COLOR
    # file volume
    volumes:
    - name: app-config-volume
      configMap:
        name: app-config
```