[Go To Home](/learning-cloud-k8s)

[Go To Previous Step (Docker Setup)](/learning-cloud-k8s/docker-setup)
## Setup minikube on Ubuntu
### Prerequisites
- 2 CPUs or more
- 2GB of free memory
- 20GB of free disk space
- Internet connection
- Container or virtual machine manager, such as: `Docker, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMWare`

### Machine environment
*Local environment might matter for some shell commands*
- AWS EC2 instance
- OS Ubuntu 20.x
- Container - docker

### Setup steps
*All commands mentioned in the guide are executed inside post doing SSH into AWS EC2 ubuntu instance*

1. Download and install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

2. Start minikube cluster
```bash
minikube start
```

### References
Just a couple of steps are mentioned here to show how simple it's to start a minikube cluster. There are many commands, most of which are executed using `kubectl`.

Follow [minikube start](https://minikube.sigs.k8s.io/docs/start/) for details.

[Go To Next Step (Docker Setup)](/learning-cloud-k8s/kubectl-setup)
