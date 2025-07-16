juice-shop-app
Kubernetes Juice Shop Demo on CentOS

This repository contains step-by-step setup for a minimal Kubernetes cluster on CentOS using kubeadm, and deployment of the OWASP Juice Shop application via NGINX Ingress.
Structure

    manifests/: YAMLs for deployment, service, ingress
    cluster-setup/: Instructions for setting up master/worker and securing the API
    monitoring-recommendation.md: Tools to monitor the app and cluster

Requirements

    CentOS nodes
    containerd installed
    kubelet, kubeadm, kubectl installed

Usage

    Follow cluster-setup to bootstrap the cluster
    Apply YAMLs in manifests/
    Access Juice Shop via Ingress using /etc/hosts entry for juice.local
