
# Documentation Structure

[**About this project**](README.md#about-this-project)  
[**About Ansible**](README.md#about-ansible)  
[**About Ansible modules for Junos automation**](README.md#about-ansible-modules-for-junos-automation)  
[**Requirements for Junos automation with Ansible**](README.md#requirements-for-junos-automation-with-ansible)  
[**Requirements to use this repository**](README.md#requirements-to-use-this-repository)  
[**Repository structure**](README.md#repository-structure)  


# About this project   

This project has many ready-to-use Ansible playbooks to interact with Junos devices.   

This repository has been tested using: 
- Ubuntu 16.04
- Ansible 2.4.2.0  
- the version 1.4.3 of the Juniper.junos role available on Galaxy.  


# About Ansible

Ansible is an open-source IT automation tool.  

[Here's an ansible presentation](ansible.pdf)  

[Ansible vs Saltstack vs Stackstorm](https://medium.com/@anthonypjshaw/ansible-v-s-salt-saltstack-v-s-stackstorm-3d8f57149368)  

# About Ansible modules for Junos automation

There are two modules libraries to interact with Junos:
- An Ansible library for Junos built by Juniper
    - These modules are available on [Ansible Galaxy website](https://galaxy.ansible.com/Juniper/junos/)   
- An Ansible library for Junos built by Ansible 
    - Since Ansible version >= 2.1, Ansible natively includes [core modules for Junos](http://docs.ansible.com/ansible/latest/list_of_network_modules.html#junos). 
  - These modules are shipped with Ansible
  - The Junos modules included in Ansible core have names which begin with the prefix **junos_**. 
  
These two sets of modules for Junos automation can coexist on the same Ansible control machine.  
Both of them are used in this repository.  

### Ansible modules for Junos built by Juniper and hosted on Galaxy

- They are hosted on the [Ansible Galaxy website](https://galaxy.ansible.com/Juniper/junos/):      
  - The role is Juniper.junos  
- Here's the [source code](https://github.com/Juniper/ansible-junos-stdlib)  
- Until the version 1.4.3 of the modules included in the Juniper.junos role:
  - Their names begin with the prefix **junos_**. 
  - Here's the [doc for the version 1.4.3](http://junos-ansible-modules.readthedocs.io/en/1.4.3/)
  - To download and install them to the Ansible server, execute the command ```sudo ansible-galaxy install Juniper.junos,1.4.3```
- From version 2 of the modules included in the Juniper.junos role: 
  - To avoid conflict with the names used by ansible native modules for Junos, since the version 2 of the modules included in the Juniper.junos role on Galaxy, their names begin with the prefix **juniper_junos_**. 
  - Here's the [doc for the last version](http://junos-ansible-modules.readthedocs.io/en/stable/) 
  - To download and install them to the Ansible server, execute the command ```sudo ansible-galaxy install Juniper.junos```

### Ansible modules for Junos built and shipped by Ansible  

- Here's the [documentation](http://docs.ansible.com/ansible/latest/list_of_network_modules.html#junos)   
- The Junos modules included in Ansible core have names which begin with the prefix **junos_**. 
- Here's the [source code](https://github.com/ansible/ansible/tree/devel/lib/ansible/modules/network/junos)    
- Installation: They are shipped with ansible itself (from Ansible 2.1). Ansible 2.1 or above is required.  

# Requirements for Junos automation with Ansible  

### On the Ansible server

Most of these Ansbile modules require installing the python library **py-junos-eznc** on the Ansible server.  
Some options also require installing the python library **jxmlease** on the Ansible server.  
Some options (like the console option in the junos_install_config module) also require the python library **junos-netconify** on the Ansible server.  

### On the Junos devices

Except for the module **junos_netconf**, all the Ansible modules for Junos require the NETCONF to be configured on the Junos devices.  
Note: It is not required to use Junos CLI to configure Netconf on Junos devices. This can be done with the Ansible module junos_netconf. 

# Requirements to use this repository  
 
### Get the content of the remote repository locally

```
sudo -s
git clone https://github.com/ksator/junos-automation-with-ansible.git
ls junos-automation-with-ansible
```

### Move to the local copy of the remote repo

```
cd junos-automation-with-ansible
sudo -s
```

### Install PyEZ, Jxmlease, Ansible, JSNAPy and all their dependencies

This repository has been tested using Ansible 2.4.2.0  

Run these commands on Ubuntu 16.04 to install these tools:
```
sudo -s
apt-get update
apt-get upgrade
apt-get install -y python-dev libxml2-dev python-pip libxslt1-dev build-essential libssl-dev libffi-dev git
pip install junos-eznc jxmlease wget jsnapy ansible==2.4.2.0 requests ipaddress cryptography 
ansible-galaxy install Juniper.junos,1.4.3
```
Check the Ansible version:
```
ansible --version
```
Verify you have the Juniper.junos role: 
```
ls /etc/ansible/roles/
```
This repository has been tested using the version 1.4.3 of the Juniper.junos role available on Galaxy.  
Use this command to see the name and version of each role installed:
```
ansible-galaxy list
```

### Configure NETCONF on the Junos devices

```
set system services netconf ssh
commit
```

# Repository structure 

### Inventory file:  

The default ```hosts``` file lives in ```/etc/ansible/hosts```.  

The inventory file we are using in this repository is [**hosts**](hosts).  
- It is at the root of the repository, so it is not at the default place.  
- It defines the inventory (hosts and groups).  
- It also defines the ip address of each device with the variable **junos_host**. This variable is re-used in the playbooks.     

### Config file for ansible   
There is an [**ansible.cfg**](ansible.cfg) file at the root of the repository.  
It refers to [our inventory file](**hosts**): So even if the inventory file is not in ```/etc/ansible/hosts```, there is no need to add ```-i hosts``` to your ```ansible-playbook``` commands.  

### Variables  
[**group_vars**](group_vars) and [**host_vars**](host_vars) directories at the root of this repository define variables for hosts and for groups.  
The inventory file [**hosts**](hosts) at the root of the repository also defines some variables.   
The playbooks in this directory use all of them.   
Some playbooks also use other variables.  
In order to see all variables for a ```hostname```, you can run this command:  
```
ansible -m debug -a "var=hostvars['hostname']" localhost
```

### Playbooks  
All playbooks in this repository are named pb*.yml 
These playbooks use the two sets of modules for Junos automation. They also use other Ansible modules (template, assemble, uri, wait_for, debug, ...).  

### Directories
This repository uses several directories.  
I am re-using the Ansible module names for the directories names.  
Each directory has: 
- Playbooks
- a readme file

### Lab topology   
The lab topology is described in the file [lab topology.pdf](lab%20topology.pdf)  

You can very easily re-use this automation content with your own Junos devices: you just need to build a similar topology and then adapt this content with your IP addresses, username and password. 

### repository usage instructions  

You need to run the playbooks from the root of the project tree.  
Use the ```ansible-playbook``` commands to execute the playbooks:    
```
cd junos-automation-with-ansible
ls
ls -l
ls xxx/
more xxx/readme.md
ansible-playbook xxx/pb*.yml  
```

