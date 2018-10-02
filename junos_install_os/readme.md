module: junos_install_os  
Install a Junos OS image. 
doc: http://junos-ansible-modules.readthedocs.io/en/1.4.3/  
installation: this role is hosted on the Ansible Galaxy website (https://galaxy.ansible.com/Juniper/junos/).  
To download and install the Juniper.junos role to the Ansible server, execute the command: ```ansible-galaxy install Juniper.junos,1.4.3```  

playbook: 
- **pb.yml**: upgrade Junos devices: The new Junos image is already in Ansible. Install the new Junos image on a remote device and reboot the device. if the existing Junos OS version on the remote device matches the desired version, no action is performed. You can run it in check mode   

usage:  
```
ansible-playbook junos_install_os/pb.yml  --check
```
```
ansible-playbook junos_install_os/pb.yml  
```
