# Setup Steps

## 1. Create the VMs in UTM

Create two Linux VMs:

- Ubuntu VM for the control plane
- AlmaLinux VM for the worker node

Recommended resources:
- 2 vCPUs minimum
- 4 GB RAM minimum
- 30+ GB disk

## 2. Configure SSH access

### Ubuntu
Configure SSH access from the Mac host using UTM port forwarding.

Example:
- host port `2222` -> guest port `22`

### Alma
Configure SSH access from the Mac host using a different UTM host port.

Example:
- host port `2223` -> guest port `22`

## 3. Set up passwordless SSH

Generate SSH keys on the Mac host and copy public keys to the VMs.

Important note from this lab:
- the Alma VM was FIPS-enabled
- ED25519 was rejected
- RSA 4096 worked successfully

## 4. Install MicroK8s

### On Ubuntu
Install MicroK8s and initialize the existing control plane.

### On Alma
Install MicroK8s with the same channel used by Ubuntu:

```bash
sudo snap install microk8s --classic --channel=1.32/stable

5. Join the worker to the cluster

From Ubuntu:

microk8s add-node

Copy the generated join command and run it on Alma.

6. Fix worker-side issues

During this lab, the worker joined successfully but still required troubleshooting before workloads could run correctly.

Issues resolved included:

FIPS-related SSH key rejection
version mismatch
Calico stuck in init state
kubelet using an invalid resolver file path

7. Validate the cluster

Run validation commands from Ubuntu:

microk8s kubectl get nodes -o wide
microk8s kubectl get pods -A -o wide
microk8s kubectl get svc
microk8s kubectl get endpoints nginx-test

8. Validate workload scheduling

Deploy a sample nginx workload:

microk8s kubectl create deployment nginx-test --image=nginx
microk8s kubectl scale deployment nginx-test --replicas=2
microk8s kubectl get pods -o wide

Confirm that pods can run on both the Ubuntu control plane and the Alma worker.


