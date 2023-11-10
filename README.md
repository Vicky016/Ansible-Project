# Ansible Configuration and Deployment Guide

This guide provides step-by-step instructions for setting up an Ansible machine and configuring three servers using Ansible playbooks. The infrastructure will consist of an Ansible machine and three EC2 instances where a website will be hosted. Security groups will be created for both the Ansible machine and the servers to ensure secure communication.

## Prerequisites
- AWS account with appropriate permissions
- AWS CLI installed and configured
- Ansible installed on the Ansible machine
- Basic understanding of Ansible concepts

## Steps

### 1. Create Security Groups

#### Ansible Machine Security Group
- **Name:** `ansible-sg`
- **Inbound Rules:** Allow SSH access from your IP address.

#### Server Security Group
- **Name:** `server-sg`
- **Inbound Rules:**
  - Allow SSH access from the Ansible machine security group (`ansible-sg`).
  - Allow HTTP access on port 80.

### 2. Launch Ansible Machine
- Launch an Amazon Linux 2 instance with the `ansible-sg` security group.

### 3. Create SSH Key Pair on Ansible Machine
```sh
# On the Ansible machine
ssh-keygen -t rsa -b 2048
```

### 4. Import Public Key into EC2 Console
- Copy the public key (`~/.ssh/id_rsa.pub`) and import it into the EC2 instances.

### 5. Launch EC2 Instances
- Launch three EC2 instances with the following configurations:
  - **Key Pair:** `ansible-pub-key`
  - **Security Group:** `server-sg`

### 6. Test Connection
```sh
# On the Ansible machine
ssh -i ~/.ssh/id_rsa ec2-user@<private-ip-of-ec2-instance>
```

### 7. Install Ansible on Ansible Machine
```sh
# On the Ansible machine
sudo yum update -y
sudo amazon-linux-extras install ansible2 -y
```

### 8. Create Inventory File
- Create a file named `inventory` with the private IP addresses of the three EC2 instances.

### 9. Create Ansible Playbook
- Create a playbook file named `deploy-techmax.yml` with the provided content.

### 10. Create Ansible Configuration File
- Create a file named `ansible.cfg` with the following content:
  ```ini
  [defaults]
  remote_user = ec2-user
  inventory = inventory
  private_key_file = ~/.ssh/id_rsa
  ```

### 11. Run Ansible Ping Module
```sh
# On the Ansible machine
ansible all --key-file ~/.ssh/id_rsa -i inventory -m ping -u ec2-user
```

### 12. Run Ansible Playbook
```sh
# On the Ansible machine
ansible-playbook deploy-techmax.yml
```

## Additional Notes
- Make sure your AWS credentials are properly configured on the Ansible machine.
- Adjust security group settings and key pair names as needed.
- The provided playbook deploys a sample website. Modify it according to your project requirements.

For a visual reference, watch the [YouTube video](https://youtu.be/Z6YGH0HVejE?si=jQ3S9ycAKlheEvmr) that complements this guide.

Congratulations! You have successfully set up an Ansible environment and configured three servers using Ansible playbooks.
