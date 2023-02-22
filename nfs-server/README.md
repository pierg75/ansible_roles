Role Name
=========

A brief role to configure a NFS server with some exports

Requirements
------------

No specific requirements, everything needed will be installed/configured.

Role Variables
--------------

The role uses these variables:

```
number_exports: 3
base_dir: /exports/test  
allowed_address: "*"
export_options: "rw,async,no_root_squash"
```

If you want to modify any of these, set the roles variables or the hosts/group vars.
Note that these variables will be used for every export, so in this case it will end up with exports like:
```
# exportfs -v
/exports/test0  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/test1  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
/exports/test2  <world>(async,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
```

Dependencies
------------

No specific dependencies.

Example Playbook
----------------

A possible playbook with custom options is:

```
---
- name: Configure nfs server
  hosts: all
  become: true
  roles:
    - role: nfs-server
      vars:
        number_exports: 5
        export_options: "rw,sync,no_root_squash"
```

License
-------

BSD

Author Information
------------------

Pierguido Lambri