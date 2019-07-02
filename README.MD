This will install openstack onto a single VM

Steps:

1) Create an Ubuntu 18.04 VM  
 You probably want to give it 2-4 vCPUs and 8GB of RAM and ~15GB Storage  

2) Attach a 2nd disk ~20GB  

3) SSH into the VM as root  

4) Update all the available packages and reboot to apply latest kernel  
 `# apt update && apt dist-upgrade -y`  
 `# reboot`  

5) After SSH'ing back into the VM clone this repo into `/root`  
 `# git clone https://git.mikerkelly.com/mikerkelly87/openstack_ansible.git /root/openstack_ansible`  

6) Add the Ansible repo to make sure you get the latest version of Ansible  
 `# add-apt-repository ppa:ansible/ansible`  
 `# apt-get update`  

7) Install Ansible  
 `# apt install ansible -y`  

8) Ansible by default expects roles to be in `/etc/ansible/roles/` so let's create a symlink  
 `# ln -s /root/openstack_ansible/playbooks/aio /etc/ansible/roles/`  

9) Edit the Variables as you see fit for your environment  
 `# vim /etc/ansible/roles/aio/vars/create_Variables.yml`  
 The following values will almost absolutely need to be changed:  
  `ip_address_cidr` (based on the network information of your VM)  
  `gateway` (based on the network information of your VM)  
  `my_ip` (The IP of your VM)  
  `OVERLAY_INTERFACE_IP_ADDRESS` (The IP of your VM)  
  `PHYSICAL_VOLUME` (The 2nd disk you attached to the VM ex: `sdb`, `vdb`, `etc`)  

10) Run the playbook to apply the Openstack `aio` (all-in-one) role to your VM  
 `# ansible-playbook /root/openstack_ansible/playbooks/aio.yml`  

11) Once this has completed you can login to the Horizon dashboard at `http://<YOUR_VM_IP>/horizon`  
 The domain will be `default`  
 The username will be `admin`  
 The password will be what you put as the `ADMIN_PASS` in the variable file
