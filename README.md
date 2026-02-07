# arch-btrfs-boot-fix
A quick guide to fix Arch Linux boot issues after Btrfs snapshot rollbacks.
# 🛠️ Arch Linux Btrfs Boot Repair Guide

A practical guide to fixing boot failures on Arch Linux after performing a Btrfs snapshot rollback.

## 📖 The Problem
After restoring a `@` (root) subvolume, the system often fails to boot because the **initramfs** and **GRUB/systemd-boot** configurations are out of sync with the current subvolume state.


---

## 🚀 Recovery Steps

### 1. Boot from Arch ISO
Boot into your **Arch Linux Live USB** and identify your partitions:
```bash
lsblk
