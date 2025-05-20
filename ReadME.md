This playbook aims to fully install ArchLinux on a fresh machine.

- [Step 0](#step-0---minimal-bootable-system) : Minimal bootable system with installation ISO
- [Step 1](#step-1--build-core-system) : Build core system

# Step 0 - Minimal bootable system

The aim here is to install **a bare minimum bootable system**.<br>
It could have been done in Ansible too, but I prefer to do it manually due to the number of different systems I'm targeting ... 
and some of them, like ARM SBC's, are requiring manual actions, by the way.

> [!WARNING]
> For ARM SBC, take a look on [ArchARM website](https://archlinuxarm.org/) ...
> and if your SBC is not supported, you may have to build a new kernel **before** booting.

## Boot on Arch install ISO

Can be USB stick, an SD card, net boot ...

> [!CAUTION]
> Change `/dev/sda` as per your own need

##### Partitionning

You need at least :
- a boot partition : `/dev/sda1` (512MB at least)
- the root partition : `/dev/sda2`

`fdisk /dev/sda`

##### formating and mounting

```
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt
mount --mkdir /dev/sda1 /mnt/boot
```

##### Install system bootstrap

```
pacstrap -K /mnt base linux linux-firmware networkmanager vim e2fsprogs sudo openssh python
```

> [!TIP]
> Add platform specifics : typically **intel-ucode** for an intel CPU.

```
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

##### change root password
```
passwd
```
##### Install the Boot loader
```
pacman -S refind
refind-install
```

##### Finally, reboot

> [!IMPORTANT]
> In the advanced menu, you have to choose "*minimal install*" as refind-install configured USB boot device and not the hard drive.

You're now in your new environment. You need to finalize the refine installation to let it choose by default your hard drive.
```
rm /boot/refind_linux.conf
refind-install
```

##### Enable the network

```
systemctl enable NetworkManager
```
and, temporarily, enable root login. In file **/etc/ssh/sshd_config**, uncomment line `PermitRootLogin true`.

It's the time to finalize environment specifics, like enabling ZRam.

> ![image](images/level.png)
> Now you've got a minimal running system, which is able to boot autonomously.

***

# Step 1 : Build core system

On the node manager (the machine running Ansible), you may have to have **ansible** and **python-passlib** installed (`pacman -S ansible python-passlib`)

In order to ensure Ansible doesn't need a remote root password, we will associate a key between our current account and the remote's root.
```
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.0.30
```
Where **192.168.0.30** has to be changed as per the IP address of the new system. Then populate `hosts.yaml` file.

##### Configuration

- [hostname](roles/hostname)
- [timezone](roles/timezone)
- [locale](roles/locale)
- [reflector](roles/reflector)
- [buildsys_complements](roles/buildsys_complements)
- [environment](roles/environment)
- [deployer](roles/deployer)
- [users](roles/users)

More information in `buildSys.yaml` playbook.

##### Run the playbook

```
ansible-playbook BuildSys.yaml
```
> [!IMPORTANT]
> After this playbook is completed successfully, remote root login is disabled. Consequently, this playbook is not idempotent.

> ![image](images/level.png)
> Now you've got a basic installed system, configured, Ansible capable, with **root** account disabled and your user account configuration and sudo capable.
