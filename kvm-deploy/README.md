Role Name
=========

A role to deploy VM on a KVM hypervisor inspired by [this post](https://www.redhat.com/sysadmin/build-VM-fast-ansible) on Red Hat.

Requirements
------------

- Make sure a Storage pool is available.
  This will be used in the var "libvirt_pool_dir".

- Make sure that a ssh key is available

Role Variables
--------------

These are the variables that can be set in the playbook:
```
base_image_name: Fedora-Server-KVM-38-1.6.x86_64.qcow2
base_image_url: https://download.fedoraproject.org/pub/fedora/linux/releases/38/Server/x86_64/images/
base_image_sha: fe07c4885c6a224e5527c6eccd0ada07c858a0eec2135e1c9ccd02c26eec408d
libvirt_pool_dir: "/mnt/vms"
vm_name: f38-minikube
vm_vcpus: 8
vm_ram_mb: 16384
vm_net: default
vm_root_pass: test123
cleanup_tmp: no
ssh_key: /root/.ssh/id_rsa.pub
nested: true|false
```

Setting nested to true will enable the cpu-passthrough in the guest vm, enabling nesting virtualization.
This still requires that the hypervisor is configured to so so ("options kvm_intel nested=1" for Intel
or "options kvm_amd nested=1" for AMD, in modprobe)

Dependencies
------------

- community.libvirt

Example Playbook
----------------

An example of a playbook to deploy a VM:
```
- name: Deploys VM based on cloud image
  hosts: localhost
  gather_facts: yes
  become: yes
  vars:
    vm: f38-minikube
    cleanup: yes
    net: default
    ssh_pub_key: "/home/plambri/.ssh/id_rsa.pub"

  tasks:
    - name: KVM Provision role
      include_role:
        name: kvm-deploy
      vars:
        vm_name: "{{ vm }}"
        vm_net: "{{ net }}"
        cleanup_tmp: "{{ cleanup }}"
        ssh_key: "{{ ssh_pub_key }}"
```

License
-------

BSD

Author Information
------------------

Pierguido Lambri <pierg75@yahoo.it>
