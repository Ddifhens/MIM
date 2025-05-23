#MIM #arch #setup
### 1 drive partitioning
an arch install needs the drives ready, for this we require 3 different partitions usually:

1. boot partition : fat32 , up to 1GB in size
2. Swap partition : swap, 4, 8, or 16 GB in size
3. root partition : ext4, the rest of the available space 

formatting commands
1. mkfs.fat -F 32 /dev/drive
2. mkswap /dev/drive
3. mkfs.ext4 /dev/drive

Mounting points
1. mnt/boot/efi (this directory has to be made at install with -p)
2. the swap is not mounted, it is only turned on with swapon /dev/drive
3. mount /dev/drive /mnt (mnt/data usually for extra drives) 

### Base packages install

using the command 
```
pacstrap /mnt *package* *package* *package*
```

a list of recommended packages:

base, linux, linux-firmware >> basic necessities
Vim >> text editor 
sof-firmware >> for more modern soundcards
base-devel >> AUR and basic config
grub >> boot manager 
efibootmgr >> efi boot support for grub
networkmanager >> self explanatory 


### FSTAB generation 

the creation of this file is not permanent, and changes can be made afterward that will still apply at boot time, this is specially useful to know when working for example with ntfs shared drives, at this time your root user does not have an ntfs compatible mounting software, mounting one would probably result in errors and installing this software on root is really not needed. 
so after generating this file refer to the DRIVES page to edit it after reboot.
```
genfstab /mnt > /mnt/etc/fstab
```

### changing into the new system 

now you can change root into your new system with 
```
arch-chroot /mnt
```

first create a simlink to adjust locale settings 
```
ln -sf /usr/shar/zoneinfo/Europe/Lisbon /etc/localtime 
```
run "date" to check the time is correct

synchronize system clock with
```
hwclock --systohc
```

### generating a locale 

with your text editor of choice open /etc/locale.gen 

and un-comment out the desired locales on your machine. 
The most usual one being 
en_US.UTF-8 UTF-8  >> for the standard english locale 

after doing this you can generate the local using 
```
locale-gen
```

afterwards you must specify your local in 
/etc/local.conf

with the locale mentioned above we would write into this file 
```
LANG=en_US.UTF-8
```

### name the system 

edit  /etc/hostname and save the machine name inside 

### set a root password 

```
passwd *your password*
```

### creating an user 

you can run the next command to create an user on your machine, assuming you want to create a home directory ("-m") and give them root privileges by adding them to the wheel group ("-G wheel").
```
useradd -m -G wheel -s /bin/bash *username*
```

add a password with the same command as before 
```
passwd *username*
```

next we want to actually give the wheel group root privileges by editing the visudo file by doing 
```
EDITOR=vim visudo 
```

and un-commenting out the lines about giving the wheel privileges, optionally you can give privileges without a password even. 

### enabling necessary services 

enabling networkmanager 
```
systemctl enable NetworkManager
```
note the capitalizations

installing grub to the drive 
```
grub-install /dev/drive
```

configuring grub 
```
grub-mkconfig -o /boot/grub/grub.cfg
```
note the "drive" in this case denotes the full drive and not a partition 
### log out, unmount and reboot 

```
exit 
umount -a 
reboot 
```


### installing DE 
before enabling the GUI be sure to install a terminal emulator, lest you be locked out of terminal access. 

plasma: 
```
sudo pacman -S sddm plasma 
sudo systemctl enable --now sddm 
```