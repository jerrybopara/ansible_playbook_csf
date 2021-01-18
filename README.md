# Ansible + Playbook + CSF + Alias 

## *Agenda*

#### - I wanted to have a solution, where i simply `WHITELIST` Ip addresses with single command on multiple remote server's CSF at Once.  

#### - I've many Servers and I use to access them from my remote locations. Even my team access them remotely but we all having dynamic ip's.

#### - So it became a repeating task for everyone to have their ip's `WHITELISTED`. 
#### - I thought to create a small ansible playbook and reduce the repeating work. 


## *Let's Begin -*

1. Setup the Ansible Inventory File.
	- In this case, mine inventory file is : aws_web.yml
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

	``` 