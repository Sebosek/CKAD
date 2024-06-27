Seting a security context at pod level

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      command: ["sleep", "3600"]
```

Seting a security context at container level

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
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        # Adding restricted capabilities which aren't available by default
        capabilities:
          add: ["MAC_ADMIN"]
```