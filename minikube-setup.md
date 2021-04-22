[Go to home](/learning-cloud-k8s)
## Setup minikube on Ubuntu
### Machine environment
*Local environment might matter for some shell commands*
   - AWS EC2 instance
   - OS Ubuntu 20.x

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