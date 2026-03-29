# Architecture

## Overview

This homelab was built on **UTM** running on macOS and consists of two Linux virtual machines:

- **Ubuntu 24.04.3 LTS** as the MicroK8s control plane
- **AlmaLinux 9.7** as the MicroK8s worker node

The cluster uses:

- **MicroK8s v1.32.13**
- **containerd**
- **Calico CNI**

## Components

### Host
- macOS
- UTM hypervisor

### Virtual Machines
- **devlabubuntu** — control plane
- **alma-k8s-worker-1** — worker

## Networking Design

The lab used two networking ideas during setup:

### 1. Host-to-VM access
Used for SSH access from the Mac host into the VMs through UTM port forwarding.

Examples:
- Ubuntu SSH forwarded through host port `2222`
- Alma SSH forwarded through host port `2223`

### 2. VM-to-VM communication
Used for Kubernetes node communication between Ubuntu and Alma.

Observed cluster-facing IPs:
- Ubuntu: `10.0.2.15`
- Alma: `10.0.2.20`

A shared VM network was also used during setup and troubleshooting to verify node-to-node reachability.

## Cluster Roles

### Ubuntu Control Plane
Responsible for:
- cluster management
- API access
- scheduling
- running core Kubernetes components

### Alma Worker Node
Responsible for:
- joining the cluster
- running scheduled workloads
- participating in cluster networking through Calico

## Workload Validation

A sample `nginx` deployment was used to validate:

- cluster health
- pod scheduling
- worker participation
- service creation
- endpoint population

## Key Notes

- both nodes were aligned to **MicroK8s v1.32.13**
- the worker node required extra troubleshooting before Calico became healthy
- the final lab state successfully ran workloads across both nodes
