# KVM_Ansible_Kubernetes #
Ansible Playbook to Create Kubernetes Cluster using KVM

## Disclaimer ##
This is no way shape or form a good place to get your bulletproof scripts to execute on your production systems.. and I hope that if you have production servers to manage, you won't be reading this...

The only reason this is public, is to commit myself some time to learn about ansible (and other tools) in a way that I have not problem showing it to others...
Hopefully, this will force me to spend time on it and not just leave it behind like most of other projects.

Since you will find errors and problems, please make a comment about them, I will love to inprove this project and keep learning with it :)

## Prerequisites ##
To work with these scripts, I assume you already have KVM and Ansible installed.
If you don't, you have few easy guides to follow, but just in case, you can see a couple of them below:

https://www.cyberciti.biz/faq/how-to-install-kvm-on-centos-7-rhel-7-headless-server/
http://www.ghanshammahajan.com/how-to-install-ansible-in-centos-7-with-pip/

You will need lxml to alter/create XML files for KVM
To install it, I recommend you to use "pip install lxml --user". More info: 

https://lxml.de/installation.html

## Usage ##

The exeuction is as simple as execute the follwing to create the cluster:

`ansible-playbook playbook.yml`

or the folliwng to delete it:

`ansible-playbook Delete-all.yml`

By defult, the script will create 1 Master and 2 Nodes, as well as a KVM network.
Additionally (at least for now, since I dont think should be necessary) will add those elements to your host `/etc/hosts` file

## Configuration and changes before you start ##

- *ansible.cfg*

For the default configuration, I have disable host_key_checking.
The remote user has been set to root just in case your kvm_host_user is different
```[defaults]
# host_key_check diabled to avoid issues connecting to the VMs created *Is a security risk*
host_key_checking = False

# Make user we are using local inventory and not the default one
inventory=./inventory

# Set default user to root
remote_user=root
```

- *variables.yml*

This is the main configuration file.
Here, you will need to set all parameters for the playbook to work.
I have changed this file multiple times, and I'm committed to simplify it in the future, but will take time... :)
```---
################################################
##                                            ##
## This file contains all necessary variables ##
##                                            ##
################################################

#

kvm_host: localhost

# KVM Network Information

bridge_name: kuberbr42
gateway_ip: 10.10.1.1
netmask: 255.255.255.0
dhcp_range_start: 10.10.1.10
dhcp_range_end: 10.10.1.15
network_name: "Kubernetes"
network_model: "virtio"



#Will try to use ipmath to simplify variables
#https://docs.ansible.com/ansible/2.7/user_guide/playbooks_filters_ipaddr.html#ip-math
# {{ '192.168.1.5' | ipmath(5) }}
#192.168.1.10
# {{ '192.168.0.5' | ipmath(-10) }}
#192.167.255.251


#You can use flannel or calico network#
kube_network: "flannel"

# To change vm_location, remember to add quemu user priviledges to that folder
vm_location: "{{ playbook_dir }}/KVM_Disks"

root_pass: "Welcome1"

seed_name: kseed

os_type: centos-7.0
file_type: qcow2

guests:
  kmaster:
    name: kmaster
    mem: 4096
    cpus: 4
    mac: '52:54:00:6c:20:00'
    ip: 10.10.1.10
  knode1:
    name: knode1
    mem: 2048
    cpus: 4
    mac: '52:54:00:6c:20:01'
    ip: 10.10.1.11
  knode2:
    name: knode2
    mem: 2048
    cpus: 4
    mac: '52:54:00:6c:20:02'
    ip: 10.10.1.12
```

- */etc/hosts*

In order to get the playbook working, is necessary to add the VMs hostname and IPs to you kvm_host hosts file. 
I added that step into the initial playbook file `vms_creation.ym`, so Im using `become_user: root`, or just add them directly before the ansible execution.
Should end like this:
```

[....]
    - name: add Kbernetes line to /etc/hosts
      # Adding title to /etc/hosts file
      become: yes
      become_user: root
      lineinfile:
        dest: /etc/hosts
        line: "## KUBERNETES CLUSTER ##\n"
        state: present
[....]
```        

- *playbook.yml*

The main playbook file is just a way to divide 3 different playbook that will start each of the phases.

I tried to add comments in all the files, so won't be into details here.

```
---

# vms_creations.yml will create the kvm VMs in the kvm_host
- import_playbook: vms_creation.yml

# kubernetes_requirements.yml will modify VMs to install docker/kubernetes software
- import_playbook: kubernetes_requirements.yml

# kubernetes_create_cluster.yml will create the kubernetes cluster
- import_playbook: kubernetes_create_cluster.yml
```


