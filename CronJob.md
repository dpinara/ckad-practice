> **PersistentVolume**
```bash
```


```YAML
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: cronjob-example
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - image: alpine
            name: fail
            command: ['date']
          restartPolicy: Never
```

```YAML
```bash
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  creationTimestamp: null
  name: time-limited-job
spec:
  startingDeadlineSeconds: 17 # add this line
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: time-limited-job
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - args:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
            image: busybox
            name: time-limited-job
            resources: {}
          restartPolicy: Never
  schedule: '* * * * *'
status: {}
```

```
