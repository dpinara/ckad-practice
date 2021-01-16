> **job**
```bash
# When we create job or cronjob internally it creates pods.

```
```text
backoffLimit: Number of times a Job will retry before marking a Job as failed
completions: Number of Pod completions the Job needs before being considered a success. If completions value is 6, than six pods to be created ( which ends susscessfully) before marking that job complete.
parallelism: Number of Pods the Job is allowed to run in parallel
spec.template.spec.restartPolicy: Job Pods default to never attempting to restart. Instead the Job is responsible for managing the restart of failed Pods.

```


```YAML
```

```YAML
```

```text
Create a job with the specified name.

Examples:
  # Create a job
  kubectl create job my-job --image=busybox

  # Create a job with command
  kubectl create job my-job --image=busybox -- date

  # Create a job from a CronJob named "a-cronjob"
  kubectl create job test-job --from=cronjob/a-cronjob

Options:
      --allow-missing-template-keys=true: If true, ignore any errors in templates when a field or map key is missing in
the template. Only applies to golang and jsonpath output formats.
      --dry-run='none': Must be "none", "server", or "client". If client strategy, only print the object that would be
sent, without sending it. If server strategy, submit server-side request without persisting the resource.
      --field-manager='kubectl-create': Name of the manager used to track field ownership.
      --from='': The name of the resource to create a Job from (only cronjob is supported).
      --image='': Image name to run.
  -o, --output='': Output format. One of:
json|yaml|name|go-template|go-template-file|template|templatefile|jsonpath|jsonpath-as-json|jsonpath-file.
      --save-config=false: If true, the configuration of current object will be saved in its annotation. Otherwise, the
annotation will be unchanged. This flag is useful when you want to perform kubectl apply on this object in the future.
      --template='': Template string or path to template file to use when -o=go-template, -o=go-template-file. The
template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate=true: If true, use a schema to validate the input before sending it

Usage:
  kubectl create job NAME --image=image [--from=cronjob/name] -- [COMMAND] [args...] [options]

Use "kubectl options" for a list of global command-line options (applies to all commands).
```
