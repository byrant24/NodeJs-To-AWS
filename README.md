# ethenahealth_project
A project to build, configure and deploy a nodejs application to hello world

1. Build 

Impleneted Jenkins to construct a build system to construct a package of the application.

Jenkins url: http://13.127.81.33:8080

Jenkins job pulls the 'hello-world' node application from githb and construct a easily deployable pakcage of the application using npm pack.

2. Configure

- Ansible script 'ansible_deploy_package_ec2_instances.yml' uses the package created by jenkins and deploys it to the ec2 inventory list, unpacks the package and starts the node application.

ansible-playbook ansible_deploy_package_ec2_instances.yml -vvv

The application is available on {instance.public.ip}:5000

- 'hosts' file maintains the inventroy list of the {ec2.instnaces.publicip} where the application is to be deployed.
[node]
172.31.24.68 ansible_ssh_user=ubuntu ansible_ssh_private_key_file=ansible_ec2.pem ansible_python_interpreter=/usr/bin/python3


- 'ansible_ec2.pem' file is the aws ec2 key pair in order to connect.


- 'ansible_provision_ec2.yml' creates ec2 instnaces with attached security group and tags and writes them to the host inventory file.

NOTE: Ansible uses python boto framework to provision/configure ec2 instances hence a IAM user credentials are required for interactions.

export AWS_ACCESS_KEY_ID="XXXXXXXXXXXXXX"
export AWS_SECRET_ACCESS_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

ansible-playbook ansible_provision_ec2.yml -vvv

[local]
localhost

[webserver]
52.66.151.4

3. Deploy

CloudFormation Script - 'cloud_formation_script_stack.json'

