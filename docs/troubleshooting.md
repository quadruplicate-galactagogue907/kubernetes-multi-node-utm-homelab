```
# Troubleshooting

## 1. SSH key login failed on AlmaLinux

### Symptom
Passwordless SSH login failed even after using `ssh-copy-id`.

### Root cause
The Alma VM was running in **FIPS mode**, and the server rejected the ED25519 key type.

### Fix
Generated and used an RSA 4096-bit key instead.

---

## 2. Version mismatch between nodes

### Symptom
Ubuntu and Alma were running different MicroK8s versions.

### Root cause
Alma was installed on a newer channel than Ubuntu.

### Fix
Removed and reinstalled MicroK8s on Alma using the same channel as Ubuntu:

```
sudo snap install microk8s --classic --channel=1.32/stable

3. Calico stuck on the Alma worker
Symptom

The worker joined the cluster, but the Calico pod stayed at Init:0/2.

What I checked
firewall behavior
SELinux mode
kernel modules
sysctl settings
image pull behavior
MicroK8s logs
Calico pod events and describe output
Root cause

The Alma kubelet was configured with:

--resolv-conf=/run/systemd/resolve/resolv.conf

That file did not exist on the worker node, which caused pod sandbox creation to fail.
Fix
Changed kubelet to use:

--resolv-conf=/etc/resolv.conf

Then restarted MicroK8s on Alma.

4. Image pull investigation
Symptom
Calico init containers appeared stuck.
What I found
Image pulling was not the final root cause, but I still validated it manually by pulling Calico images on the worker.
This helped rule out:
* Docker Hub access problems
* containerd image pull failures

5. Noisy old workloads on Ubuntu
Symptom
Old broken workloads made MicroK8s behavior noisy and harder to read.
Fix
Cleaned up stale workloads and restarted MicroK8s components as needed during troubleshooting.
Lessons from troubleshooting
* a worker can join successfully while still being unusable for workloads
* resolver configuration can break pod sandbox creation
* FIPS mode changes SSH key behavior
* version consistency matters in multi-node labs
* systematic troubleshooting is critical in Kubernetes and Linux labs

