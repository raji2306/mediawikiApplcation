#MediawikiApplcation

GitHub Link : https://github.com/raji2306/mediawikiApplcation

Hi Guys, 

In this Tutorial, we are going to create an MediaWiki Application through Ansible Script. Also, We are going to run the ansible script through Terraform Script.
One more Highlight is we are going to do this complete processs in CI / CD Pipeline. We are using Jenkins tool for this tutorial.

Terraform : 

Terraform is an open-source, infrastructure as code, software tool created by HashiCorp. Users define and provide data center infrastructure using a declarative configuration language known as HashiCorp Configuration Language (HCL)

Ansible :

Ansible is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality

Jenkins : 

Jenkins is an open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery.

Let's Begin!!!

First, we look at our folder structure

Jenkinsfile --> We are going to use this file in Jenkins Pipeline. Note : If you have multiple .tf files in repo. then, use target to specify the particular file.


ansible.cfg --> This is just an Asnible Configuration File. Stable Version, If you want you can use it. 


mediawiki.yaml --> This is our Ansible Script. If we run this Script, it will install the MediaWiki application. If you want to run the script manually, then clone this repo and run this ansible playbook

ansible-playbook -i inventoryfile mediawiki.yaml

![image](https://user-images.githubusercontent.com/94977452/181640421-38269968-500a-416e-9c36-a1ff756b134c.png)


mysql.yaml --> We have stored the mysql password here. We can use Vault to encrypt if needed


my.cnf.j2 --> Default config directory for Mysql. I just set the mysql username and password


providers.tf --> This is the main file that needed to run the terraform script. It contains the Cloud providers details and our access, secret keys 


linuxmachinekey.pem --> Pem Key that I am using to login into the managed nodes 

Now we look at the 3 Terraform Scripts step by step

1. ec2.tf :

By using this Terraform File, we are going to create a Centos EC2 instances with SG,LifeCycle policy and also running our Ansible script using provisioner. 
  
For this Tutorial, I am using Centos 7 Image So I have subscribed it from AWS Marketplace. 
  
![image](https://user-images.githubusercontent.com/94977452/181642679-324787ea-73d2-44f3-a95e-c016426bf676.png)

prerequisite : Please install the Jenkins server. So that, we can run this script through Jenkins pipeline

Also, Please Install Terraform plugin. I have already installed the TerraForm Plugin

![image](https://user-images.githubusercontent.com/94977452/181643254-5d35a666-bd60-42d9-9b50-b705e42d2c00.png)

Also, specify the Terraform Executables Path

![image](https://user-images.githubusercontent.com/94977452/181647455-25318ede-7859-4107-8aad-f86bd8582cb5.png)


Before Running the Script :

![image](https://user-images.githubusercontent.com/94977452/181643347-b2792828-50c4-4f11-8618-b96bdc112f13.png)

Our Script ran successfully through Jenkins Pipeline :

![image](https://user-images.githubusercontent.com/94977452/181646268-a263460b-09c4-4546-ac3c-ac40da9d2fbd.png)

![image](https://user-images.githubusercontent.com/94977452/181646337-af90dea4-d6f8-4556-bd75-e046947c4116.png)

Now,  try to access our application 

![image](https://user-images.githubusercontent.com/94977452/181646405-722b013d-df4d-4c63-9155-75e46a9529ad.png)

Also, we are able to connect to database using the user that we created through script

![image](https://user-images.githubusercontent.com/94977452/181646586-c965f10c-333e-42c0-869e-19f789134815.png)

Yes!!! We are able to connect to Database

![image](https://user-images.githubusercontent.com/94977452/181646639-967567e3-12fa-45ef-8762-5b37c48b5acb.png)

Now, login into the our control machine and see whether the scripts and state files are availabe and also delete it using "terraform destroy -auto-approve"

![image](https://user-images.githubusercontent.com/94977452/181646879-9dad87f8-049a-44d6-9d3c-5bd98e30a961.png)

Deleted Successfully !!!

![image](https://user-images.githubusercontent.com/94977452/181646925-0a45671b-a123-452b-bf99-98311364129e.png)


2. Rollingdeploy.tf

Rolling Deployment : It is an deployment strategy that slowly replaces previous versions of an application with new versions of an application by completely replacing the infrastructure on which the application is running

By using ec2.tf file, we have successfuly Created an EC2 instances with MediaWiki Application. And we have created an AMI based on this server.

Now We are going to create a LoadBalancer with LaunchConfiguration and Autoscaling Group based on the AMI we created 

So that, If there is an some changes happened to the application. We can just change our AMI ID in the File. This is an Rolling Deployment

I have attached the Server, LoadBalancer, SG and LaunchCofiguration Screenshot as well

Our Script is successully an on Jenkins Pipeline :

![image](https://user-images.githubusercontent.com/94977452/181644832-49e344de-e4b5-44c8-9b6c-9730b54f6e21.png)

AMI Image we build :

![image](https://user-images.githubusercontent.com/94977452/181644354-6e6cf34e-c3b9-495a-be5c-6ac7ad77d12e.png)


LoadBalancer :

![image](https://user-images.githubusercontent.com/94977452/181644735-9764c482-e162-47f6-8e8e-c2141801101b.png)


Application Running based on AMI Image :

![image](https://user-images.githubusercontent.com/94977452/181644523-96b75fe0-550e-4f0a-9680-57ca81e4b647.png)

Mediawiki is successfully launched 

![image](https://user-images.githubusercontent.com/94977452/181644267-68fc8b07-f2dd-4972-b225-4f94426e4a75.png)

3. vpc.tf :

Please use this Script if you want to create a VPC in separate VPC and CIDR

For profession way, we don't use IP Address for accessing the server. So for fun, we will just attach an domain name to our instances and try to access it.

For Time Being, I have done this for one instances that we have created using ec.tf. If we want, we can attach a domain name to our Load Balancer

I have register an domain name from Freenom

![image](https://user-images.githubusercontent.com/94977452/181648842-9b769a57-07a4-4cac-8958-de3dd4cca1e7.png)

Now, By using AWS R53 I have mapped the ip address to domain name. Now we are able to access using Domain name.

![image](https://user-images.githubusercontent.com/94977452/181649369-d82c4265-2530-4e95-a54c-3cf36c3b7a3d.png)


That's it :

We have successfully configured our MediaWiki application using Jenkins, Terraform and Ansible
