This playbook aims to fully install ArchLinux on a fresh machine ... as per my own needs.

# Step 0 - Minimal system install

The aim here is to install **a bare minimum bootable system**.<br>
It could have been done in Ansible too, but I prefer to do it manually due to the number of different systems I'm targeting ... 
and some of them, like ARM SBC's, are requiring manual actions, by the way.

> [!WARNING]
> For ARM SBC, take a look on [ArchARM website](https://archlinuxarm.org/) ...
> and if your SBC is not supported, you may have to build a new kernel **before** booting.

## Boot on Arch install ISO

Can be USB stick, an SD card, net boot ...

## Filesystems

> [!CAUTION]
> Change `/dev/sda` as per your own need

### Partitionning

You need at least :
- a boot partition : `/dev/sda1` (512MB at least)
- the root partition : `/dev/sda2`

`fdisk /dev/sda`

### formating

```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```

