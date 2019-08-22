# bios changes

# partitions
parted -a optimal /dev/sda
mklabel gpt
unit mib
mkpart primary 1 3
name 1 grub
set 1 bios_grub on
print

mkpart primary fat32 3 515
name 2 boot
set 2 BOOT on

mkpart primary 515 -1
name 3 lvm
print
quit

# File systems


# Encrypted partition
To randomize disks
# cryptsetup open --type plain -d /dev/urandom /dev/<block-device> to_be_wiped
cryptsetup open --type plain -d /dev/urandom /dev/sda3 to_be_wiped
dd if=/dev/zero of=/dev/mapper/to_be_wiped status=progress

#to encrypt boot partition - but no working for now.
cryptsetup luksFormat --type luks1 /dev/sda2
mkfs.vfat -F32 /dev/mapper/cryptboot
mkfs.vfat -F32 /dev/sda2

cryptsetup luksFormat -c aes-xts-plain64:sha512 -s 512 /dev/sda3
tikal: fovodwafOd16
tulum:  ricPhagyes

cryptsetup luksOpen /dev/sda3 lvm
lvm pvcreate /dev/mapper/lvm
vgcreate vg0 /dev/mapper/lvm
lvcreate -L 30G -n root vg0
lvcreate -L 180G -n var vg0
lvcreate -l 100%FREE -n home vg0

mkfs.ext4 /dev/mapper/vg0-root
mkfs.ext4 /dev/mapper/vg0-var
mkfs.ext4 /dev/mapper/vg0-home


#Install gentoo

mkdir /mnt/gentoo
mount /dev/mapper/vg0-root /mnt/gentoo
mkdir /mnt/gentoo/var
mount /dev/mapper/vg0-var /mnt/gentoo/var
cd /mnt/gentoo

#Download stage
wget https://mirror.netcologne.de/gentoo/releases/amd64/autobuilds/current-stage3-amd64-hardened-selinux%2Bnomultilib/stage3-amd64-hardened-selinux%2Bnomultilib-20190724T214501Z.tar.xz
wget https://mirror.netcologne.de/gentoo/releases/amd64/autobuilds/current-stage3-amd64-hardened-selinux%2Bnomultilib/stage3-amd64-hardened-selinux%2Bnomultilib-20190724T214501Z.tar.xz.CONTENTS
wget https://mirror.netcologne.de/gentoo/releases/amd64/autobuilds/current-stage3-amd64-hardened-selinux%2Bnomultilib/stage3-amd64-hardened-selinux%2Bnomultilib-20190724T214501Z.tar.xz.DIGESTS
wget https://mirror.netcologne.de/gentoo/releases/amd64/autobuilds/current-stage3-amd64-hardened-selinux%2Bnomultilib/stage3-amd64-hardened-selinux%2Bnomultilib-20190724T214501Z.tar.xz.DIGESTS.asc



openssl dgst -r -sha512 stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz
sha512sum stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz
cat stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz.DIGESTS
openssl dgst -r -whirlpool stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz.CONTENTS

gpg --recv-keys 0xBB572E0E2D182910
gpg --verify stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz.DIGESTS.asc

tar xvpf stage3-amd64-hardened-selinux+nomultilib-20190724T214501Z.tar.xz --xattrs --numeric-owner

# Setup make.conf

nano -w /mnt/gentoo/etc/portage/make.conf
Change content as required, with:

POLICY_TYPES="targeted"
USE="peer_perms open_perms ubac"

# repo config

mkdir /mnt/gentoo/etc/portage/repos.conf
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf


## CHROOT
cp /etc/resolv.conf /mnt/gentoo/etc/resolv.conf

mount -t proc proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev

test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount -t tmpfs -o nosuid,nodev,noexec shm /dev/shm
chmod 1777 /dev/shm

chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) $PS1"

# Basic setup
passwd
tikal: NivtivIo
tulum: pay6bnisped

emerge-webrsync

# select profile
eselect profile list
eselect profile set default/linux/amd64/17.1/no-multilib/hardened/selinux

#Continue setup
echo "Europe/Warsaw" > /etc/timezone
echo "Europe/Brussels" > /etc/timezone
emerge --config sys-libs/timezone-data

# locale:
nano -w /etc/locale.gen
 
 content:
en_US.UTF-8 UTF-8
pl_PL.UTF-8 UTF-8

locale-gen
eselect locale set en_US.utf8
env-update && source /etc/profile

# Configure fstab

Patition info:
blkid


create fstab with uuid.
UUID=56BA-556C                                  /boot           vfat            noauto,noatime  1 2
UUID=caf288cd-63fe-4f8f-bdd9-3594f930d347       /               ext4            defaults        0 1
UUID=63e13dd3-65ca-495a-8c31-b508d897f988       /var            ext4            defaults        0 1
UUID=fc1ef2ff-4bea-47a5-aadf-a8c8c3513e17       /home           ext4            defaults        0 1
# tmps
tmpfs                                           /tmp            tmpfs           size=4Gb,defaults,nodev,noexec,nosuid,rootcontext=system_u:object_r:tmp_t        0 0
tmpfs                                           /run            tmpfs           size=100M,mode=0755,nosuid,nodev,rootcontext=system_u:object_r:var_run_t       0 0
tmpfs		                                    /var/tmp/portage	tmpfs	size=8G,uid=portage,gid=portage,mode=775,noatime	0 0
# shm
shm                                             /dev/shm        tmpfs           nodev,nosuid,noexec,defaults,noexec,nosuid,rootcontext=system_u:object_r:tmp_t 0 0


# Install tools
emerge -av -j5 pciutils gentoolkit eix cryptsetup grub


#emerge kernel:
emerge -av sys-kernel/gentoo-sources
config kernel, look 
https://wiki.gentoo.org/wiki/Dm-crypt#Kernel_Configuration
make -j7 && make modules_install && make install

#grub
echo "sys-boot/grub:2 device-mapper" >> /etc/portage/package.use/sys-boot
emerge -av grub
GRUB_CMDLINE_LINUX="dolvm crypt_root=UUID=6a7a642a-3262-4f87-9540-bcd53969343b root=/dev/mapper/vg0-root root_trim=yes"

#TODO add password to grub
grub-mkpasswd-pbkdf2
https://wiki.gentoo.org/wiki/Security_Handbook/Bootloader_security


grub-install /dev/sda


#initramfs
emerge -av dracut

dracut --hostonly /boot/initramfs-4.19.57-gentoo.img 4.19.57-gentoo

dracut --print-cmdline
rd.luks.uuid=luks-0dc45283-77c6-42ef-8216-479fbb7aba38 rd.lvm.lv=vg0/root root=/dev/mapper/vg0-root rootfstype=ext4 rootflags=rw,relatime,data=ordered

 to disable some: rd.luks=0 rd.lvm=0 rd.md=0 rd.dm=0 rd.luks.crypttab=0

Then add output to /etc/default/grub in GRUB_CMDLINE_LINUX

grub-mkconfig -o /boot/grub/grub.cfg

#install tools


emerge -av -j3 net-misc/dhcpcd sys-kernel/linux-firmware
emerge --ask 



exit
cd
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount /mnt/gentoo{/boot,/sys,/proc,}
reboot

## Setup users
semanage user -l
useradd -G wheel,users -Z staff_u pawel
passw pawel
tulum: GovNeytjap3422
tikal: GovNeytjap3422

### to reenter the gentoo system after systemrescue reboot:

cryptsetup luksOpen /dev/sda3 lvm

cryptsetup luksOpen /dev/sda2 cryptboot


mount /dev/mapper/vg0-root /mnt/gentoo
mount /dev/mapper/vg0-var /mnt/gentoo/var


mount -t proc proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
l
test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
mount -t tmpfs -o nosuid,nodev,noexec shm /dev/shm
chmod 1777 /dev/shm

chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) $PS1"


# configurate

hostname: /etc/conf.d/hostname
emerge -av -j7 sys-process/cronie app-admin/syslog-ng
rc-update add syslog-ng default
rc-update add cronie default
rc-update add sshd default

#TODO add logcheck


#TODO after install change fstab to readonly
https://wiki.gentoo.org/wiki/Security_Handbook/Mounting_partitions

#TODO squid on network