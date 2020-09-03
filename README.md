# Ansible_testing_project_03
Deploy jenkins in a EC2 instance using a ansible tower. The following repos will be used:
- https://github.com/geerlingguy/ansible-role-java.git
- https://github.com/geerlingguy/ansible-role-jenkins.git


# Overview

The main focus for this project is to prepare the EC2 instance with the required software to deploy jenkins, for this purpose we will first create an ansible tower to later install the software in a EC2 instance


### Project Tasks

This project goal is to do a small example how to use this technology:
* Create a new user
* Provide roles to the new user
* Deploy infraestructure: 
    - VCP
    - Subnet
    - IG
* Install two EC2 instance
* Install ansible in the first EC2 instace (ansible tower)
* Install java and jenkins software in the secind EC2 instace using ansible playbook from the ansible tower

## Pre-requisites

1. Create the user
2. Create a policy with the permissions to create an EC2 isntance
3. Configure the user in aws cli in your standalone environment
4. Create a KeyPair (For this case I use "jb_aws_keypair.pem")

## Instructions

### 1. Running the infraestructure in a EC2 instance

a. Configure the user in your aws cli

b. Use cloudformation to create the infraestructure and the ec2 instances

c. Connect to the EC2 instance and clone this repository in the EC2 instance
```bash
chmod 400 jb_aws_keypair.pem
ssh -i "jb_aws_keypair.pem" ubuntu@ec2-56-23-79-154.us-west-2.compute.amazonaws.com
git clone https://github.com/JoseBerrocal/Ansible_testing_project_03.git
```

d. Install Ansible
```bash
cd Ansible_testing_project_02
sh install_ansible.sh
```

e. Modify the IP in jenkins_ansible/Inventory and copy "jb_aws_keypair.pem" in the ansible tower

f. Run Ansible
```bash
cd jenkins_ansible
ansible-playbook -i Inventory main.yaml
```

d. Access the URL of the EC2 instance, the output can be the following
![alt text](https://github.com/JoseBerrocal/Ansible_testing_project_02/blob/master/images/ansible_project_02_outoput.png)


## Enhancements

To improve the project in willbe required to deploy the infraestructure usign terraform, deploy the ansible tower(EC2 instance) and install jenkins in a different EC2 instance