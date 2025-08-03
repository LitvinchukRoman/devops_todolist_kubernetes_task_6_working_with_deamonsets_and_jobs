# Deployment Instructions

## Prerequisites
- Kubernetes cluster
- Namespace `mateapp` must exist
- `kubectl` configured and connected to your cluster

## Deploy DaemonSet

```sh
kubectl apply -f daemonset.yml

Deploy CronJob

kubectl apply -f cronjob.yml

Validation Instructions

Check DaemonSet logs

kubectl get pods -n mateapp -l app=curl-daemon
kubectl logs -n mateapp <daemon-pod-name>

Logs should show continuous curl requests every 5 seconds to the todoapp service.

Check CronJob logs

Wait 4+ minutes, then:

kubectl get jobs -n mateapp
kubectl get pods -n mateapp --selector=job-name=todoapp-healthcheck-<timestamp>
kubectl logs -n mateapp <cronjob-pod-name>

Logs should show the result of curl http://todoapp/api/health.

⸻

Notes
	•	Ensure todoapp is exposed via ClusterIP and accessible within the mateapp namespace.
	•	You can monitor events with:

kubectl describe daemonset todoapp-curl-daemon -n mateapp
kubectl describe cronjob todoapp-healthcheck -n mateapp