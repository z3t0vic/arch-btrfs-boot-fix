# 🛠️ Arch Linux Btrfs Boot Repair Guide

A universal guide to fixing boot failures on Arch Linux after performing a Btrfs snapshot rollback. This method works regardless of your hardware (Intel/AMD/Nvidia) and is designed to fix "emergency shell" or "GRUB rescue" errors.

---

## 🔍 The Problem
After restoring a `@` (root) subvolume from a snapshot, the system often fails to boot because the **initramfs** and **bootloader** (GRUB/systemd-boot) configurations are out of sync with the new subvolume state.



---

## 🚀 Recovery Steps

### 1. Identify Partitions
Boot from your **Arch Linux Live USB** and identify your drive names:
```bash
lsblk
Note: Identify your Btrfs partition (e.g., /dev/sda2) and your EFI partition (e.g., /dev/sda1).

2. Mount and Setup
Mount the top-level Btrfs volume to ensure your snapshot is correctly named as the active root (@).

Bash
# Mount the parent Btrfs partition (ID 5 is the top-level)
mount -o subvolid=5 /dev/sdXy /mnt

# Rename your snapshot to @ (root) if needed
mv /mnt/@ /mnt/@_broken_backup
mv /mnt/@snapshots/your-snapshot-name /mnt/@
3. Chroot & Repair
Mount the restored subvolume and the EFI partition to enter the system environment:

Bash
# Mount the restored root
mount -o subvol=@ /dev/sdXy /mnt

# Mount the EFI partition (Required for bootloader updates)
mount /dev/sdXz /mnt/boot

# Enter the system
arch-chroot /mnt
Once inside the chroot, synchronize the kernel and the bootloader:

Bash
# Regenerate initramfs for all kernels
mkinitcpio -P

# Update GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg
4. Finalize
Exit and unmount everything safely:

Bash
exit
umount -R /mnt
reboot
