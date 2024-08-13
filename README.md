# Steps in `fdisk` for `/dev/sdb`

## 1. Create the Boot Partition (500 MB)

1. Press `n` to create a new partition.
2. Choose `p` for a primary partition.
3. Enter `1` for the partition number.
4. Press `Enter` to accept the default starting sector.
5. Enter `+500M` for the size.
6. Press `t` to change the partition type.
7. Enter `83` for Linux filesystem (or `b` for W95 FAT32 if using BIOS boot).

## 2. Create the EFI Partition (500 MB)

1. Press `n` to create a new partition.
2. Choose `p` for a primary partition.
3. Enter `2` for the partition number.
4. Press `Enter` to accept the default starting sector.
5. Enter `+500M` for the size.
6. Press `t` to change the partition type.
7. Enter `ef` for EFI System.

## 3. Create the Swap Partition (8 GB)

1. Press `n` to create a new partition.
2. Choose `p` for a primary partition.
3. Enter `3` for the partition number.
4. Press `Enter` to accept the default starting sector.
5. Enter `+8G` for the size.
6. Press `t` to change the partition type.
7. Enter `82` for Linux swap.

## 4. Create the Root Partition (remaining space)

1. Press `n` to create a new partition.
2. Choose `p` for a primary partition.
3. Enter `4` for the partition number.
4. Press `Enter` to accept the default starting sector.
5. Press `Enter` again to use the remaining space.

## 5. Write Changes and Exit

1. Press `w` to write the changes and exit `fdisk`.


# Formating partitions

## Format the boot partition
```
mkfs.ext4 /dev/sdb1
```

## Format the EFI partition
```
mkfs.fat -F32 /dev/sdb2
```

## Format the root partition
```
mkfs.ext4 /dev/sdb4
```

## Initialize the swap partition
```
mkswap /dev/sdb3
```

# Mount these drives 

## Mount the root partition /mnt
```
mount /dev/sdb4 /mnt
```

## Create and mount the boot partition /boot
```
mkdir /mnt/boot
```
```
mount /dev/sdb1 /mnt/boot
```

## Create and mount the EFI partition /efi
```
mkdir /mnt/boot/efi
```
```
mount /dev/sdb2 /mnt/boot/efi
```


## Enable the swap partition
```
swapon /dev/sdb3
```


# Arch base install KDE


```
pacstrap /mnt base linux linux-firmware
```

## Generate fstab 

```
genfstab -U /mnt >> /mnt/etc/fstab

```

## Arch chroot into /mnt 


```
arch-chroot /mnt
```

# Chroot tuning 



## Create user


```
useradd -m -G wheel -s /bin/bash yourname

```

## Passwd the user 

```
passwd newusername
```


## Edit visudo envs 

```
visudo
```

and enable or copy

```
%wheel ALL=(ALL) ALL
```


## Install plain clean plasma env

```
pacman -S plasma-desktop sddm vim nano networkmanager git github-cli ufw
```


## Create grub files 


```
pacman -S grub efibootmgr
```

```
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=grub
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

# Last part 

```
exit
```

```
reboot
```

# It should start with grub if not then shady boy you can try again these steps because linux is awesome <3













