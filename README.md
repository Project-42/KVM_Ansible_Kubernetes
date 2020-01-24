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
