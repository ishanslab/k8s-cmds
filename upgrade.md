### Upgrading the master node  (upgrading to v1.29.0)
```bash
kubectl drain controlplane --ignore-daemonsets

vim /etc/apt/sources.list.d/kubernetes.list

# Update the version in the URL to the next available minor release, i.e v1.29.
#deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /
# It was v1.28.0 so we changed it to v1.29.0
#deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /

apt update  

# TO find the latest kubeadm versions
apt-cache madison kubeadm
apt-get install kubeadm=1.29.0-1.1

kubeadm upgrade plan v1.29.0
kubeadm upgrade apply v1.29.0 

apt-get install kubelet=1.29.0-1.1
systemctl daemon-reload
systemctl restart kubelet
kubectl uncordon controlplane

kubectl get nodes  # Only master node will show at the latest version  
```

### Upgrading the worker node  
```bash
kubectl drain node-1

ssh node-1

vim /etc/apt/sources.list.d/kubernetes.list

# Update the version in the URL to the next available minor release, i.e v1.29.
#deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /
# It was v1.28.0 so we changed it to v1.29.0
#deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /

apt update  

# TO find the latest kubeadm versions
apt-cache madison kubeadm
apt-get install kubeadm=1.29.0-1.1

# Upgrade the node 
kubeadm upgrade node

apt-get install kubelet=1.29.0-1.1
systemctl daemon-reload
systemctl restart kubelet
kubectl uncordon node-1

kubectl get nodes  # selected worker node will show at the latest version  
```
