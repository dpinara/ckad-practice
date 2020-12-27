> **securityContext**
```bash
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
    imagePullPolicy: Always
    name: ubuntu
    resources: {}
```

```YAML
```

```text
```
