1. Set hostname and update /etc/hosts   
```
vim /etc/hosts
master-node-ip	hostname
worker-node-ip	hostname
```
2. Disable SELinux and swap
```
setenforce 0
sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=permissive/' /etc/selinux/config
swapoff -a
sed -i '/swap/d' /etc/fstab
```
3. Load kernel modules and apply sysctl settings
```
modprobe overlay
modprobe br_netfilter

cat <<EOF | tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sysctl --system
```
4. Install containerd and configure it
```
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum install -y containerd.io
containerd config default | tee /etc/containerd/config.toml
sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
systemctl enable --now containerd
```
5. Add Kubernetes YUM repo and install kubeadm, kubelet, kubectl
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.33/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet
```
6. Initialize Kubernetes cluster
```
kubeadm init --pod-network-cidr=10.244.0.0/16
```
Your Kubernetes control-plane has initialized successfully!
To start using your cluster, you need to run the following as a regular user:
```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Alternatively, if you are the root user, you can run:
```
  export KUBECONFIG=/etc/kubernetes/admin.conf
```
You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:
```
kubeadm join 192.168.88.136:6443 --token 3u7i84.m1c4iawvsv6d6aiz \
        --discovery-token-ca-cert-hash sha256:1263524ee5130968feea94fde855ecb360d5acce557a669f6757ed6d1bf538c1  
```
8. Apply flannel network plugin
```
kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
```
If you use custom podCIDR (not 10.244.0.0/16) you first need to download the above manifest and modify the network to match your one.
