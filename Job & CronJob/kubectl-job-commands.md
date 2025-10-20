# kubectl Job & CronJob Commands for CKAD

## Job Commands

### Create and Manage Jobs

```bash
# Create job from image
k create job hello --image=busybox -- echo "Hello World"
k create job pi-calc --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'

# Create job from YAML
k apply -f job.yaml

# Get jobs
k get jobs
k get job hello -o wide
k describe job hello
```

### Monitor Job Execution

```bash
# Check job status
k get jobs -w
k get pods -l job-name=hello

# View job logs
k logs job/hello
k logs -l job-name=hello

# Follow job logs
k logs job/hello -f
```

### Job Cleanup

```bash
# Delete job (and its pods)
k delete job hello

# Delete job but keep pods
k delete job hello --cascade=orphan

# Clean up completed jobs
k get jobs --field-selector status.successful=1
k delete jobs --field-selector status.successful=1
```

## CronJob Commands

### Create and Manage CronJobs

```bash
# Create cronjob
k create cronjob hello --image=busybox --schedule="*/1 * * * *" -- echo "Hello from CronJob"
k create cronjob backup --image=mysql:5.7 --schedule="0 2 * * *" -- mysqldump -u root mydb

# Create from YAML
k apply -f cronjob.yaml

# Get cronjobs
k get cronjobs
k get cj                    # Short name
k describe cronjob hello
```

### Monitor CronJob Execution

```bash
# Check cronjob status
k get cronjobs -w
k get jobs -l cronjob=hello

# View cronjob history
k get jobs --sort-by=.metadata.creationTimestamp
k get pods -l job-name=hello-<timestamp>

# Check logs from cronjob runs
k logs -l job-name=hello-27789456
```

### CronJob Management

```bash
# Suspend cronjob (stop scheduling)
k patch cronjob hello -p '{"spec":{"suspend":true}}'

# Resume cronjob
k patch cronjob hello -p '{"spec":{"suspend":false}}'

# Manually trigger cronjob
k create job hello-manual --from=cronjob/hello
```

### CronJob Cleanup

```bash
# Delete cronjob
k delete cronjob hello

# Clean up old jobs from cronjob
k delete jobs -l cronjob=hello

# Set job history limits (in YAML)
# successfulJobsHistoryLimit: 3
# failedJobsHistoryLimit: 1
```

## CKAD Exam Patterns

### Quick Job Creation

```bash
# Generate job YAML
k create job test --image=nginx --dry-run=client -o yaml > job.yaml
k create cronjob test --image=nginx --schedule="0 */6 * * *" --dry-run=client -o yaml > cronjob.yaml
```

### Troubleshooting Jobs

```bash
# Check job failure reasons
k describe job <job-name>
k get events --field-selector involvedObject.name=<job-name>
k logs job/<job-name>

# Check pod status for job
k get pods -l job-name=<job-name>
k describe pod <pod-name>
```
