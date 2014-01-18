---
layout: post
title: Transparent encrypted partition unlocking/locking at login/unlogin in Arch Linux
description: ""
category: articles
tags: [archlinux,security,encryption]
---

### General overview for the impatients

In this article I first use cryptsetup to create a regular crypted partition. It needs to have the Linux users password as key to (un)lock the partition. I then use a pretty neat tool called [pam\_mount](http://pam-mount.sourceforge.net/). This PAM module enables volume mounting and decrypting at login.

### Pros

-   Quite simple to set up and pretty KISS
-   Usefulness of encrypted partitions but without the hassle of typing multiple passwords
-   Completely transparent for user

### Cons

-   Using Linux users passwords as uncrypting passphrase may be seen as a flaw in the security of your box because your session password (thus your uncrypting passphrase) is stored in /etc/shadow. This file resides in your root partition which, in this method, must not be crypted. See [the ending comment](#more) for a "solution".
-   Any user password change will need to be "applied" to this setup. ie removing the password from the luks keyring and adding the new one.
-   Will only work for 8 different users as all users passwords must be able to unlock the crypted parition and luks only allows 8 different passwords on its keyring.

With these pros and cons, I must say this solution suits me well: a single user on a laptop (with no real sensitive data) who knows how to modify a key on the unlocking keyring when changing the session password. So let's get started.

### Warning

-   This howto is made for GDM but with few adaptations it should work for any kind of login manager.
-   I use [pam\_mount](http://aur.archlinux.org/packages.php?ID=1976) which is provided in Arch Linux in [AUR](http://aur.archlinux.org/) aka "Not officially supported". Paranoid users should check the PKGBUILD (and the sources ;)).
-   For this to work you will have to use the same locking passphrase for your partition as the one used when login to your Linux account.
-   In this howto I encrypt my home partition (/dev/sda4) but of course one could unlock/lock any other EXCEPT ROOT.

### How to procede

The first step when crypting a partition is to backup your data as this will erase it all on the target partition. The second step beeing to properly erase all data on the parition ([why and how to do so](http://wiki.archlinux.org/index.php/LUKS#Secure_Erasure_of_the_Harddisk_Drive)).\
 To create a crypted partition, let's be sure that we have all the tools in that intention:\
 `     % pacman -Qs     local/cryptsetup 1.3.1-2 [0.50 MB] (base)         Userspace setup tool for transparent encryption         of block devices using the Linux 2.6 cryptoapi`\
 Cryptsetup belongs to the "base" group in Arch Linux so you should be fine but otherwise just "pacman -Sy cryptsetup". Now let's load the kernel modules depending on your architecture:\
 `modprobe dm-crypt aes-i586` or\
 `modprobe dm-crypt aes-x86_64` We are then going to create a regular crypted partition. I usually use the following command for that purpose but any other cypher, keysize etc would work. This will ask you for the passphrase you want to use in order to crypt/decrypt your partition. As already said, you NEED to specify here the same password as the one for your Linux account. Change /dev/sda4 with your partition:\
 `cryptsetup -c aes-xts-plain -y -s 512 luksFormat /dev/sda4`\
 Also remember that in order for other accounts to still be able to unlock this partition, you need to add all Linux account passwords as keys to unlock the partition. This is made possible through the following command. To be done for all Linux account passwords. Beware that the fist password asked here is the master password you supplied in the previous step:\
 `cryptsetup luksAddKey /dev/sda4`\
 Let's open the newly created partition:\
 `cryptsetup luksOpen /dev/sda4 home`\
 You should be able to see the mapped device:\
 `ls /dev/mapper/home`\
 Create a filesystem on it (such as ReiserFS in this example or any other):\
 `mkfs.reiserfs /dev/mapper/home`\
 There! We have an encrypted partition. Now let's get it opened/closed and mounted/unmounted at login time.

For that matter we will need pam\_mount from AUR that enables volume mounting at login:\
 `yaourt pam_mount`\
 Add the following line to /etc/fstab (notice the "noauto". We just declare it but ask it not to be mounted. Also be sure to specify the filesystem you used instead of 'reiserfs' if required):\
 `/dev/mapper/home  /home  reiserfs  defaults,noauto  0  0`\
 Add the following line to /etc/security/pam\_mount.conf.xml with the corresponding path:\
 \<pam\_mount\>\
 ...\
 \<volume fstype="crypt" path="/dev/sda4" mountpoint="/home" /\>\
 ...\
 \</pam\_mount\>\
 \
 Add the following lines to /etc/pam.d/gdm (and/or /etc/pam.d/login to perform encryption when on console):\
 `     ...     auth    optional    pam_mount.so     ...     session optional    pam_mount.so     ...`\

There! Just reboot and enjoy your fully automated encrypted home opening when you log in. If that fails well... then you can't login so there should not be any complaint ;)

### Explanation

At login time, gdm uses pam to authenticate the user. PAM will check all modules we asked it to "activate" when GDM asks for login (/etc/pam.d/gdm). PAM will thus launch pam\_mount which opens and then mounts our encrypted partition.

### More

As said in the introduction, using Linux users passwords as uncrypting passphrase may be seen as a security hole because your these passwords are stored in /etc/shadow. This file resides in your root partition which in this method must not be crypted. A possible hardening of this is just to [use SHA passwords in /etc/shadows](http://wiki.archlinux.org/index.php/SHA_Passwords).

