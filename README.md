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

## How to launch
Clone the repo through the `git clone` command or through Github by downloading the zip.  
Before deploying the AWS Minecraft Server there are some initial configuration steps you must adhere to.  
1. Create a new pulumi stack with `pulumi stack init` in a command window. (Pulumi may ask you to login, or create an account if don't already have one)  
2. Generate the key that will be used to login to the EC2 instance with the `ssh-keygen` command, this will create a private a public key on the local machine. (Never share private keys).
   ```bash
   ssh-keygen -f minecraft-key
   ```
3. Now to configure some AWS values through pulumi.  
   ```bash
   pulumi config set aws:region us-east-1 # any AWS region, a full list can viewed here: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html
   pulumi config set publicKeyPath minecraft-key.pub # The public key generated in the previous step
   pulumi config set privateKeyPath minecraft-key # The private key generated in the previous step
   ```
4. (Optional) There are some additional values that can be changed as part of the setup, although they are not required.
   ```bash
   pulumi config set mcserverEC2size t3.large # defaults to t3.medium (4GB RAM)
   pulumi config set mcserverXMX 4096 # Size in MB. The max RAM available for the JVM, defaults to 3072MB
   pulumi config set mcserverXMS 4096 # Size in MB. The staring RAM available for the JVM, defaults to 3072MB
   ```
   **Note:** XMX and XMS, do not set these higher than the available EC2 instance RAM. It will crash the JVM. Ensure you have the correct settings before continuing and you do not over-provision resources.  
   **Additonal tip:** Set XMX and XMS equal to each other, since this EC2 instance will only be a Minecraft server there is no need to start the server with a lower RAM than the max. It is also common to set the max near or to the maximum system RAM, I would recommend against this as it can cause system crashing, but it's up to you.
5. Now that all the configuration steps are done can deploy the system by running `pulumi up`. This command starts the creation of all the resources needed to run the server.  
   You will be prompted if you want to continue `Do you want to perform this update? yes` select 'yes' from the options. Now the update will begin.
   This may take some time ~5 mins, so be patience.
   Once the script completes it will spit out some DNS and IP information.
   ![Pulumi complete](https://github.com/BenReed161/CS312MinecraftServerProject/blob/main/images/pulumi1.png)
   The `publicIp` is what you connect with and put into your Minecraft client to connect.
   The `publicDNS` is just for your convenience, so that if in the future you can SSH into your instance to check logs and access the minecraft server console.
6. (Optional) If you're feeling adventurous you are free to edit the Ansible playbook or the Pulumi setup to fit your needs. Save your changes with a `pulumi up` and it will update the instance with any changes you make.
7. When you're done with the server or screwed anything up at any part of the setup run:
   ```bash
   pulumi destroy # Will delete and destroy all allocated resources by this script.
   pulumi stack rm # Will completly remove the project stack associated with the Pulumi configuration.
   ```
You're Done!
And I hope you ..
![ENOJY!](https://github.com/BenReed161/CS312MinecraftServerProject/blob/main/images/mc.png)
   


## References
* [Alexander Ulbrich's Wordpress Demo](https://github.com/adulbrich/demo-pulumi-wordpress-ec2/?tab=readme-ov-file)  
* [Pulumi AWS Networking setup guide](https://www.pulumi.com/ai/answers/oW1vyw8e4Xi8dFD9Q552Df/creating-ec2-instances-with-aws-networking)  
* [Ansible Documentation](https://docs.ansible.com/)  
* [Pulumi Documentation](https://www.pulumi.com/docs/)  
* Alexander Ulbrich's OSU Computer Science 312 class notes and resources page.  
