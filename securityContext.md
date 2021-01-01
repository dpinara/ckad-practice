> **securityContext**
```bash
kubectl explain pod.spec.securityContext | more
kubectl explain pod.spec.containers.securityContext | more

kubectl exec security-context-test-2 -it -- ls /dev
## Notice that the shell prompt is $ and not # indicating that you are not the root user.



Update pod 'ubuntu-sleeper' to run as Root user and with the 'SYS_TIME' capability.
date -s '19 APR 2012 11:14:00'

#Search for SYS_TIME for capability. Also remember that this is child of container..
securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
        
```


```YAML
spec:
  securityContext:
    runAsUser: 0
  containers:
  - command:
    - sleep
    - "4800"
    image: ubuntu
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
      privileged: true
    imagePullPolicy: Always
    name: ubuntu
    resources: {}
```

```YAML
```

```text
```
