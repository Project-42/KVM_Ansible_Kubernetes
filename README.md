# KVM_Ansible_Kubernetes #
Ansible Playbook to Create Kubernetes Cluster using KVM

## Disclaimer ##
This is no way shape or form a good place to get your bulletproof scripts to execute on your production systems.. and I hope that if you have production servers to manage, you won't be reading this...

The only reason this is public, is to commit myself some time to learn about ansible (and other tools) in a way that I have not problem showing it to others...
Hopefully, this will force me to spend time on it and not just leave it behind like most of other projects.

## Prerequisites ##
To work with these scripts, I assume you already have KVM and Ansible installed.
If you don't, you have few easy guides to follow, but just in case, you can see a couple of them below:

https://www.cyberciti.biz/faq/how-to-install-kvm-on-centos-7-rhel-7-headless-server/
http://www.ghanshammahajan.com/how-to-install-ansible-in-centos-7-with-pip/

You will need lxml to alter/create XML files for KVM
To install it, I recommend you to use "pip install lxml --user". More info: 

https://lxml.de/installation.html

## Usage ##

For now (until get some time to split the playbooks) the exeuction is as simple as execute the follwing to create the cluster:

`ansible-playbook playbook.yml`

or the folliwng to delete it:

`ansible-playbook Delete-all.yml`

## Configuration and changes before you start ##

- ansible.cfg file

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

- kubernetes_variables_vms.yml file
This is the main configuration file.
Here, you will need to set all parameters for the playbook to work

```---
kvm_host_user: solifugo

bridge_name: kuberbr42
gateway_ip: 10.10.1.1
netmask: 255.255.255.0
dhcp_range_start: 10.10.1.10
dhcp_range_end: 10.10.1.15
network_name: "Kubernetes"

kube_network: flannel

network_model: "virtio"
vm_location: "/home/kvm/HDD/Kubernetes"
root_pass: "Welcome1"


guests:
  kmaster:
    name: kmaster
    mem: 4096
    cpus: 4
    os_type: centos-7.0
    file_type: qcow2
    mac: '52:54:00:6c:20:00'
    ip: 10.10.1.10
  knode1:
    name: knode1
    mem: 2048
    cpus: 4
    os_type: centos-7.0
    file_type: qcow2
    mac: '52:54:00:6c:20:01'
    ip: 10.10.1.11
  knode2:
    name: knode2
    mem: 2048
    cpus: 4
    os_type: centos-7.0
    file_type: qcow2
    mac: '52:54:00:6c:20:02'
    ip: 10.10.1.12
```

