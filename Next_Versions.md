## Next Version Improvements ##

#Fix master latest restart#
For some reason, the master wont show as ready unless is rebooted after cluster is created.
Need to avoid that or at least, understand why is happening

#Divide Playbook.yml on different files#
Playbook is too large, so will try to divide different phases and use playbook include instead

#VMs Update process#
Change the way VMs are updated. Currently the VMs are created and them updated, that means we wait for all VMs to get updated.
Need to work in a system where a VM seed is created, updated and then cloned to create the VMs requested

#Simplify the Variables Files#
There should be not need to set so many different variables.
Macs can be ramdomize with Random Mac Address Filter (https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html#random-mac-address-filter)
DHCP range should be obteine from the guests lists

#Make playbook "Ubuntu ready"#
Work with the Generic OS package manager to make playbbok compatible with Debian/Ubuntu (https://docs.ansible.com/ansible/latest/modules/package_module.html#package-module)

#Allow to select calico/flannel networks#
Add a Variable in the variables file to create system with flannel or calico


#Use more modules and less commands#
VMs creation/delete is using commands instead of virt module
Same with other parts of the playbook
(https://docs.ansible.com/ansible/latest/modules/virt_module.html?highlight=virt)
## defining and launching an LXC guest
#- name: define vm
#  virt:
#    command: define
#    xml: "{{ lookup('template', 'container-template.xml.j2') }}"
#    uri: 'lxc:///'