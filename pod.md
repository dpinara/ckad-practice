**Pod**
> the present is our past.

````
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: webserver
spec:
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
    # Multi container pods
    - name: counter
      image: lrakai/microservices:counter-v1
      env:
        - name: API_URL
          value: http://localhost:8080     
````