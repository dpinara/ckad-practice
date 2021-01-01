> **Title**
```bash
#Important note - We need to put taint for node and toleration in pod to address that, while dealing with taint/toleration
# However node affinity deals will deal with labels. just check what labels are there in node and use same label in affinity yaml inside pod to give preferences
# Affinity is applied on spec of pod and not on spec of deployment. so while doing copy paste take this in mind

#Add and remove taint
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-

```


```YAML
```

```YAML
tolerations:
- key: "example-key"
  operator: "Exists"
  effect: "NoSchedule"

OR

tolerations:
  - key: "key1"
    operator: "Equal"
    value: "value1"
    effect: "NoSchedule"

```

```yaml
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/e2e-az-name
            operator: In
            values:
            - e2e-az1
            - e2e-az2
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: Exists

#Here key and value wil be lablel in respective object
# Operatror value could be -   In, NotIn, Exists, DoesNotExist.

```

```text
```


