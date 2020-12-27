> **PersistentVolume**
```bash
kubectl create configmap cm-3392845 --from-literal=DB_NAME=SQL3322 --from-literal=DB_HOST=sql322.mycompany.com

```


```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: redis-config
data:
  config: | # YAML for multi-line string
    # Redis config file
    tcp-keepalive 240
    maxmemory 1mb
```

```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: random-generator-config
data:
  PATTERN: Configuration Resource
  application.properties: | # YAML for multi-line string
    # Random Generator config
    log.file=/tmp/generator.log
    version=1.0
    server.port=7070
  # Used with a prefix 'env.' for being picked up
  # by a Pod's 'envFrom' declaration:
  EXTRA_OPTIONS: "high-secure,native"
  SEED: "432576345"

```

```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: webapp-config
  annotations:
    k8spatterns.io/podDeleteSelector: "app=webapp"
data:
  message: "Welcome to Kubernetes Patterns !"
```

```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: cwagentconfig
  namespace: amazon-cloudwatch
data:
  cwagentconfig.json: |
    {
      "logs": {
        "metrics_collected": {
          "kubernetes": {
            "cluster_name": "{{cluster_name}}",
            "metrics_collection_interval": 60
          }
        },
        "force_flush_interval": 5
      }
    }
```



```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: postgres-config
  labels:
    app: postgres
data:
  POSTGRES_DB: postgresdb
  POSTGRES_USER: testuser
  POSTGRES_PASSWORD: testpassword123

```

```YAML

```

```YAML

```

```YAML

```



```text
Create a configmap based on a file, directory, or specified literal value.

 A single configmap may package one or more key/value pairs.

 When creating a configmap based on a file, the key will default to the basename of the file, and the value will default
to the file content.  If the basename is an invalid key, you may specify an alternate key.

 When creating a configmap based on a directory, each file whose basename is a valid key in the directory will be
packaged into the configmap.  Any directory entries except regular files are ignored (e.g. subdirectories, symlinks,
devices, pipes, etc).

Aliases:
configmap, cm

Examples:
  # Create a new configmap named my-config based on folder bar
  kubectl create configmap my-config --from-file=path/to/bar

  # Create a new configmap named my-config with specified keys instead of file basenames on disk
  kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt --from-file=key2=/path/to/bar/file2.txt

  # Create a new configmap named my-config with key1=config1 and key2=config2
  kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2

  # Create a new configmap named my-config from the key=value pairs in the file
  kubectl create configmap my-config --from-file=path/to/bar

  # Create a new configmap named my-config from an env file
  kubectl create configmap my-config --from-env-file=path/to/bar.env

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
the template. Only applies to golang and jsonpath output formats.
      --append-hash=false: Append a hash of the configmap to its name.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be
sent, without sending it. If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
      --from-env-file='': Specify the path to a file to read lines of key=val pairs to create a configmap (i.e. a Docker
.env file).
      --from-file=[]: Key file can be specified using its file path, in which case file basename will be used as
configmap key, or optionally with a key and file path, in which case the given key will be used.  Specifying a directory
will iterate each named file in the directory whose basename is a valid configmap key.
      --from-literal=[]: Specify a key and literal value to insert in configmap (i.e. mykey=somevalue)
  -o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create configmap NAME [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run=server|client|none]
[options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
controlplane $
```
