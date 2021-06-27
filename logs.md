> **logs debug and monitoring**
```bash
# Use netstat, netcat, wget etc for testing..
1) 
kubectl logs -f --tail=1 --timestamps pod-logs client
# Here -f is nothing but --follow

2) 

k get events

3)
// List the pod with different verbosity
kubectl get po nginx --v=7
kubectl get po nginx --v=8
kubectl get po nginx --v=9


```
