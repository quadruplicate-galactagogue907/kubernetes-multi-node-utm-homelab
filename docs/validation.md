```
# Validation

## Cluster Validation Commands

```
microk8s kubectl get nodes -o wide
microk8s kubectl get pods -A -o wide
microk8s kubectl get pods -o wide
microk8s kubectl get svc
microk8s kubectl get endpoints nginx-test

Workload Validation Commands
```
microk8s kubectl create deployment nginx-test --image=nginx
microk8s kubectl scale deployment nginx-test --replicas=2
microk8s kubectl get pods -o wide
microk8s kubectl describe pod <nginx-pod-name>

What Was Validated
* both nodes reached Ready
* both nodes were aligned to MicroK8s v1.32.13
* Calico became healthy on both nodes
* the nginx deployment was scheduled successfully
* workloads ran on both Ubuntu and Alma
* a ClusterIP service was created successfully
* endpoints were created successfully
Validation Evidence
See the screenshots/ folder:
* 01-main-validation-cluster-state.png
* 02-alma-worker-runtime-proof.png
* 03-nginx-pod-details.png
* 04-service-and-endpoints.png
Notes
At one point during testing, the Alma VM was temporarily powered off to reduce host load. During that moment, endpoint output reflected only the currently available backends. Earlier validation had already confirmed successful workload scheduling on both nodes.



