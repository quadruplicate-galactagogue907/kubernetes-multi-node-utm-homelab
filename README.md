# Kubernetes Multi-Node UTM Homelab

A two-node MicroK8s homelab running on UTM with:

- **Ubuntu 24.04.3 LTS** as the control plane
- **AlmaLinux 9.7** as the worker node
- **MicroK8s v1.32.13**
- **containerd**
- **Calico CNI**

This lab was built for **KCNA prep**, Linux troubleshooting practice, and future expansion into observability and AIOps experiments.

## Project Goals

- build a realistic multi-node Kubernetes lab on local VMs
- practice Linux, networking, SSH, and cluster troubleshooting
- validate worker-node joining and cross-node scheduling
- document real problems encountered and how they were fixed

## Architecture

- **Host machine:** macOS
- **Hypervisor:** UTM
- **Control plane VM:** Ubuntu
- **Worker VM:** AlmaLinux
- **Kubernetes distro:** MicroK8s
- **Container runtime:** containerd
- **CNI:** Calico

## Lab Topology

- **Control plane:** `devlabubuntu`
- **Worker:** `alma-k8s-worker-1`

Observed node IPs in the cluster:

- Ubuntu: `10.0.2.15`
- Alma: `10.0.2.20`

Shared lab networking was also used during setup for node-to-node reachability.

## What I Built

- configured a **multi-node MicroK8s cluster** on UTM
- joined a fresh **AlmaLinux worker** to an existing **Ubuntu control plane**
- aligned both nodes to the same **MicroK8s version (`v1.32.13`)**
- validated cluster health with:
  - node readiness checks
  - system pod checks
  - nginx deployment scheduling across nodes
  - service and endpoint checks

## Key Challenges Encountered

### 1) SSH key login failed on AlmaLinux
I initially set up passwordless SSH using my usual ED25519 key, but Alma rejected it.

**Root cause:** the Alma VM was running in **FIPS mode**, which did not accept my ED25519 key type.

**Fix:** I generated and used an **RSA 4096-bit key** for the Alma VM.

---

### 2) Worker node joined, but Calico stayed stuck
The worker joined the cluster, but the Calico pod on Alma was stuck in `Init:0/2`, which prevented workloads from running correctly on the worker.

**Checks performed:**
- verified image pulling
- checked kernel modules and sysctl settings
- checked firewalld and SELinux behavior
- reviewed MicroK8s containerd and kubelite logs

**Root cause:** kubelet on Alma was pointing to a resolver file that did not exist:

```text
/run/systemd/resolve/resolv.conf

This caused pod sandbox creation to fail.

Fix: updated the kubelet resolver path to:

/etc/resolv.conf

After restarting MicroK8s, Calico came up successfully on Alma.

3) Version mismatch between nodes

The Ubuntu control plane was on v1.32.13, while Alma was initially installed on a newer MicroK8s version.

Fix: removed and reinstalled MicroK8s on Alma using the same channel as Ubuntu:

1.32/stable

Validation Performed

The following checks were used to validate the lab:

microk8s kubectl get nodes -o wide
microk8s kubectl get pods -A -o wide
microk8s kubectl get pods -o wide
microk8s kubectl get svc
microk8s kubectl get endpoints nginx-test

Workload validation:

microk8s kubectl create deployment nginx-test --image=nginx
microk8s kubectl scale deployment nginx-test --replicas=2
microk8s kubectl get pods -o wide

Validation Results
both nodes reached Ready
both nodes matched on MicroK8s v1.32.13
Calico was healthy on both nodes
nginx pods were scheduled successfully
workload ran on:
Ubuntu control plane
Alma worker node
nginx-test service was created successfully
service endpoints were populated successfully
Screenshots

See the screenshots/ folder for validation evidence:

01-main-validation-cluster-state.png
02-alma-worker-runtime-proof.png
03-nginx-pod-details.png
04-service-and-endpoints.png
Project Structure

kubernetes-multi-node-utm-homelab/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── setup-steps.md
│   ├── troubleshooting.md
│   └── validation.md
├── manifests/
│   └── nginx-test.yaml
└── screenshots/

Lessons Learned
FIPS-enabled systems may reject ED25519 keys even when SSH is otherwise configured correctly
version alignment matters in multi-node MicroK8s labs
a node can join successfully while CNI is still unhealthy
kubelet DNS resolver configuration can break pod sandbox creation
troubleshooting a real lab teaches much more than a clean one-command setup
Next Improvements
force node registration to use the shared-network IPs instead of 10.0.2.x
add a cleaner service validation with both worker and control-plane backends active at the same time
deploy a sample app through ingress
add observability tooling such as Prometheus and Grafana
automate lab setup with shell scripts or Ansible
extend the lab into AIOps experiments later
Why This Project Matters

This homelab gave me hands-on experience with:

Linux administration
SSH hardening and authentication troubleshooting
virtualization and VM networking
Kubernetes clustering concepts
MicroK8s operations
real-world troubleshooting across nodes

It also serves as a practical study environment for KCNA and a foundation for future Kubernetes and platform engineering experiments.
