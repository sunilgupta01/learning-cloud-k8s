[Go to home](/learning-cloud-k8s)

[Go To Previous Step (AWS EC2 Setup)](/learning-cloud-k8s/aws-ec2-setup)
## Setup docker on Ubuntu
### Machine environment
*Local environment might matter for some shell commands*
   - AWS EC2 instance
   - OS Ubuntu 20.x

### Setup steps
*All commands mentioned in the guide are executed inside post doing SSH into AWS EC2 ubuntu instance*

1. Install/configure common/global repositories for getting software
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
2. Install docker public key
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
3. Set up the stable repository (not nightly or test)
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
4. Update the apt package index
```bash
sudo apt-get update
```
5. Install the latest version of Docker Engine and containerd (not sure whether ce-cli and containerd were required.
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
6. Start the docker engine as a service
```bash
sudo service docker start
```
7. Give access on docker shell to local user
```bash
sudo usermod -aG docker $USER
```
   - re-login to the terminal post running the command. If this is not done, docker commands are available through sudo only
8. Run a sample hello-world app
```bash
docker run hello-world
```
   - Output, in case of success
   ```bash
   Hello from Docker!
   This message shows that your installation appears to be working correctly.
   ```

[Go To Next Step (Minikube Setup)](/learning-cloud-k8s/minikube-setup)
