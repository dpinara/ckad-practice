> **Pod**
```bash
k run pod1 \
    -o yaml \
    --dry-run=client \
    --image=busybox \
    --env="USER=dpinara" \                      # Unlike label We have to mention each env separately..
    --env="USER_ACCESS=pw"" \
    --labels="USER=dpinara,USER_ACCESS=pw" \    # We can use multiple label comma separated..
    --requests "cpu=100m,memory=256Mi" \
    --limits "cpu=200m,memory=512Mi" \
    --command \
    -- sh -c "sleep 1d"
    
k run pd --image=busybox  -o yaml --dry-run=client --env=USER=dpinara --env=USER_ACCESS=pw --port=8080 --labels="USER=dpinara,USER_ACCESS=pw" -- sleep 3000

```
````text
The number of consecurtive successes is configured via the successThreshold field, and the number of consecutive failures required to tranisition from success to failure is failureThreshold. The probe runs every periodSeconds and each probe will wait up to timeoutSeconds to complete.

````

##### Note - -- for command has to be in last 


```text
Notice that the nginx-container has started however there are no messages in the log that indicate that it's actually running so we'll check this by mapping port 19999 on the local machine to port 80 in the pod-b pod, which should point to the nginx-container which has the Nginx web server running on port 80.
kubectl port-forward pods/pod-b 19999:80 --namespace ggckad-s0
When we execute the line above, the output should look like what we have below:

Finally, we can now test that Nginx is running by browsing localhost:19999.

```

```yaml
ubuntu@ip-10-0-128-5:~$ kubectl run pd \
  --image=busybox  -o yaml \
  --dry-run=client \
  --env=USER=dpinara --env=USER_ACCESS=pw \
  --port=8080 \
  --labels="USER=dpinara,USER_ACCESS=pw" \
  --serviceaccount='sa' 
  -- sh -c "sleep 3000"

apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    USER: dpinara
    USER_ACCESS: pw
  name: pd
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 3000
    env:
    - name: USER
      value: dpinara
    - name: USER_ACCESS
      value: pw
    image: busybox
    name: pd
    ports:
    - containerPort: 8080
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  serviceAccountName: sa
status: {}
ubuntu@ip-10-0-128-5:~$ 

```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: webserver
spec:
  securityContext: # insert this line
    runAsUser: 101 # UID for the user
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: accelerator
                operator: In
                values:
                  - nvidia-tesla-p100
  initContainers: #
    - args: #
        - /bin/sh #
        - -c #
        - wget -O /work-dir/index.html http://neverssl.com/online #
      image: busybox 
      securityContext: 
        capabilities: 
          add: ["NET_ADMIN", "SYS_TIME"] 
      name: box 
      volumeMounts: 
        - name: vol 
          mountPath: /work-dir 
  containers:
  - name: mycontainer
    image: nginx:latest
    imagePullPolicy: IfNotPresent
    resources:
      requests:
        memory: "128Mi" # 128Mi = 128 mebibytes
        cpu: "500m"     # 500m = 500 milliCPUs (1/2 CPU)
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 80
    volumeMounts:
      - name: vol
        mountPath: /usr/share/nginx/html
    livenessProbe:
      initialDelaySeconds: 5 
      periodSeconds: 5 
      exec: 
        command: 
          - ls 
    readinessProbe: 
      httpGet: 
        path: / 
        port: 80 
        
  volumes:
    - name: vol
      emptyDir: {}
  nodeSelector: 
    accelerator: nvidia-tesla-p100 

  # Multi containers...
  - name: mycontainer2
    image: nginx:latest
````

````text
#At times we can not get details from get, describe, log, then we can try doing get -o yaml and checking lastState. Here it says that pod crashed due ot OOM
 
    lastState:
      terminated:
        containerID: docker://0eb3e478cff2492b7fbcdeafb4584ca48702637b92d8c5ef6c84e2cebebfc708
        exitCode: 1
        finishedAt: "2021-01-01T10:11:01Z"
        reason: OOMKilled
        startedAt: "2021-01-01T10:11:01Z"
````

````text

Create and run a particular image in a pod.

Examples:
# Start a nginx pod.
kubectl run nginx --image=nginx

# Start a hazelcast pod and let the container expose port 5701.
kubectl run hazelcast --image=hazelcast/hazelcast --port=5701

# Start a hazelcast pod and set environment variables "DNS_DOMAIN=cluster" and "POD_NAMESPACE=default" in the container.
kubectl run hazelcast --image=hazelcast/hazelcast --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default"

# Start a hazelcast pod and set labels "app=hazelcast" and "env=prod" in the container.
kubectl run hazelcast --image=hazelcast/hazelcast --labels="app=hazelcast,env=prod"

# Dry run. Print the corresponding API objects without creating them.
kubectl run nginx --image=nginx --dry-run=client

# Start a nginx pod, but overload the spec with a partial set of values parsed from JSON.
kubectl run nginx --image=nginx --overrides='{ "apiVersion": "v1", "spec": { ... } }'

# Start a busybox pod and keep it in the foreground, don't restart it if it exits.
kubectl run -i -t busybox --image=busybox --restart=Never

# Start the nginx pod using the default command, but use custom arguments (arg1 .. argN) for that command.
kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>

# Start the nginx pod using a different command and custom arguments.
kubectl run nginx --image=nginx --command -- <cmd> <arg1> ... <argN>

Options:
--allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in the template. Only applies to golang and jsonpath output formats.
--attach=false: If true, wait for the Pod to start running, and then attach to the Pod as if 'kubectl attach ...' were called.  Default false, unless '-i/--stdin' is set, in which case the default is true. With '--restart=Never' the exit code of the container process is returned.
--cascade=true: If true, cascade the deletion of the resources managed by this resource (e.g. Pods created by a ReplicationController).  Default true.
--command=false: If true and extra arguments are present, use them as the 'command' field in the container, rather than the 'args' field which is the default.
--dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be sent, without sending it.If server strategy, submit server-side request without persisting the resource.
--env=[]: Environment variables to set in the container.
--expose=false: If true, service is created for the container(s) which are run
--field-manager='kubectl-run': Name of the manager used to track field ownership.
-f, --filename=[]: to use to replace the resource.
--force=false: If true, immediately remove resources from API and bypass graceful deletion. Note that immediate deletion of some resourcesmay result in inconsistency or data loss and requires confirmation.
--grace-period=-1: Period of time in seconds given to the resource to terminate gracefully. Ignored if negative. Set to 1 for immediate shutdown. Can only be set to 0 when --force is true (force deletion).
--hostport=-1: The host port mapping for the container port. To demonstrate a single-machine container.
--image='': The image for the container to run.
--image-pull-policy='': The image pull policy for the container. If left empty, this value will not be specified by the client and defaulted by the server
-k, --kustomize='': Process a kustomization directory. This flag can't be used together with -f or -R.
-l, --labels='': Comma separated labels to apply to the pod(s). Will override previous values.
--leave-stdin-open=false: If the pod is started in interactive mode or with stdin, leave stdin open after the first attach completes. By default, stdin will be closed after the first attach completes.
--limits='': The resource requirement limits for this container.  For example, 'cpu=200m,memory=512Mi'.  Note that server side components may assign limits depending on the server configuration, such as limit ranges.
-o, --output='': Output format. One of: json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
--overrides='': An inline JSON override for the generated object. If this is non-empty, it is used to override the generated object. Requires that the object supply a valid apiVersion field.
--pod-running-timeout=1m0s: The length of time (like 5s, 2m, or 3h, higher than zero) to wait until at least one pod is running
--port='': The port that this container exposes.
--privileged=false: If true, run the container in privileged mode.
--quiet=false: If true, suppress prompt messages.
--record=false: Record current kubectl command in the resource annotation. If set to false, do not record the command. If set to true, record the command. If not set, default to updating the existing annotation value only if one already exists.
-R, --recursive=false: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
--requests='': The resource requirement requests for this container.  For example, 'cpu=100m,memory=256Mi'.  Note that server side components may assign requests depending on the server configuration, such as limit ranges.
--restart='Always': The restart policy for this Pod.  Legal values [Always, OnFailure, Never].
--rm=false: If true, delete resources created in this command for attached containers.
--save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
--serviceaccount='': Service account to set in the pod spec.
-i, --stdin=false: Keep stdin open on the container(s) in the pod, even if nothing is attached.
--template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
--timeout=0s: The length of time to wait before giving up on a delete, zero means determine a timeout from the size of the object
-t, --tty=false: Allocated a TTY for each container in the pod.
--wait=false: If true, wait for resources to be gone before returning. This waits for finalizers.

Usage:
kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
controlplane $
````

```text
```
