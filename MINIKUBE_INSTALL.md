## Minikube installation process on WSL2
### Install Docker
```
# Install prerequisites
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add official Docker PGP eky
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add apt repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install latest Docker CE
sudo apt-get update -y
sudo apt-get install -y docker-ce

# Add new 'docker' user
sudo usermod -aG docker $USER && newgrp docker
```
### Install minikube prerequisites
```
# Install systemctl hack
git clone https://github.com/DamionGans/ubuntu-wsl2-systemd-script.git
bash ubuntu-wsl2-systemd-script/ubuntu-wsl2-systemd-script.sh

# Install Conntrack
sudo apt install -y conntrack
```
### Install minikube
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x ./minikube
sudo mv ./minikube /usr/local/bin/
minikube config set driver docker
```
### Install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
```
### Start minikube
```
sudo service docker start
minikube start
```

### Test
```
kubectl get nodes -o wide
```
You should be able to see that single node cluster is up and running