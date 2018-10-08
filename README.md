LVM Ansible role
================

Role to manage LVM Groups/Logical Volumes. Can be used to create, extend or resize LVM Groups and volumes.

Requirements
------------

Devices/disks to be part of the LVM setup must be identified prior to using this role. Ensure that you select the correct devices/disks.

Role Variables
--------------

`lvm_apply` allow backward compatibility for non-LVM
`lvm_groups` is a list containing vgs.

### vgs

- `vgname`: uniq name
- `disks`: add disks/partitions to vg (comma separated)
- `state`: present (-> create), absent (-> remove)
- `lvnames`: lv list (see bellow)

### lvnames

- `lvname`: uniq name
- `size`: define size of lvol (ex: "10G", "512M"...)
- `status`: defines if lvol should exist, mounted, exported or be removed...Values are mounted,inactive,absent
- `filesystem`: defines filesystem to format lvol as
- `mount_point`: defines mountpoint
- `mount_options`: defines mount options (comma separated)

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      vars:
        lvm_apply: true
        lvm_groups:
          - vgname: misc-vg
            disks: /dev/sda5,/dev/sdc,/dev/sdd
            state: present
            lvnames:
              - lvname: swap_1
                size: 5g
                state: mounted
                filesystem:
              - lvname: mysql
                size: 40g
                state: inactive
                filesystem: ext4
                mount_point: /var/lib/mysql
                mount_options: 'defaults,noatime'
          # VG whitout LV
          - vgname: test-vg
            disks: /dev/sdb
            state: present
            lvnames: []

      roles:
         - peopledoc.ansible-lvm

License
-------

LGPL

Donation
--------

 :star: the project is also a way of saying thank you! :sunglasses:

Author Information
------------------

- Current fork by [François TOURDE](https://github.com/FrancoisT31) for [PeopleDoc](https://www.people-doc.com)
- Based on [HanXHX/ansible-lvm](https://github.com/HanXHX/ansible-lvm) Twitter: [@hanxhx_](https://twitter.com/hanxhx_)
