# We can restrict the Kubenetes API access via:

1- Editing the kube-apiserver.yaml file
```
vim /etc/kubernetes/manifests/kube-apiserver.yaml
```
add the below flag:
- --allow-address=<YOUR_ALLOWED_IP>

# OR

2- Firewall Service

- Enable firewalld:
   systemctl enable --now firewalld

- Remove public access to API:
   firewall-cmd --permanent --remove-port=6443/tcp

- Allow specific IP:
   firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="YOUR.IP.ADD.RESS" port protocol="tcp" port="6443" accept'

- Allow internal cluster subnet:
   firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.0.0.0/8" port protocol="tcp" port="6443" accept'

- Reload rules:
   firewall-cmd --reload
 
