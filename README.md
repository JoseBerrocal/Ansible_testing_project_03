# Ansible_testing_project_03
Deploy jenkins in a EC2 instance using an ansible-playbook. The following repos will be used:
- https://github.com/geerlingguy/ansible-role-java.git
- https://github.com/geerlingguy/ansible-role-jenkins.git


# Overview

The main focus for this project is to prepare the EC2 instance with the required software to deploy jenkins, for this purpose we will install ansible in a EC2 instance to later install remotely jenkins in another EC2 instance


### Project Tasks

This project goal is to do a small example how to use this technology:
* Create a new user
* Provide roles to the new user
* Deploy infraestructure: 
    - VCP
    - Subnet
    - IG
* Install two EC2 instance
* Install ansible in the first EC2 instace (ansible-testing1)
* Install java and jenkins software in the second EC2 instace (jenkins-ec2-testing1) using ansible playbook from the ansible tower

## Pre-requisites

1. Create the user
2. Create a policy with the permissions to create an EC2 isntance
3. Configure the user in aws cli in your standalone environment
4. Create a KeyPair (For this case I use "jb_aws_keypair.pem")

## Instructions

### 1. Running the infraestructure from your local environment

a. Configure the user in your aws cli

b. Use cloudformation to create the infraestructure and the ec2 instances

c. Connect to the EC2 instance and clone this repository in the EC2 instance
```bash
cd Ansible_testing_project_03
cp -p /path_where_key_pairs_is_located/jb_aws_keypair.pem .
chmod 400 jb_aws_keypair.pem
# For the following command it will be required to update the Private IPv4 DNS of the EC2 instace (ansible-testing1) 
ssh -i "jb_aws_keypair.pem" ubuntu@ec2-56-23-79-154.us-west-2.compute.amazonaws.com
# Once inside, clone the repository
git clone https://github.com/JoseBerrocal/Ansible_testing_project_03.git
```

d. Install Ansible
```bash
cd Ansible_testing_project_03
sh install_ansible.sh
```

e. From the local environmet copy jb_aws_keypair.pem to the EC2 instace (ansible-testing1)
```bash
scp -i "jb_aws_keypair.pem" /path_where_key_pairs_is_located/jb_aws_keypair.pem ubuntu@ec2-56-23-79-154.us-west-2.compute.amazonaws.com:/home/ubuntu
# Once in the EC2 instace (ansible-testing1)
chmod 400 jb_aws_keypair.pem
```

e. In the EC2 instace (ansible-testing1) modify the Public IPv4 DNS of (jenkins-ec2-testing1) in Ansible_testing_project_03/jenkins_ansible/Inventory stablish a connection to (jenkins-ec2-testing1) using the *Private* IPv4 DNS of the destination
```bash
vim Ansible_testing_project_03/jenkins_ansible/Inventory
ssh -i "jb_aws_keypair.pem" ubuntu@ec2-192-168-1-162.us-west-2.compute.amazonaws.com
exit
```

f. Can test before executing the connection is stablished
```bash
ubuntu@ip-192-168-1-96:~/Ansible_testing_project_03/jenkins_ansible$ ansible jenkins_servers -i Inventory -m ping
ip-192-168-1-162.us-west-2.compute.internal | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    }, 
    "changed": false, 
    "ping": "pong"
}
ubuntu@ip-192-168-1-96:~/Ansible_testing_project_03/jenkins_ansible$ 
```

f. Run ansible playbool
```bash
cd Ansible_testing_project_03/jenkins_ansible
ansible-playbook -i Inventory main.yaml
```

d. Access the URL of the EC2 instance (jenkins-ec2-testing1), the output can be the following
![alt text](https://github.com/JoseBerrocal/Ansible_testing_project_03/blob/master/images/ansible_project_03_outoput.png)


## Enhancements

To improve the project in will be required manually create the ansible playbook to install three different softwares