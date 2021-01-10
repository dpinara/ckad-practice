> **StorageClass**
```bash

```

```text

controlplane $ k get sc
NAME                        PROVISIONER                     RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-storage               kubernetes.io/no-provisioner    Delete          WaitForFirstConsumer   false                  9s
portworx-io-priority-high   kubernetes.io/portworx-volume   Delete          Immediate              false                  9s

#Storage Class that does not support dynamic volume provisioning uses following field value as no-provisioner
Provisioner:           kubernetes.io/no-provisioner

```


```YAML
```

```YAML
```

```text
```

```text
```
