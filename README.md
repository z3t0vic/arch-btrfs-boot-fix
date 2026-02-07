# 🛠️ Arch Linux Btrfs Boot Repair Guide

A practical guide to fixing boot failures on Arch Linux after performing a Btrfs snapshot rollback. This guide is specifically designed for users who encounter the "emergency shell" or "GRUB rescue" issue due to subvolume mismatches.

---

## 🔍 The Problem
After restoring a `@` (root) subvolume from a snapshot, the system often fails to boot because the **initramfs** and **bootloader** (GRUB/systemd-boot) configurations are still looking for the old subvolume state or the kernel images in `/boot` are out of sync.



---

## 🚀 Recovery Steps

### 1. Identify Partitions
Boot from your **Arch Linux Live USB** and identify your partitions:
```bash
lsblk
2. Mount and Setup
Mount the top-level Btrfs volume to ensure your snapshot is correctly named as the active root (@). Replace /dev/sdXy with your actual partition.

Bash
# Mount the parent Btrfs partition (ID 5 is the top-level)
mount -o subvolid=5 /dev/sdXy /mnt

# Rename your snapshot to @ (root) if needed
mv /mnt/@ /mnt/@_broken_backup
mv /mnt/@snapshots/your-snapshot-name /mnt/@
3. Chroot & Repair
Now, mount the corrected subvolume and the EFI partition to enter the system environment:

Bash
# Mount the restored root
mount -o subvol=@ /dev/sdXy /mnt

# Mount the EFI partition (Crucial!)
mount /dev/sdXz /mnt/boot

# Enter the system
arch-chroot /mnt
Once inside the chroot, synchronize the kernel and the bootloader:

Bash
# Regenerate initramfs
mkinitcpio -P

# Update GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg
4. Finalize
Exit the environment and reboot your system:

Bash
exit
umount -R /mnt
reboot
