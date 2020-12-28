> **Ingress**
```bash
```

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: web-ingress
  namespace: linkerd
  annotations:
    kubernetes.io/ingress.class: "nginx"
Required"
spec:
  rules:
    - host: dashboard.example.com
      http:
        paths:
          - backend:
              serviceName: linkerd-web
              servicePort: 8084

````


```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: kubia
spec:
  tls:
    - hosts:
        - kubia.example.com
      secretName: tls-secret
  rules:
    - host: kubia.example.com
      http:
        paths:
          - path: /
            backend:
              serviceName: kubia-nodeport
              servicePort: 80

````

````

```yaml

````

```yaml

````


```text
???


```
