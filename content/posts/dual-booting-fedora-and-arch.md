+++
title = 'Dual Booting Fedora and Arch'
date = 2023-10-09T10:30:32-04:00
draft = false
+++

These are some notes on how to use Fedora's GRUB to dual boot additional Linux distributions such as Arch.

I already had Arch installed, but I wanted to install Fedora and use Fedora's boot loader. Fedora is always renaming the kernel and creating additional GRUB entries, so it makes sense to use Fedora's boot loader and just add the Arch kernel as a static entry where the name doesn't change (reword that).

## Preparation
So I booted Systemrescuecd and used Gparted to shrink my arch rootfs and I deleted my EFI partition, which I had been mounting to /boot/efi. My /boot is on the same partition as my rootfs.
I also moved my /home to a separate partition, which I think some people have already done. 

Later I realized that I probably could have skipped sysrescuecd done this with the Fedora installer since it is a live environment.

So what I did was add an entry to /etc/grub.d/40_custom as follows:

	menuentry "Arch Linux" {
	  UUID=fd46f235-0789-152a-b79c-8edh5ff65927
	  search --fs-uuid $UUID --set root
	  linux /boot/vmlinuz-linux root=UUID=$UUID ro
	  initrd /boot/initramfs-linux.img
	}

Thank you to ray731 on the Arch forums for providing the GRUB [config example](https://bbs.archlinux.org/viewtopic.php?id=234982).

After editing the config, it's necessary to regenerate the GRUB config.

	$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg



References
1. https://bbs.archlinux.org/viewtopic.php?id=234982
2. 