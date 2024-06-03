# TKGS Control Plane Disk Cleanup

This project is designed to perform disk cleanup on the control plane nodes of Tanzu Kubernetes Grid Service (TKGS) guest clusters. The cleanup job is scheduled to run once a day at midnight, ensuring that disk space is managed efficiently on control plane nodes.

## Components

### 1. Namespace

A dedicated namespace `cleanup` for the resources related to this cleanup task. 

### 2. Service Account

A service account `cleanup-jobs-sa` in the `cleanup` namespace to manage the permissions required for this job.

### 3. Cluster Role

A cluster role `cleanup-jobs-role` to grant necessary permissions to list, get, watch, create, and delete pods and jobs.

### 4. Cluster Role Binding

A cluster role binding `cleanup-jobs-binding` to bind the `cleanup-jobs-role` to the `cleanup-jobs-sa` service account.

### 5. ConfigMap

A config map `cleanup-jobs-script` to store the cleanup script.

### 6. CronJob

A cron job `cleanup-jobs-cronjob` to run the cleanup script once a day at midnight.


## Note :- 
In the script-container section replace commands with actual commands for disk cleanup. The example commands include displaying disk usage information (df -h), removing old log files (rm -rf /var/log/*), and clearing temporary directories (rm -rf /tmp/*). You can customize these commands further based on your specific disk cleanup requirements.


## Steps to Set Up the Disk Cleanup Job

### Apply the Configurations 
To apply all configurations, run the following commands:

```bash
kubectl create namespace cleanup
kubectl apply -f serviceaccount.yaml
kubectl apply -f clusterrole.yaml
kubectl apply -f clusterrolebinding.yaml
kubectl apply -f configmap.yaml
kubectl apply -f cronjob.yaml
```

## Conclusion
This setup will ensure that the disk cleanup script runs once a day on all control plane nodes of the TKGS guest clusters, using a CronJob deployed on the Supervisor Cluster. Ensure you monitor the logs and status of the jobs to verify successful execution.