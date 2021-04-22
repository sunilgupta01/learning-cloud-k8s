# AWS Setup
## this setup assumes setup on Linux based OS - ubuntu 20.x as on Apr 2021 (some steps might not be relevant in future)

1. Create an AWS account using an existing email ID.
   1. Debit or credit card details will be needed to setup an account
2. Add an user for all the setup because root user should not be used for security purposes.
   - Login to AWS console
   - AWS Console > IAM > Click on users > Add user
     - Provide a name
     - Check Programmatic access
     - No need to check AWS Management Console access
     - Attach existing policy with AdministratorAccess (for starters; in longer run, provide only required access)
     - On user creation, Access key id, secret access key are created. Save these locally for reference
3. Install AWS CLI on local
	```bash 
	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
	unzip awscliv2.zip
	sudo ./aws/install
	```
4. Configure AWS CLI
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
5. Create key pair
   - I created a key pair locally so that AWS does not know my private key
   - Alternatively, one can create it on AWS console under EC2.
   - This key pair is used to login to EC2 instances on AWS.
   - Using this approach user name and password are not asked.
   - Execute below command (I did not give any paraphrase.)
		```
		ssh-keygen -t rsa -b 4096
		```
   - Output
		```
		Your identification has been saved in /home/username/.ssh/id_rsa
		Your public key has been saved in /home/username/.ssh/id_rsa.pub
		The key fingerprint is: SHA256:abcd usrname@pcname
		```
6. Import public key generated in previous step on AWS
   - AWS Console > Services > EC2 > Key Pairs > Actions > Import key pair 
   - Give a name to this key pair (say - awsec2key) that  will be used while calling AWS CLI for EC2 creation
   - Select public key file from local machine (it was ubuntu WSL in my case `\\wsl$\Ubuntu\home\username\.ssh\id_rsa.pub`
7. Create EC2 instance using AWS CLI
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
8. Update EC2 instance security group settings [pending - check AWS command to do so]
   - AWS Console > EC2 > <Click on instance id> > Select Security Group > inbound rules
     - Select all traffic
     - add my ip
9. Allocate elastic IP address so that with each restart new IP is not assigned
   - Execute following command (it provides a static IP)
   ```
   aws ec2 allocate-address --domain vpc --network-border-group ap-south-1
   ```
   - Alternatively, AWS Console > EC2 > Network and Security > Elastic IPs > Allocate Elastic IP address > Allocate
10. Associate static/elastic IP address to the EC2 instance/VM
    - Execute following command
      `aws ec2 associate-address --instance-id <ec2-istanceid> --public-ip <elastic ip address>`
11. Connect to AWS Ubuntu machine
    - Execute following command  
		```
		ssh -i <private key file path> ubuntu@<elastic ip address>
		```

## Notes
- For each account created on AWS, a VPC is created
- Each VPC has three subnets for three availability zones
- Each VPC subnet has a IPv4 CIDR block that tells how many IP addresses are available. In total, around 65,536 IP addresses are available
- Security Groups are like firewall

## Yet to try
Configure AWS profile (on local machine) to access different accounts?
Check approach to set security settings of EC2 instance through AWS CLI