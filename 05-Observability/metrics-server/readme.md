# Installation

https://github.com/kubernetes-sigs/metrics-server

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability-1.21+.yaml
```

# Installation by old way

1. Clone the git repo

```
git clone git@github.com:kubernetes-sigs/metrics-server.git
```

2. Install it using `kubectl create`

```
kubectl create -f deploy/1.8+/
```

