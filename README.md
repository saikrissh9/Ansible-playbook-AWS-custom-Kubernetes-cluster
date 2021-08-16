# Capstone project - Ansible playbook to deploy Wordpress & MYSQL in a custom Kubernetes cluster on AWS

Description:
 Creates Wordpress & MYSQL K8S cluster on AWS using EC2 instances, EIP, and NLB
   - creates SG group(name: lab), KeyPair(name:lab), EC2 instances, target group, and NLB(X:80 -> workers:32000) with EIP
   - Installs K8S cluster
   - Installs NFS server on K8S Master and uses '/html' '/mysql' directories on Master as persistant volume mounts for Wordpress & MySQL pods
   - Deploys Wordpress, MySQL (DB pass: sai) pods with persistent volumes and Nodeport service (32000). [to set different DB password, fork this project, edit password in k8s/final.yml and change the git clone link in setup.yml to your git link] 
   - Creates etcd backup after deployment
   -  Wordpress access at EIP:80
   
   OS:  Ansible server - Ubuntu 20.04  K8S - Amazon Linux2
----------------------
**Preparation:**
1. Install ansible
2. create a new project directory mkdir project
3. create cred.yml using 'ansible-vault create cred.yml' and store AWS access & secret keys(& security_token, if present) in the project directory
4. create roles folder mkdir project/roles
5. Run below commands inside roles folder: 
      ansible-galaxy init ec2;
      ansible-galaxy init k8s_master;
      ansible-galaxy init k8s_slave
6. Clone https://github.com/saikrissh9/capstone.git in a temp location
7. mv setup.yml and ansible.cfg from capstone/project/ to the project directory(project/)
8. Also, replace tasks/main.yml and vars/main.yml in 'project/roles/[ec2 |k8s_master|k8s_slaves]' with the ones from the 'capstone/project/roles/[ec2 |k8s_master|k8s_slaves]' directories
9. Update the project/roles/ec2/vars/main.yml with VPC ID, subnet, AMI ID, path to store generated keypair ( has to be in the project root folder at same level as setup.yml) and IP info.
10. Run 'ansible-playbook setup.yml --ask-vault-pass'
