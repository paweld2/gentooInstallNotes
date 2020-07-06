# Installation commands for uxmal installation

# bios changes

# partitions
 
parted -a optimal --script /dev/sda \
    mklabel gpt \
    mkpart primary 1MiB 3MiB \
    name 1 grub \
    set 1 bios_grub on \
    mkpart primary fat32 3MiB 1GiB \
    name 2 boot \
    set 2 BOOT on \
    mkpart primary 1GiB 8GiB \
    name 3 swap\
    mkpart primary 8GiB 100% \
    name 4 root \
    print


# File systems

mkfs.vfat -F32 /dev/sda2
mkswap /dev/sda3
swapon /dev/sda3
mkfs.ext4 /dev/sda4

#Install

mount /dev/sda4 /mnt
mkdir /mnt/boot
mount /dev/sda2 /mnt/boot
cd /mnt

pacstrap /mnt base linux linux-firmware

genfstab -U /mnt >> /mnt/etc/fstab

arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Europe/Brussels /etc/localtime
hwclock --systohc

pacman -Suy netctl
pacman -Suy grub
pacman -Suy neovim
pacman -Suy openssh
pacman -Suy sudo
pacman -Suy mdadm
pacman -Suy htop
pacman -Suy cryptsetup

useradd -m -G wheel pawel

mdadm --assemble --scan

/etc/netctl/enp4s0

cp /etc/netctl/examples/ethernet-static /etc/netctl/INTERFACE
# Edit network configuration
netctl enable INTERFACE



cryptsetup luksOpen /dev/md2 data
mount /dev/mapper/vg-root /storage/data/root
mount /dev/mapper/vg-home /storage/data/home
mount /dev/mapper/vg-shared /storage/data/shared
mount /dev/mapper/vg-data /storage/data/data/
mount /dev/mapper/vg-www /storage/data/www/

