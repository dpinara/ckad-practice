> **PersistentVolume**
```bash
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


