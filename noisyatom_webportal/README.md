# noisyAtom_config
This describes the installation and setup of Noisy Atom web server. It requires that you have ansible setup on your
host, the ansible vault password and a remote machine that you have access to as the root user. 



## Customise Your Host File
The first thing to customize is the hosts file. This is the file where you could list all the hosts controlled by Ansible. 
If you only want to deploy to one machine only put one entry in this.

	/> nano hosts
	
Add your host name and IP address e.g.:
	
	[NoisyAtomUbuntu14]
	104.236.14.123



## Setup SSH On Your Machine
----------------------------
Create an SSH public and private key pair on your host machine. To learn more about what public/private keys are 
and used for or what the SSH protocol does read this:
https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
To create your public/private key on your machine do this:

	/>  ssh-keygen

Install the keys with the ssh-copy-id command like:

	/>  ssh-copy-id root@104.236.14.12
	
Check your key is installed by trying to SSH to the machine:

	/>  ssh root@104.236.14.12	



## Brief Description Of Files
Here is a brief description of each playbook you’ll find:
* create_users.yml      – Create an admin and CMS user on the server.
* install_software.yml  - Install nginx, django, postgres using apt, setup DB, setup virtual environment, start 
	postgres service, start nginx, stop nginx. Block root from SSH access
* deploy_system.yml     – Pull from the production branch. Install requirements. Setup gunicorn. Copy config files. 
	Migrate the DB and Collect static files. Restart nginx and gunicorn.
* upgrade.yml           – Pull from the latest commit on production branch. Run migrate. Run collectstatic. 
	Restart nginx and gunicorn.
* soft_upgrade.yml      – Pull from github production branch. Run migrate. Run collectstatic. This will allow nginx 
	to reload python files as they are requested and cache runs out.
* remove_software.yml   - Removes software components with apt. Deletes project directories. Deletes nginx and
	gunicorn files.
* delete_users.yml      – Connects in as admin user. Allows root access via SSH. Connects as root and and removes 
	admin and CMS users.
		


## Setup Users On Your Machine
------------------------------
As the root user, setup an admin user and a CMS user. The admin user will have sudo access to the machine. The CMS 
user will have limited access to the machine. The admin and CMS user names are defined in the /vars/project_variables.yml file.
* Note: Once you run this script root will not be able to login directly via SSH.

### Setup Host Variable If Needed
The create_users.yml file has a hosts variable at the top of the script. Ensure that the host you want to run the script against 
is configured here  and matches what you have in the 'host' file. At the time of writing it was tested against NoisyAtomUbuntu14 
machine. e.g.
	- hosts: NoisyAtomUbuntu14

### Running The File
From the root folder of this readme file do:
	/> ansible-playbook create_users.yml  --ask-vault-pass



## Install Software On Your Machine
-----------------------------------
As the admin user, this installs all the required base packages needed on your machine. Pip, Django, Nginx, Postgres and so on. 
It configures the DB, DB name and the DB user. It creates a virtual environment and installs django and python to the virtual 
env. It starts Postgres and Nginx. It finally stops Nginx and root access via SSH.

### Setup Host Variable If Needed
The install_software.yml file has a hosts variable at the top of the script, and near the end. Ensure that the host you want to 
run the script against is configured here  and matches what you have in the 'host' file. At the time of writing it was tested against 
NoisyAtomUbuntu14 machine. e.g.
	- hosts: NoisyAtomUbuntu14

### Running The File
From the root folder of this readme file do:
	/> ansible-playbook install_software.yml  --ask-vault-pass



## Deploy The Server
---------------------
As the CMS user, this sets up the project folder, pulls the production branch, runs the requirements.txt file. It
then runs the DB migration and CollectStatic commands. Finally it restarts the nginx and gunicorn services.

### Setup Host Variable If Needed
The deploy_system.yml file has a hosts variable at the top of the script and further down near the bottom. Ensure that the 
host you want to run the script against is configured here and matches what you have in the 'host' file. At the time of writing 
it was tested against NoisyAtomUbuntu14 machine. e.g.
	- hosts: NoisyAtomUbuntu14

### Apply The Correct Nginx File
Currently the deployed nginx config file has a hard coded IP address. This has to be changed until templating is applied to 
the ansible scripts. So the naecommerce file contains the line:

	server {
	    listen 80;
	    server_name 104.236.14.123;

Change the IP address to the machine you are deploying to.

### Update Settings.py On Production Branch To Match IP Address Of Machine
If you are deploying to an IP address that does not map to a DNS name you will need to change a section of your settings.py file
on the production branch. But if your DNS name maps to the IP address of your machine name then it does not require updating.
Hence for testing on an IP address you need to update the file settings.py within the config directory to include your new IP address
like:

    # Set allowed hosts so that we can verify where requests are coming from.
    ALLOWED_HOSTS = [
        'localhost',
        '127.0.0.1',
        'noisyatom.com',
        'noisyatom.co.uk',
        '104.236.14.123',
        '46.101.19.29',
        ]

You then need to update and push to the production branch

### Running The File
From the root folder of this readme file do:
	/> ansible-playbook deploy_system.yml  --ask-vault-pass


## Upgrade The Server
---------------------
As the CMS user, this pulls the production branch, runs the requirements.txt file. It then runs the DB migration and 
CollectStatic commands. Finally it restarts the nginx and gunicorn services.

### Setup Host Variable If Needed
The upgrade_new_release.yml file has a hosts variable at the top of the script and further down near the bottom. Ensure that 
the host you want to run the script against is configured here and matches what you have in the 'host' file. At the time of 
writing it was tested against NoisyAtomUbuntu14 machine. e.g.
	- hosts: NoisyAtomUbuntu14

### Running The File
From the root folder of this readme file do:
	/> ansible-playbook upgrade_new_release.yml  --ask-vault-pass



## Soft Upgrade The Server


## Remove Software


## Delete Users


