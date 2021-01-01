> **ServiceAccount**

````bash
#Run the command and check the user running the user that is running the container.
kubectl exec ubuntu-sleeper -- whoami

#To add service account, all need to do is to create sa, through BARC add role and previleges for that role and mainly put sa as part of peer for image in container..

serviceAccount: dashboard-sa

````




```text
Kubernetes uses ServiceAccounts as a mechanism for providing Pods with an identity in the cluster. Pod's can authenticate using ServiceAccounts and gain access to APIs that the ServiceAccount has been granted. Your cluster administrator can create specific roles that grant access to APIs and bind the roles to ServiceAccounts. This is referred to as role-based access control (RBAC). Pods can then declare a ServiceAccount in their specification to gain the access associated with the ServiceAccount's role. As an example, you could use a ServiceAccount to grant a Pod access to the GET Pod API to allow the Pod to get the details of other Pods. This Lab Step focuses on ServiceAccounts and not the roles that are used to grant access to Kubernetes APIs that would be configured by a Kubernetes administrator.

Each Namespace has a default ServiceAccount. The default ServiceAccount grants minimal access to APIs and cannot be used to get any cluster state information. Therefore, you should use custom ServiceAccounts when your application requires access to cluster state.

kubectl get pod custom-sa-pod -o yaml | more

The output confirms the app-sa ServiceAccount is being used. Every ServiceAccount has a corresponding token secret (see them with kubectl get secrets) that can be used to authenticate requests from inside the container. The ServiceAccount's token secret is automatically mounted as a volume. That is what you see in the volumeMounts configuration.



```
```bash
kubectl run default-pod --generator=run-pod/v1 --image=mongo:4.0.6 --serviceaccount=app-sa --dry-run -o yaml

```


```YAML
```

```YAML
```

```text
controlplane $ k create sa --help
Create a service account with the specified name.

Aliases:
serviceaccount, sa

Examples:
  # Create a new service account named my-service-account
  kubectl create serviceaccount my-service-account

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
the template. Only applies to golang and jsonpath output formats.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be
sent, without sending it. If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
  -o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create serviceaccount NAME [--dry-run=server|client|none] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
controlplane $
```
