# Guide to create and access an AWS EC2 instance
### Local Machine Environment
*Local environment might matter for some shell commands*
   - Windows 10 Pro
   - OS Build 19042.928
   - Software installed: WSL 2
   - Software installed: Ubuntu 20.x

### Setup Steps
*All commands mentioned in the guide are tried on ubuntu shell*
1. `Create an AWS account` using an existing email ID.
   - Debit or credit card details will be needed to setup an account
2. `Add an user` for all the setup because root user should not be used for security purposes.
   - AWS Console > IAM > Users > Add user
     - Provide a `name`
     - Check `Programmatic access`
     - Keep `AWS Management Console access` unchecked
     - Attach existing policy with `AdministratorAccess` (for starters; in longer run, provide only required access)
     - On user creation, `Access key id` and `secret access key` are created. Save these locally for reference
3. `Install AWS CLI` on local
```bash 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
4. `Configure AWS CLI`
   - execute following command
   ```bash
   aws configure
   ```
   - It is an interactive command. When prompted provide following details:
   ```
   AWS Access Key ID [None]: <access key id>
   AWS Secret Access Key [None]: <secret access key>
   Default region name [None]: 
   Default output format [None]:
   ```
5. `Create key pair`
   - I created a key pair locally so that AWS does not know my private key
   - Alternatively, one can create it on AWS console under EC2.
   - This key pair is used to login to EC2 instances on AWS.
   - Using this approach user name and password are not asked.
   - Execute below command (I did not give any paraphrase.)
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   - Output
   ```
   Your identification has been saved in /home/username/.ssh/id_rsa
   Your public key has been saved in /home/username/.ssh/id_rsa.pub
   The key fingerprint is: SHA256:abcd usrname@pcname
   ```
6. `Import public key` generated in previous step on AWS
   - `AWS Console > Services > EC2 > Key Pairs > Actions > Import key pair`
   - Give a name to this key pair (say - awsec2key) that  will be used while calling AWS CLI for EC2 creation
   - Select public key file from local machine. It was under ubuntu WSL in my case.
   ```bash
   \\wsl$\Ubuntu\home\username\.ssh\id_rsa.pub
   ```
7. `Create EC2 instance` using AWS CLI
   - I created ubuntu server 20.x version in India with t3a.large (8GB RAM)
   - AMI IDs are different for same size but different regions.
   - Also provide volume size. For this AMI, default was 8 GB, which was not sufficient for working with minikube
   ```bash
   aws ec2 run-instances \
      --image-id ami-0d758c1134823146a \
      --instance-type t3a.large \
      --key-name awsec2key --region ap-south-1 \
      --block-device-mappings Ebs={VolumeSize=32},DeviceName=/dev/sda1
   ```
	- It creates a machine with an instance Id
8. `Update EC2 instance security group settings` [pending - check AWS command to do so]
   - `AWS Console > EC2 > Instance id > Security Group > Inbound rules`
     - Select `all traffic`
     - Add `my ip` from drop-down
9. `Connect to AWS EC2 instance`
    - Execute following command  
   ```bash
   ssh -i <private key file path> ubuntu@<public ip address>
   ```

### Optional Steps
1. `Allocate elastic IP` address so that with each restart new IP is not assigned
   - Execute following command (it provides a static IP)
   ```bash
   aws ec2 allocate-address --domain vpc --network-border-group ap-south-1
   ```
   - Alternatively, `AWS Console > EC2 > Network and Security > Elastic IPs > Allocate Elastic IP address > Allocate`
   - *this IP address will add to bill even when EC2 instance is down. Need to see about it*
2. `Associate static/elastic IP address` to the EC2 instance/VM
    - Execute following command
   ```bash
   aws ec2 associate-address --instance-id <ec2-istanceid> --public-ip <elastic ip address>
   ```
   *Post this association, one can access the EC2 instance using this IP address*

### Maintenance Steps
1. Check status of an EC2 instance
```bash
aws ec2 describe-instance-status --instance-ids my-instance-id
```
2. Start an EC2 instance
```bash
aws ec2 start-instances --instance-ids my-instance-id
```
3. Start an EC2 instance
```bash
aws ec2 stop-instances --instance-ids my-instance-id
```

### Notes
- For each account created on AWS, a `VPC` is created
- Each VPC has `three subnets` for `three availability zones` in a `region`
- Each VPC subnet has a `IPv4 CIDR block` that tells how many IP addresses are available. In total, around `65,536` IP addresses are available
- `Security Groups` are like `firewall`

### Yet to try
- Configure `AWS profile` (on local machine) to access different accounts?
- Check approach to `set security settings of EC2 instance through AWS CLI`