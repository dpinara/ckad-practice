> **PersistentVolume**
```bash

kubectl get endpoints <Service_Name>
```


```YAML
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: redis
  name: redis
spec:
  ports:
  - port: 6379
    protocol: TCP
    targetPort: 6379
  selector:
    app: redis
  type: ClusterIP
status:
  loadBalancer: {}
```



```text

ubuntu@ip-10-0-128-5:~$ k create svc --help
Create a service using specified subcommand.

Aliases:
service, svc

Available Commands:
clusterip    Create a ClusterIP service.
externalname Create an ExternalName service.
loadbalancer Create a LoadBalancer service.
nodeport     Create a NodePort service.

Usage:
kubectl create service [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
ubuntu@ip-10-0-128-5:~$ k create svc --help 
```

```text
```
