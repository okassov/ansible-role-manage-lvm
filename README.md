# Ansible role for LVM

Ansible role to manage LVMs

- Configure the LVM
- Mount LVM to directory
- Resize PV to maximum size (optional)

#### Variables

```yaml

lvm_groups: []
# - vgname: ubuntu-vg
#   disks:
#     - /dev/sda5
#     - /dev/sdc
#     - /dev/sdd
#   # defines if VG should exist or be removed
#   # true or false
#   create: true
#   lvnames:
#     - lvname: swap_1
#       # Define size of lvol
#       # 100%FREE, 10g, 1024 (megabytes by default)
#       size: 5g
#       # Defines additional lvcreate options (e.g. stripes, stripesize, etc)
#       opts: ''
#       # Defines if lvol should exist or be removed
#       # true or false
#       create: true
#       # Defines filesystem to format lvol as
#       filesystem: swap
#       # Defines if filesystem should be mounted
#       mount: false
#       # Defines mountpoint for lvol
#       mntp: []
#       # Defines additional mount options (e.g. noatime, noexec, etc)
#       mopts: ''
#     - lvname: root
#       size: 40g
#       create: true
#       filesystem: ext4
#       mount: true
#       mntp: /
# - vgname: test-vg
#   disks:
#     - /dev/sdb
#   create: true
#   lvnames:
#     - lvname: test_1
#       size: 5g
#       create: true
#       filesystem: ext4
#       mount: true
#       mntp: /mnt/test_1
#     - lvname: test_2
#       size: 10g
#       create: true
#       filesystem: ext4
#       mount: true
#       mntp: /mnt/test_2
# - vgname: cinder-volumes
#   disks:
#     - /dev/cciss/c0d1
#   create: true
#   lvnames:
#   # Set to None to only create LVM VG w/out creating LVM LVOLS
#    - None

# Defines if LVM will be managed by role
# default is false to ensure nothing is changed by accident.
manage_lvm: false

### nvme to scsi device name map binary helper
ebsnvme_binary_helper_ver: '0.1.3'
ebsnvme_binary_helper_tmp: '/tmp'
ebsnvme_binary_helper_path: '/sbin/go-ebsnvme'

### nvme to scsi device name map script helper
ebsnvme_scrip_helper_path: '/usr/local/bin/ebsnvme-id'

### auto pvresize
pvresize_to_max: false
```

#### Usage

Example inventory.yaml for LVM:

```yaml
all:
  children:
    mongodb:
  vars:
    lvm_groups:
      - vgname: test-vg
        disks:
          - /dev/sdb
        create: true
        lvnames:
          - lvname: test_1
            size: 2g
            create: true
            filesystem: ext4
            mount: true
            mntp: /mnt/test_1
          - lvname: test_2
            size: 3g
            create: true
            filesystem: ext4
            mount: true
            mntp: /mnt/test_2
    manage_lvm: true
    pvresize_to_max: true

test:
  hosts:
    test-01:
      ansible_host: 192.168.1.101
```

Example of playbook.yaml

```yaml
- name: Test
  hosts: test
  roles:
    - manage-lvm
```
