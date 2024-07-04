# k8s-learning
Kubernetes learning environment automation

## Create VM

Setup VM on local machine

### Add user to sudoers group and log out session

```
su root
/sbin/usermod -aG sudo $USER
pkill -KILL -u $USER
```

### Configure VM
```
sudo apt-get update
sudo apt-get dist-upgrade -y
```

### Enable copy paste into VM
```
sudo apt-get install -y spice-vdagent
pkill -KILL -u $USER
```

### Turn off swapping by creating a backup and editing the fstab file
```
sudo swapoff -a
sudo sed -i.bak '/swap/s/^/#/' /etc/fstab
```

### Installing QEMU
```
sudo apt-get install -y qemu-system swtpm-tools virt-manager 
```

### Adding K8S gpg-key
```
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

### Add K8S repository
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
```

## Install K8S kubectl
```
sudo apt-get update
sudo apt-get install -y kubectl
```

## Autocompletion for K8S kubectl
```
sudo apt-get install bash-completion
echo '' >>~/.bashrc
echo '# K8S' >>~/.bashrc
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

## K8S kubectl alias
```
echo 'alias k=kubectl' >>~/.bashrc
echo 'complete -o default -F __start_kubectl k' >>~/.bashrc
```

## K8S kubectl-convert
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
echo "$(cat kubectl-convert.sha256) kubectl-convert" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl-convert /usr/local/bin/kubectl-convert 
rm kubectl-convert kubectl-convert.sha256
```

## Download minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

## Autocompletion for K8S kubectl
```
sudo apt-get install bash-completion
echo '' >>~/.bashrc
echo '# minicube' >>~/.bashrc
echo 'source <(minicube completion bash)' >>~/.bashrc
```

## Starting minicube
```
minikube start
```
