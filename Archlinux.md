# Arch Linux

# Install

After open the virtual machine, First to check the internet connection. (network)
  
        ping www.baidu.com
  
If the network could connect successful, we could do the download in the next step

Then

        cfdisk

Using to set up partition of partition, I think it is more intuitive and clearer than
command gfdisk
Choose gpt, and you will set up your partition in it.

        lsblk
        
To check your partitions

        kfs. ext4(root) /dev/root_partition
        mkfs.fat -F32(efi) /dev/efi_partition
        mkswap /dev/swap_partition
        
Using these commands to format the partition mkswap and initial partition of swap

Then to mount the file

        mount /dev/root_partition
        mkdir /mnt/efi
        mount /dev/efi_partition  /mnt/efi
        
# Mirror
        
        vim /etc/pacman.d/mirrorlist
        
To find the mirrorlist, then add your mirror into the file

Then we try to begin to download:
        
        pacman -Syy
        pacstrap /mnt base base-devel linux linux-firmware lvm2 dhcpcd networkmanager vi vim nano man-db man-pages tecinfo
        
To generate fstab file

        genfstab -U /mnt >> /mnt/etc/fstab

To check fstab file

        cat /mnt/etc/fstab

# Chroot

Get into new system to set up the time, set up hostname and add information into hosts and root, passward

        arch-chroot /mnt
        ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime # hwclock --systohc
        hwclock --systobc
        date
        nano /etc/locale.gen

To delete # in the front of en_US.UTF-8 UTF-8
To set up the conf file

        vim /etc/locale.conf
        
To set up hostname
        
        vim /etc/hostname
        vim /etc/hosts
Put this into the hosts file
        
        127.0.0.1   localhost
        ::1         localhost
        127.0.1.1   myhostname.localdomain myhostname
        
        vim /etc/mkinitcpio.conf
        mkinitcpio -p

Set up new user and password
        
        useradd -m -G wheel -s /bin/bash uesrname
        passwd username
        vim /etc/sudor
delete the # in front of %wheel ALL=(ALL)

        pacman -S grub efibootmgr
        grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch
        grub-mkconfig -o /boot/grub/grub.cfg
        exit
        
Then we could restart it.

# Gnome
We try to download GUI environment --- Gnome

        ping -c 3 baidu.com
        pacman -S gnome gnome-extra
        systemctl enable gdm
        reboot

You could log into  the Gnome. Then to find terminal. Attention, if you couldn't oper terminal, go to setting to change the Language to English.

After you open terminal

        sudo pacman --S neofetch

Then you could done.
        




       
        
   
