# Kubernetes Juice Shop Demo on CentOS

This repository contains step-by-step setup for a minimal Kubernetes cluster on CentOS using kubeadm, and deployment of the OWASP Juice Shop application via NGINX Ingress.

## Structure
- **manifests/**: YAMLs for deployment, service, ingress
- **cluster-setup/**: Instructions for setting up master/worker and securing the API
- **monitoring-recommendation.md**: Tools to monitor the app and cluster

## Requirements
- CentOS 7 or 8 nodes
- containerd installed
- kubelet, kubeadm, kubectl installed

## Usage
1. Follow cluster-setup to bootstrap the cluster
2. Apply YAMLs in `manifests/`
3. Access Juice Shop via Ingress using `/etc/hosts` entry for `juice.local`
