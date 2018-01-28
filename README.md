# ansible_playbook_ELupdate
Simple ansible playbook to apply patches to an inventory of linux systems. The role it calls is forked from https://github.com/while-true-do/ansible-role-system-update , currently supports only Red Hat family. Could be expanded to include SUSE family and Debian family straightforwardly enough if required.

**USAGE:**

1) Ensure ansible and ansible-galaxy are installed
2) Ensure you know are working from a system where you have an appropriate user and credential available to access all the required servers
3) Ensure you have access to an inventory file covering the required servers
4) Download the required roles with the command:
	ansible-galaxy install -r requirements.yml
5) Modify variables under group_vars/all to define behaviour per task requirement. These can also be set in inventory on a per-host or per-host-group basis, or in the ansible-playbook command, refer to Ansible documentation if this is required.
	 wtd_system_update_security_only=True|False
         - Install only security updates (True)
         - or all updates (False).
         - is True if you don't explicitely define it
	wtd_system_update_autoreboot=True|False
		- Perform reboot and wait for reconnection, if kernel or glibc has been updated (True)
		- Do not perform reboot regardless (False)
		- Is true if you don't explicitely define it.
	ansible_user=username
		- user account that has root access to servers in the inventory
		- If needed, can be overruled on an individual server basis in the inventory file
6) Run playbook with the command:
		-ansible-playbook elupdate.yml -i path/to/inventory/file --ask-become-pass
	If you don't have key-based or kerberos based passwordless access to these servers, you will need to run it as:
		-ansible-playbook -i path/to/inventory/file --ask-pass --ask-become-pass elupdate.yml 
	If you have passwordless sudo access or are allowed to ssh as root (I hope you have some solid bastion infrastructure in the latter case.)
		-ansible-playbook elupdate.yml -i path/to/inventory/file

7) Monitor output; respond to any servers that fail to come back online as an incident; when completed test application functionality is correct.


