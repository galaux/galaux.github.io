---
layout: post
title: UEFI and Linux killed my Grub
description: ""
category: articles
tags: [archlinux,boot]
---

I bought myself a new laptop the other day: an [Asus N56VZ](http://www.asus.com/Notebooks/Multimedia_Entertainment/N56VZ/#specifications). This gave me the opportunity to dive back into the Linux boot stuff and I actually figured a lot had changed. Bye bye old [Bios](http://en.wikipedia.org/wiki/BIOS) and [MBR](http://en.wikipedia.org/wiki/Master_boot_record) ; welcome [UEFI](http://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface) and [GPT](http://en.wikipedia.org/wiki/GUID_Partition_Table).

So while reading wiki articles and blog posts in order to figure out what I should do to install and boot a brand new Arch Linux, I learnt a lot about GRUB. The most interesting one is the fact that it actually performs two differents things. It first manages which kernel you want to boot (it is thus a "boot manager") and after you selected one, it actually loads it (also making it a "boot loader"). And this really matters because new UEFI are capable of handling boot management (the part where you choose the OS you want to boot). So GRUB is only left with the boot loading part. And this is where the fun enters the party : the [Linux kernel](http://www.kernel.org/), in its infinite wisdom, [has the necessary code to act as a boot loader](https://lkml.org/lkml/2011/10/17/81) since version 3.3.0. I guess you see where I'm going: booting is then possible without any third party boot loader such as GRUB, [Lilo](http://en.wikipedia.org/wiki/LILO_(boot_loader)) etc. UEFI can directly hand over control to your distribution's kernel which acts as its own boot loader.

This article repeats what has already been explained in [this post by Rod Smith](http://www.rodsbooks.com/efi-bootloaders/efistub.html) (thank you very much for the great article) and adds a trick that some might not like. So let us explain it here so that you will not get shocked after I made you format your partitions :).

Basically the technique explained in [Rod Smith's post](http://www.rodsbooks.com/efi-bootloaders/efistub.html) lets you create a boot entry that instructs the UEFI to load a kernel located (inside the boot partition) at `<BOOT_PARTITION>\EFI\my_os\vmlinuz-linux` (I use backslash as a UEFI boot partition must be `FAT` formatted and backslash must be used when handling UEFI boot entries with `efibootmgr`). The only issue you are left with is "maintenance". The reason is because EFI uses a standard folder hierarchy on the boot partition

    <BOOT_PARTITION>\EFI\
        my_os\
        other_os\
        yet_another_os\

And this `<BOOT_PARTITION>` must be mounted in `/boot/efi`

So next time you will update the kernel, the packet manager will install the kernel (`vmlinuz-linux`) in `/boot`. But the UEFI is instructed to load the one located in `<BOOT_PARTITION>\EFI\my_os\`. So you will need to make sure the newly installed kernel and the generated `initramfs` are copied inside the `<BOOT_PARTITION>\EFI\my_os` folder. There is [a solution explained in the Arch Linux wiki](https://wiki.archlinux.org/index.php/UEFI_Bootloaders#Sync_EFISTUB_Kernel_in_UEFISYS_partition_using_Mkinitcpio_hook) involving a `mkinitcpio` HOOK trick (thus only usable with Arch Linux). The idea is to create a HOOK with a loop to wait for the initramfs generation to complete and then copies the files on the boot partition. But this does not look really clean to me and I would have liked something more KISS.

So here is what I propose: do not mount the boot partition in `/boot/efi` and do not use the standard EFI folder hierarchy in the boot partition. Instead, mount the boot partition in `/boot`. The main benefit beeing that next time you update the kernel (`pacman -S linux`), the kernel (`vmlinuz-linux`) and the newly generated `initramfs` image are installed by pacman in `/boot`, which is the root of the boot partition (as the boot partition is mounted in `/boot`). The last step is to create an entry on the EFI table that loads `<BOOT_PARTITION>\vmlinuz-linux` (instead of `<BOOT_PARTITION>\EFI\my_os\vmlinuz-linux`). Cons are you are not cleanly separating each OS boot files in their own subdirectory. So if you install another OS, you will get booting files for all OSes in `<BOOT_PARTITION>\` or some in `<BOOT_PARTITION>\` and some in `<BOOT_PARTITION>\EFI\other_os`. But this is a very minor drawback especially if you do not plan to install another OS on that computer.

In this article, I explain how I made what is previously explained while installing a brand new Arch Linux. Steps are actually simplier than it looks and they can easily be done on an already installed Arch Linux or maybe even on an other distribution if you know your way.

Installing Arch Linux with direct UEFI boot
-------------------------------------------

Install your Arch Linux "as usual" (following the official install guide for instance) When "partitionning", create a boot partition (mine was a `GPT` - `FAT32` is compulsory for UEFI) and mount it as `/mnt/boot`.\
 While hitting the "Boot loader" part, do not install GRUB but instead install `efibootmgr` (the UEFI tool):

~~~~ {.markup}
# pacstrap /mnt efibootmgr
~~~~

This tool will let you create/read/delete entries on the UEFI boot array Go on with the install procedure. After performing the "`genstab`" step (`genfstab -p /mnt >> /mnt/etc/fstab`), you should get inside your `/mnt/etc/fstab` a line to mount the boot partition as `/boot`. Go ahead with installation. After you chrooted into you installation dir (`/mnt`) you should see your boot partition mounted. If not then mount it.

~~~~ {.markup}
# mount | grep boot
/dev/sda1 on /boot type vfat <... blahblahblah>
~~~~

Go on with the install procedure until you hit "Configure the bootloader". As previously explained, no need to install GRUB. The only thing we need to do is create an entry on the UEFI that loads the kernel that pacman installs as `/boot/vmlinuz-linux`. We are going to use the `efibootmgr` binary we installed previously. So we will want to load the corresponding module:

~~~~ {.markup}
modprobe efivars
~~~~

As this kernel is actually installed directly at the root of `<BOOT_PARTITION>\`, we can use the following command (with customization):

~~~~ {.markup}
echo "root=UUID=52c2c3d9-52a3-4ebc-95e3-1fcaeeba4a57 ro rootfstype=ext4 add_efi_memmap initrd=initramfs-linux.img init=/bin/systemd" \
    | iconv -f ascii -t ucs2 \
    | efibootmgr --create --gpt --disk /dev/sda --part 1 --label 'Arch Linux' --loader 'vmlinuz-linux' --append-binary-args -
~~~~

-   "`root=UUID="` Must hold your root partitin UUID. Use `root=` if you prefer the udev "`sda`" way.
-   "`rootfstype="` Must hold your root partition filesystem type
-   "`initrd=initramfs-linux.img"` The name of the `initrd` image to boot. This should not change for Archers.
-   "`init=/bin/systemd"` Just if you use systemd during initial Arch Linux install of course. Ommit it otherwise.

-   "`--create`" instructs efibootmgr to create the entry on the UEFI table
-   "`--disk <CHANGE_ME>`" the hard drive device on which to write the entry
-   "`--part <CHANGE_ME>`" the partition of the hard drive on which to write the entry
-   "`--label <CHANGE_ME>`" The string you want to see when choosing which OS to boot
-   "`--loader 'vmlinuz-linux'`" The kernel file. This should not change for Archers. Note that we specify here a file that is at the root of the (boot) partition as explained earlier

Note to Archers: one can also add a similar entry with the `fallback` initramfs image. Please note the dash ("`-`") at the end of the command. Also note that we first echo parameters, then pipe them through iconv to change encoding. This could be investigated after as it looks like efibootmgr seems to accept ascii characters as explained by its man page. One can also see the result by just issuing "efibootmgr" which will print the current entries and their boot order. Side note for [Asus N56VZ](http://www.asus.com/Notebooks/Multimedia_Entertainment/N56VZ/#specifications) (and maybe also to similar computers/motherboards): you can also add a UEFI shell in your boot partition (`<BOOT_PARTITION>`) name it `shellx64.efi` for `x86_64`. See the last paragraph of this article for more about this. Afterwards, if you are installing your Arch Linux, do not forget to finish the install procedure (only thing left: change root password).

Now, just reboot and you should enjoy a plain UEFI direct to kernel boot! If your computer does not seem to boot on the right entry, enter the BIOS with your usual magic keystroke and navigate to a UEFI or boot option. You should see all valid entries UEFI found. Select yours and press enter. If you added the `shellx64.efi` ASUS firmware in the `BOOT_PARTITION` you should also have a supplementary entry here that will get you to a UEFI command line interface. This will greatly help you if you cannot boot the previously installed OS. Just enter `fs0:` to select your sda1 partition and then enter the previous kernel parameters to boot your distribution:

~~~~ {.markup}
root=UUID= ro rootfstype= add_efi_memmap initrd=initramfs-linux.img init=/bin/systemd ("init=/bin/systemd" if required)
~~~~

In case something went wrong... well you cannot boot so you cannot complain here : write me a letter! (funny how [I seem to be keen on remotly breaking people's computer]({{ site.url }}/transparent-encrypted-partition-unlocking-locking-at-login-unlogin-in-arch-linux). I really should see someone about that...).

