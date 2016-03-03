+++
date = "Tue Mar  1 13:58:00 CET 2016"
title = "Install Archlinux on Remote server"

+++

After meeting with a bunch of hardcore systems geeks, I decided to put my effort
back into a secure & custom server that I will *actually* use ;)
 Here's my steps for ArchLinux:

# Installing ArchLinux on remote server #

## Drives ##
First you need to setup your drives correctly. I use fdisk and you can find
any decent tutorial on it easily.
For encrypting one of the partitions, I follow the archlinux guide
(https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_a_non-root_file_system).
I did not try to encrypt the whole filesystem because it seemed a bit overkill
for my needs. I simply have /dev/sda3 encrypted where all my datas are.

```
cryptsetup -y -v luksFormat /dev/sda3
cryptsetup open /dev/sda3 cryptroot
mkfs -t ext4 /dev/mapper/cryptroo
```

## System ##

For installing on the drives, they need to be unmounted so you need to boot on
"rescue" image that your hosting provider is surely providing you. Mine was
offering a Debian image.

For the rest of the steps, simply follow the guide on ArchLinux
(https://wiki.archlinux.org/index.php/Install_from_existing_Linux).

The following notes are simply some stuff that kept me crazy during a lot of
hours.

## Notes ##

* Install `haveged` before running `pacman-key --init`, it'll be WAY much
  faster.
* DON'T forget to copy /etc/resolv.conf from the rescue distribution to the
  arhclinux one, or use your own (openDNS forexample).
* Change the udev rules so you have `eth0` instead of .. whatever:
```
[root@***** ~]# cat /etc/udev/rules.d/10-network.rules
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="**:**:**:**:**:**",NAME="eth0"}
```
* use netctl (for lxc ease-of-use containers network):
```
[root@**** ~]# cat /etc/netctl/ethernet-static
Description='Server ethernet connection'
Interface=eth0
Connection=ethernet
IP=static
Address=('****/24')
Gateway='****'
DNS=('****')
```
* Don't forget to `netctl enable ethernet-static`
* Don't forget to `systemctl enable sshd`
* For non official packages, install yaourt:
```
[root@****] cat /etc/pacman.conf
# ####
# ...
[archlinuxfr]
 SigLevel = Never
 Server = http://repo.archlinux.fr/$arch
```
