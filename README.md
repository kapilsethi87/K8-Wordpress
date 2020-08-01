# Kubernetes 

<img src="Images/K8_logo.png" width="150" hight="50">

Kubernetes is an open source platform for managing container technologies such as Docker.

Docker lets you create containers for a pre-configured image and application. Kubernetes provides the next step,
allowing you to balance loads between containers and run multiple containers across multiple systems

## Installation on Ubuntu

## Kubernetes Master Setup

Install Required Setups

```bash
apt-get install -y elinks,wget,net-tools
```
Start by disabling the swap memory

```bash
sudo swapoff -a
sed -i 's/^\(.*swap.*\)$/#\1/' /etc/fstab 
```

Set Hostname
```bash
sudo hostnamectl set-hostname master-node
```

Update the package list with the command:

```python
sudo apt-get update
```

Next, install Docker with the command:
```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Add Docker’s official GPG key:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add Docker Repo
```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Install the latest version of Docker Engine and containerd
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Check the installation (and version) by entering the following:
```bash
docker version
```

The product_uuid can be checked by using the command 
```bash
sudo cat /sys/class/dmi/id/product_uuid
```

## Start and Enable Docker

Set Docker to launch at boot by entering the following:
```bash
sudo systemctl enable docker
```

Verify Docker is running:
```bash
sudo systemctl status docker
```

Install Kubernetes
```bash
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
```bash
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

Verify the installation with:
```bash
kubeadm version
kubectl version --short
```

Initialize Kubernetes on Master Node
```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

Enter the following to create a directory for the cluster: To start using your cluster, you need to run the following as a regular user:
```bash
sudo mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Now check to see if the kubectl command is activated.
```bash
kubectl get nodes
```

Deploy Pod Network to Cluster
```bash
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

Verify that everything is running and communicating:
```bash
kubectl get pods --all-namespaces				
kubectl get -o wide pods --all-namespaces 
```


[LinuxDevOps.in](http://linuxdevops.in)
