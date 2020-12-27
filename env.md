> **env**
```bash
```


```YAML
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: sega
  name: sega
spec:
  containers:
  - image: nginx
    name: sonic
    env:
      - name: NGINX_PORT
        value: "8080"
  - command:
    - sleep
    - "3600"
    image: busybox
    name: tails
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
#### Note - value for env name should be in double quote. 

```YAML
```

```text
```
