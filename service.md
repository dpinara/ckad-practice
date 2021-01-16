> **Service**
```bash

kubectl get endpoints <Service_Name>
kubectl expose pod webserver-logs --type=LoadBalancer
# Simply put the EXTERNAL_IP on browser and it will work

# One can expose pod as well
# command for exposing is sequential and mothful
kubectl expose pod redis --name=redis-service --port=6379


controlplane $ k get all
NAME                  READY   STATUS    RESTARTS   AGE
pod/simple-webapp-1   1/1     Running   0          11s

NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP   10.96.0.1      <none>        443/TCP          2m59s
service/webapp-service   NodePort    10.97.125.46   <none>        8080:30080/TCP   11s
---
# See the naming convention for srvice and how it can be accessed.

controlplane $ cat curl-test.sh
for i in {1..20}; do
   kubectl exec --namespace=kube-public curl -- sh -c 'test=`wget -qO- -T 2  http://webapp-service.default.svc.cluster.local:8080/ready 2>&1` && echo "$test OK" || echo "Failed"';
   echo ""
donecontrolplane $ cat freeze-app.sh
nohup kubectl exec --namespace=kube-public curl -- wget -qO- http://webapp-service.default.svc.cluster.local:8080/freeze &
controlplane $ cat crash-app.sh
kubectl exec --namespace=kube-public curl -- wget -qO- http://webapp-service.default.svc.cluster.local:8080/crash
controlplane $ k get svc -o wide
NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE     SELECTOR
kubernetes       ClusterIP   10.96.0.1      <none>        443/TCP          10m     <none>
webapp-service   NodePort    10.97.22.150   <none>        8080:30080/TCP   9m11s   name=simple-webapp
controlplane $


controlplane $ cat curl-test.sh
for i in {1..35}; do
   kubectl exec --namespace=kube-public curl -- sh -c 'test=`wget -qO- -T 2  http://webapp-service.default.svc.cluster.local:8080/info 2>&1` && echo "$test OK" || echo "Failed"';
   echo ""
donecontrolplane $ k get ns

---

# Considering we have service which is exposing secure pod on port 80
controlplane $ k describe svc secure-service
Name:              secure-service
Namespace:         default
Labels:            run=secure-pod
Annotations:       <none>
Selector:          run=secure-pod
Type:              ClusterIP
IP:                10.106.23.59
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         10.32.0.6:80
Session Affinity:  None
Events:            <none>
controlplane $

# To test if other pod ie webapp-color can access this pod through service 
controlplane $ k exec -it webapp-color -- /bin/sh
/opt # nc -z -v -w 1 secure-service 80
secure-service (10.106.23.59:80) open

/opt # nc --help
BusyBox v1.28.4 (2018-07-17 15:21:40 UTC) multi-call binary.

Usage: nc [OPTIONS] HOST PORT  - connect
nc [OPTIONS] -l -p PORT [HOST] [PORT]  - listen

        -e PROG Run PROG after connect (must be last)
        -l      Listen mode, for inbound connects
        -lk     With -e, provides persistent server
        -p PORT Local port
        -s ADDR Local address
        -w SEC  Timeout for connects and final net reads
        -i SEC  Delay interval for lines sent
        -n      Don't do DNS resolution
        -u      UDP mode
        -v      Verbose
        -o FILE Hex dump traffic
        -z      Zero-I/O mode (scanning)
/opt #

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

##### clusterIP: None for headliess service
```YAML
# Headless service for stable DNS entries of StatefulSet members.
apiVersion: v1
kind: Service
metadata:
  name: mysql
  labels:
    app: mysql
spec:
  ports:
    - name: mysql
      port: 3306
  clusterIP: None
  selector:
    app: mysql
---
# Client service for connecting to any MySQL instance for reads.
# For writes, you must instead connect to the master: mysql-0.mysql.
apiVersion: v1
kind: Service
metadata:
  name: mysql-read
  labels:
    app: mysql
spec:
  ports:
    - name: mysql
      port: 3306
  selector:
    app: mysql
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


controlplane $ cat run.txt
Create a ClusterIP service with the specified name.

Examples:
  # Create a new ClusterIP service named my-cs
  kubectl create service clusterip my-cs --tcp=5678:8080

  # Create a new ClusterIP service named my-cs (in headless mode)
  kubectl create service clusterip my-cs --clusterip="None"

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in the template. Only applies to golang and jsonpath output formats.
      --clusterip='': Assign your own ClusterIP or set to 'None' for a 'headless' service (no loadbalancing).
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be sent, without sending it.If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
  -o, --output='': Output format. One of: json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --tcp=[]: Port pairs can be specified as '<port>:<targetPort>'.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create service clusterip NAME [--tcp=<port>:<targetPort>] [--dry-run=server|client|none] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
Create a NodePort service with the specified name.

Examples:
  # Create a new NodePort service named my-ns
  kubectl create service nodeport my-ns --tcp=5678:8080

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in the template. Only applies to golang and jsonpath output formats.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be sent, without sending it.If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
      --node-port=0: Port used to expose the service on each node in a cluster.
  -o, --output='': Output format. One of: json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --tcp=[]: Port pairs can be specified as '<port>:<targetPort>'.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create service nodeport NAME [--tcp=port:targetPort] [--dry-run=server|client|none] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
controlplane $

```

```text
```
