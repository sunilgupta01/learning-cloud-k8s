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
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

2. Validate binary
```bash
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(<kubectl.sha256) kubectl" | sha256sum --check
```
- output
```
kubectl: OK
```

3. Install kubectl
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

4. Check that it is working fine
```bash
kubectl version --client
```
