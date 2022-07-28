# Thoughtworks-mediawikiApplcation

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
linuxmachinekey.pem --> Pem Key that I am using to login into the managed nodes 
mediawiki.yaml --> This is our Ansible Script. If we run this Script, it will install the MediaWiki application. If you want to run the script manually, then clone this repo and run this ansible playbook

ansible-playbook -i inventoryfile mediawiki.yaml

mysql.yaml --> We have stored the mysql password here. We can use Vault to encrypt if needed
my.cnf.j2 --> Default config directory for Mysql. I just set the mysql username and password
providers.tf --> 

![image](https://user-images.githubusercontent.com/94977452/181640421-38269968-500a-416e-9c36-a1ff756b134c.png)




















