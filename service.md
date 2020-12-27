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
