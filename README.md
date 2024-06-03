# CS312 Project Part 2: Deploy a Minecraft Server to AWS EC2 using Pulumi and Ansible.
How to deploy a Minecraft Server with Pulumi and Ansible to an AWS EC2 instance:
## Requirements
* [Pulumi](https://www.pulumi.com/docs/install/)  
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)  
* [Valid AWS Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html)
* (Windows only) [Install AWS CLI](https://aws.amazon.com/cli/)
### Pulumi
Pulumi offers binary downloads for both Windows and Mac, as well as package versions through brew(Mac). Linux is offered either through publically maintained versions across different package managers(check with your distrobutions package repo), or with a installation script provided on the Pulumi site(link above).
### Ansible
Ansible is installed via pipx package manager or through the a distros package repo, and can be install similar to Pulumi(pacman, apt, etc..). Windows is not supported as a control node, only through a WSL instance can you create control nodes with Ansible on Windows.
### AWS Credentials
More information about recieving your AWS credentials is provided in the link above. You can provide your valid AWS credentials by creating a file in `~/.aws/` called `credentials`. And pasting your valid credential file into it.  
For Windows users not using a WSL instance can find it useful to use the CLI commands to setup your credentials, use the structure of the configure command:  
`aws configure set <variable> "value"`  
`aws configure set aws_access_key_id "ABC123"`  
## Overview
The goal of this project was to create an EC2 instance in AWS that ran a Minecraft server using infrastructure provisioning scripts. I chose to use Ansible and Pulumi, and followed a structure similar to Alexander Ulbrich's Wordpress demo (see References section). The idea is that there should be no interaction with the EC2 instance directly, meaning no ssh or use of the aws cli to directly interface with the instance. Instead I was tasked with setting up scripts to create and run the Minecraft server automatically with a single command. The server should also be restarted everytime the EC2 instance restarts to ensure that no sshing is required.  
  
I achieved this automation by creating a systemctl service file to load the minecraft server on start of the Debian instance. This creation and startup configuration is all handled by an Ansible playbook. The playbook also handles all other commands run on the remote, including installing Java and the Minecraft server jar file, as well as configuring the Minecraft server files. The core AWS management is handled by Pulumi which interfaces with AWS using the provided credentials and creates EC2, network, and keypair instances on your behalf through AWS. In my implementation I handed the specification of the instance type and the amount of RAM to be used for the service to the user. It is up to them to determine how much to provision on the budget they have.  
  
The defaults for the EC2 instance are detailed in the "How to launch" section.  
  
The user does not have the choice of OS however, I determined Debian as the best suited OS for this particular Minecraft server. Debian is highly compatible with most packages and has a long history of support behind it. It does not matter what region you choose the Debian AMI should be chosen correctly for you.  


## References
* [Alexander Ulbrich's Wordpress Demo](https://github.com/adulbrich/demo-pulumi-wordpress-ec2/?tab=readme-ov-file)  
* [Pulumi AWS Networking setup guide](https://www.pulumi.com/ai/answers/oW1vyw8e4Xi8dFD9Q552Df/creating-ec2-instances-with-aws-networking)  
* [Ansible Documentation](https://docs.ansible.com/)  
* [Pulumi Documentation](https://www.pulumi.com/docs/)  
* Alexander Ulbrich's OSU Computer Science 312 class notes and resources page.  
