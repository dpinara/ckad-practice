> **command**
```bash
kubectl run sega --image=busybox -- seep 3600 -o yaml --dry-run 

command: ["/bin/sh","-c"]
args: ["sleep 30 && httpd-foreground"]

```
######   1) Note - use -- for command. This seems to be new syntax
######   2) Note - sh -c In ubuntu, sh is usually symlinked to /bin/dash . That is use dash, instead of sh. To get this force, use "sh -c" 

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
#### Note - value for command name should be in double quote.


```YAML
      containers:
      - command:
        - sh
        - -c
        - echo Hello Kubernetes! && sleep 3600
        image: busybox
```

```text
```
