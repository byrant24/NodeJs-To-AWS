# athenahealth project
A project to build, configure and deploy a nodejs application to hello world

1. Build 

  Impleneted Jenkins to construct a build system to construct a package of the application.

  Jenkins url: http://13.127.81.33:8080

  Jenkins job pulls the 'hello-world' node application from githb and construct a easily deployable pakcage of the application using npm   pack.




2. Configure

- Ansible script 'ansible_deploy_package_ec2_instances.yml' uses the package created by jenkins and deploys it to the ec2 inventory list, unpacks the package and starts the node application.


      ansible-playbook ansible_deploy_package_ec2_instances.yml -vvv


  The application is available on {instance.public.ip}:5000


- 'hosts' file maintains the inventroy list of the {ec2.instnaces.publicip} where the application is to be deployed.
                        
      [node]
      172.31.24.68 ansible_ssh_user=ubuntu ansible_ssh_private_key_file=ansible_ec2.pem ansible_python_interpreter=/usr/bin/python3


- 'ansible_ec2.pem' file is the aws ec2 key pair in order to connect.



NOTE: Ansible uses python boto framework to provision/configure ec2 instances hence a IAM user credentials are required for       interactions.

      export AWS_ACCESS_KEY_ID="XXXXXXXXXXXXXX"
      export AWS_SECRET_ACCESS_KEY="XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"



- 'ansible_provision_ec2.yml' creates ec2 instnaces with attached security group and tags and writes them to the host inventory file.
  
  
      ansible-playbook ansible_provision_ec2.yml -vvv

  
  The ansible playbook would write {ec2.instnaces.public.ip} to the hosts file.

      [local]
      localhost

      [webserver]
      52.66.151.4



3. Deploy

  CloudFormation Script - 'cloud_formation_script_stack.json'

  Summary - The CloudFormation scripts provisions infastructure, deploy and runs application at one click.

  Desciption - "AWS CloudFormation Template to host a sample 'hello-world' nodejs app. AWS CloudFormation Sample Template   AutoScalingMultiAZWithNotifications: Create a multi-az, load balanced and Auto Scaled sample node js application running on ec2 instnace behind a load balancer. The application is configured to span all Availability Zones in the region and is Auto-Scaled based on the CPU utilization of the web servers. Notifications will be sent to the operator email address on scaling events. The instances are load balanced with a simple health check against the default web page.The AMI is chosen based on the private AMI's available. Default VPC and subnets are taken which would have right network configuration to access the internet".

  Parameters - The CF script takes three parameters namely 'owner's email','instance type' and 'key pair'.

  Scalability - The nodejs Web server behind a load balancer along with an auto scaler.
              
  New instance would automatically spin up if CPU crosses 90% and shut down if less than 70% for continous 5,10 mins respectively.

  Alerts - Email would be sent out to the creator in case of any auto scaling trigger.

  Working Demo: http://finalstac-elasticl-1uxrdd39rsgxr-824859765.ap-south-1.elb.amazonaws.com:5000/

  AWS Architecture Diagram: https://s3.ap-south-1.amazonaws.com/anmol-nodejs-aws-architect/Web+App+Reference+Architecture+(1).png

