# Arch Linux installation procedure [2020.08]

You can follow the deployment procedure in the [installation guide](https://wiki.archlinux.org/index.php/installation_guide).

1. Set the keyboard layout and system clock:
```
# loadkeys es
# timedatectl set-ntp true
```
2. Partition the disk and mount the partitions:
```
# cfdisk
# mkfs.ext4 /dev/nvme0n1p5
# mkswap /dev/nvme0n1p6
# swapon /dev/nvme0n1p6
# mount /dev/nvme0n1p5 /mnt
```
3. Deploy essential packages and configure the filesystem:
```
# pacstrap /mnt base base-devel linux linux-firmware vim
# genfstab -U /mnt >> /mnt/etc/fstab
```
4. Take control of the new filesystem and finish the installation:
```
# arch-chroot /mnt
# ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
# hwclock --systohc
# vim /etc/locale.gen
# locale-gen
# vim /etc/locale.conf
# vim /etc/vconsole.conf
```
5. Set the *root* password:
```
# passwd
```
6. Configure the network:
```
# vim /etc/hostname
- - -
  hostname
- - -
# vim /etc/hosts
- - -
  127.0.0.1 localhost
  ::1		    localhost
  127.0.1.1 `hostname'.localdomain	`hostname'
- - -
```
7. Complete the [network configuration](https://wiki.archlinux.org/index.php/Network_configuration) checking [wireless](https://wiki.archlinux.org/index.php/Network_configuration/Wireless) section if needed.

8. Choose and install a [Boot loader](https://wiki.archlinux.org/index.php/Boot_loader). GRUB or systemd-boot are good options.

9. Enable [microcode](https://wiki.archlinux.org/index.php/Microcode) updates.

10. Exit and reboot:
```
exit
umount -R /mnt
reboot
