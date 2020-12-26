> **Pod**
```bash
k run pod1 \
    -o yaml \
    --dry-run=client \
    --image=busybox \
    --requests "cpu=100m,memory=256Mi" \
    --limits "cpu=200m,memory=512Mi" \
    --command \
    -- sh -c "sleep 1d"
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: webserver
spec:
  securityContext: # insert this line
    runAsUser: 101 # UID for the user
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: accelerator
                operator: In
                values:
                  - nvidia-tesla-p100
  initContainers: #
    - args: #
        - /bin/sh #
        - -c #
        - wget -O /work-dir/index.html http://neverssl.com/online #
      image: busybox 
      securityContext: 
        capabilities: 
          add: ["NET_ADMIN", "SYS_TIME"] 
      name: box 
      volumeMounts: 
        - name: vol 
          mountPath: /work-dir 
  containers:
  - name: mycontainer
    image: nginx:latest
    imagePullPolicy: IfNotPresent
    resources:
      requests:
        memory: "128Mi" # 128Mi = 128 mebibytes
        cpu: "500m"     # 500m = 500 milliCPUs (1/2 CPU)
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 80
    volumeMounts:
      - name: vol
        mountPath: /usr/share/nginx/html
    livenessProbe:
      initialDelaySeconds: 5 
      periodSeconds: 5 
      exec: 
        command: 
          - ls 
    readinessProbe: 
      httpGet: 
        path: / 
        port: 80 
        
  volumes:
    - name: vol
      emptyDir: {}
  nodeSelector: 
    accelerator: nvidia-tesla-p100 

  # Multi containers...
  - name: mycontainer2
    image: nginx:latest
````