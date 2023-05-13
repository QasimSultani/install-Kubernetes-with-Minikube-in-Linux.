Setting up a Local Kubernetes Cluster with Minikube
This guide will walk you through the process of setting up a local Kubernetes cluster using Minikube on Linux.

Prerequisites
A Linux machine with root access
Docker installed on the machine
kubectl installed on the machine
Installation
Install Docker
Update the package list and install Docker using the following commands:

sh
Copy code
sudo apt update
sudo apt -y install docker.io
Install kubectl
Install kubectl by downloading the latest release from the Kubernetes repository and moving it to the /usr/local/bin/ directory:

sh
Copy code
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
Install Minikube
Install Minikube by downloading the latest release from the Minikube repository and moving it to the /usr/local/bin/ directory:

sh
Copy code
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
sudo mv minikube /usr/local/bin/
Start Minikube
Start Minikube by running the following commands:

sh
Copy code
sudo apt install conntrack
minikube start --vm-driver=none
Note: Starting Minikube with --vm-driver=none runs Kubernetes on the host machine instead of in a virtual machine.

Install cri-dockerd
Minikube requires cri-dockerd to communicate with Docker. Install cri-dockerd by running the following commands:

sh
Copy code
git clone https://github.com/Mirantis/cri-dockerd.git
cd cri-dockerd
wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile
mkdir bin
go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket
Install cri-tool
Install cri-tool by running the following commands:

sh
Copy code
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.25.0/crictl-v1.25.0-linux-amd64.tar.gz
sudo tar zxvf crictl-v1.25.0-linux-amd64.tar.gz -C /usr/local/bin
Verification
Check the status of Minikube by running the following command:

sh
Copy code
minikube status
You should now have a fully functional local Kubernetes cluster running on your Linux machine.
