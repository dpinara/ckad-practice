> **PersistentVolume**
```bash
kubectl expose deployment  redis --port=6379 --name=redis --type=ClusterIP --dry-run -o yaml

kubectl  create deploy redis \
--image=redis:alpine \
--replicas=1 \
-o yaml --dry-run=client 

```
###### Note: when we need to expose ClusterIP, we need to write service. --port doesnt work as part of imperative command.


```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  strategy: {}
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - image: redis:alpine
        name: redis
        resources: {}
status: {}
```

```YAML
```

```text
```
