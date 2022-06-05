# Chapter 2 Notes - Testing Ansible

This chapter 2 guide shows you how to test basic setup. It will test your remote server can receive execute and run ansible
commands. It then sets up your public private key on the remote server and runs through test on your system to prove it works.

**Note:** Before you begin make sure that you can:
1. SSH onto the machine by manually ssh'ing onto the machine!
2. That the username and password are correct and work!

e.g.
```
	/> ssh root@<ip_address_of_your_server>
```


## Step 1 - Test Ansible Works!

We need to send the equivalent of a ping command for ansible which shows our connection works. To do this we need to run the
ansible 'ping' command to your remote server. To do this Ansible needs to use the **sshpass** program. Install this first:

```
	/> sudo apt install sshpass
```

Next Ansible needs to know the remote server address, the user it should pretend to be and the password of that user.
It will find out the IP address of the machine from hour hosts file you setup in Chapter 1. Hence we will use that mapping 
from chapter 1. You should have a host file in this directory that maps a name to an IP address. We are using the name:
	
	NoisyAtomUbuntu16

Run the command:

	/>  ansible NoisyAtomUbuntu16 -m  ping  -u  root  --ask-pas
		SSH password: 
		46.101.45.29 | SUCCESS => {
			"changed": false, 
			"ping": "pong"
		}

The command is 'ansible'. The module is 'ping'. The user we are using is -u. We also pass --ask-pas which tells ansible to
ask for the password. Normally ansible will not allow you to type passwords and requires public/private key on the remote server.

If you have already run ansible scripts which disallow 'root' access you might get a message like this:

***104.236.14.123 | UNREACHABLE! => {
    "changed": false, 
    "msg": "Authentication failure.", 
    "unreachable": true
}***

In this case you can try the same command but as the machine admin user e.g. :

	/>  ansible NoisyAtomUbuntu14 -m  ping  -u  noisy_atom_admin  --ask-pas
	

## Step 2 - Get Your Public Key Onto The Remote Server

In Chapter 1 you create a public/private key for your host machine. We need to copy the public key to the remote machine.
Install your public key onto your remote server. If you like you can see your new keys in dir:
	~/.ssh/id_rsa.pub
	
Install the keys with the ssh-copy-id command like:
	/>  ssh-copy-id root@104.236.14.123
	
Check your key is installed by trying to SSH to the machine:
	/>  ssh root@104.236.14.123

You will not be asked for your password! Great stuff!!!

Please bare in mind that for different users on the server you will need to copy across SSH keys. i.e. if you have a cms and 
admin user you will also have to copy those keys to the remote machine e.g. :
	/>  ssh-copy-id noisy_atom_admin@104.236.14.123

### Python 2 Errors On Ubuntu 18.04
If your system is using Ubuntu 18.04 you may notice the following error when running ansible as shown above:

```
ansible NoisyAtomUbuntu18 -m  ping  -u  root
134.209.21.58 | FAILED! => {
    "changed": false, 
    "module_stderr": "Shared connection to 134.209.21.58 closed.\r\n", 
    "module_stdout": "/bin/sh: 1: /usr/bin/python: not found\r\n", 
    "msg": "MODULE FAILURE", 
    "rc": 127
}

```
This is telling you that the ansible application can't find python on the system. Which is true for Ubunut18.04, only Python3 is installed as the default python interpretar. To tell ansible to use python3 you can pass in the **-e** (environment variables flag) to use python 3 - which is: **ansible_python_interpreter**

The command should look like this:

```
ansible NoisyAtomUbuntu18 -m  ping  -u  root -e 'ansible_python_interpreter=/usr/bin/python3'
134.209.21.58 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}

```


## Step 3 - Test Ansible Ping

We are not going to test Ansible Ping again. This time we will NOT need to use '--ask-pas' as the SSH protocol will use the public
keys copied onto the server in Step 2 during encryption. Ansible can now communicate to that server as the 'root' user without
asking for password.

Run the command:

	/>  ansible  NoisyAtomUbuntu14  -m  ping  -u  root
		SSH password: 
		46.101.45.29 | SUCCESS => {
			"changed": false, 
			"ping": "pong"
		}

## Step 4 - Test Free Memory Query

Run the command:

	/>	ansible NoisyAtomUbuntu14 -a 'free -m' -u root
		104.236.14.123 | SUCCESS | rc=0 >>
					 total       used       free     shared    buffers     cached
		Mem:           488        451         36         14        124        204
		-/+ buffers/cache:        122        365
		Swap:            0          0          0


## Step 5 - Test Free Disk Space Query

Run the command:

	/>	ansible NoisyAtomUbuntu14 -a "df -h" -u root
		104.236.14.123 | SUCCESS | rc=0 >>
		Filesystem      Size  Used Avail Use% Mounted on
		udev            228M  4.0K  228M   1% /dev
		tmpfs            49M  396K   49M   1% /run
		/dev/vda1        20G  2.1G   17G  11% /
		none            4.0K     0  4.0K   0% /sys/fs/cgroup
		none            5.0M     0  5.0M   0% /run/lock
		none            245M     0  245M   0% /run/shm
		none            100M     0  100M   0% /run/user
	
	
