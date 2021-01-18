# Ansible + Playbook + CSF + Alias 

## *Agenda*

#### - I wanted to have a solution, where i simply `WHITELIST` Ip addresses with single command on multiple remote server's CSF at Once.  

#### - I've many Servers and I use to access them from my remote locations. Even my team access them remotely but we all having dynamic ip's.

#### - So it became a repeating task for everyone to have their ip's `WHITELISTED`. 
#### - I thought to create a small ansible playbook and reduce the repeating work. 


## *Let's Begin -*

1. Setup the Ansible Inventory File.
	- In this case, mine inventory file is : aws_web.yml ( This inventory in yml format, you can follow the .ini format as well.)
	- *READ MORE - [How to create Ansible Inventory ?](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)*

	```
	$ cat aws_web.yml
	---
	all:
	  children:
	    control:
	      hosts:
	        JerryLocal: # My local ansible control node.
	          ansible_connection: local
	    aws_web: # All Servers
	      hosts:
	        apache1:  # My 2nd Web Server - XX.XX.XX.XX
	        apache2:  # My 2nd Web Server - XX.XX.XX.XX
	        dbserver1: # My DB Server - XX.XX.XX.XX  
	      vars:
	       ansible_ssh_private_key_file: /home/USER/PATH_OF_SSH_KEY_FILE
	       ansible_user: centos
	       ansible_become: true
	       ansible_become_user: root
	       # ansible_port: 22
	       host_key_checking: False
	...
	```       

2. Update `/etc/hosts` file with above mentioned remote nodes.
	- I've added my test servers Ip in my `/etc/hosts` file, so that i can access/call them with the name.
	- Note - do not forget to replace - XX.XX.XX.XX with your server's Ip's.
	```
	$ cat /etc/hosts
	127.0.0.1 localhost
	# all AWS Web Hosts & DB Host
	XX.XX.XX.XX apache1
	XX.XX.XX.XX apache2
	XX.XX.XX.XX dbserver1
	```

3. Now Let's Create the Ansible Playbook which will execute the csf command on remote Nodes.
	```
	---
	- name: Whitelisting Ip on AWS_WEB Host Group.
  	hosts: aws_web
  	gather_facts: false
  	become: yes
  	tasks:
       - name: Whitelist IP
         shell: csf -ta {{ ip }} 43200 {{ name }}
	```	
	- Here I'm using ansbile `shell` module to execute `CSF` command on remote servers.  
	- `csf -ta` I'm Whitelisting IP addresses temporarily.
	- `{{ ip }} & {{ name }}` are the variables, which we'll explain use later. 
	- `43200` Seconds (12 hours) for the ip will be remain whitelisted. 

	- *[READ MODE About - CSF](https://www.configserver.com/cp/csf.html)*

	
