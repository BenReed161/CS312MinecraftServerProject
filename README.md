# CS321 Project Part 2: Deploy a Minecraft Server to AWS EC2 using Pulumi and Ansible.
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


## References
* [Alexander Ulbrich's Wordpress Demo](https://github.com/adulbrich/demo-pulumi-wordpress-ec2/?tab=readme-ov-file)  
* [Pulumi AWS Networking setup guide](https://www.pulumi.com/ai/answers/oW1vyw8e4Xi8dFD9Q552Df/creating-ec2-instances-with-aws-networking)  
* [Ansible Documentation](https://docs.ansible.com/)  
* [Pulumi Documentation](https://www.pulumi.com/docs/)  
* Alexander Ulbrich's OSU Computer Science 312 class notes and resources page.  
