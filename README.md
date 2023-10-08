# Gentoo Systemd LLVM Hardened
Configuration files for my Gentoo Linux system

# Partition Scheme
As per the [UAPI Group's Boot Specification](https://uapi-group.org/specifications/specs/boot_loader_specification/), I have an ESP (efi system partition) and a Boot Partition. Presently, they are both of type vfat32, but this may change in the future. The ESP is of size 512MB and the Boot Partition is of size 4GB. The rest of the drive is my root partition, with luks and btrfs. I normally use the following options with luks:
```bash
cryptsetup --type luks2 --hash sha512 --cipher aes-xts-plain64 --key-size 512 --pbkdf argon2id /dev/nvme0n1p3
```
I have the following subvolumes for my root partition:
 * rootfs at /
 * home at /home
 * repos at /var/db/repos
 * binpkgs at /var/cache/binpkgs
 * distfiles at /var/cache/distfiles
 * log at /var/log
 * tmp at /var/tmp
 * snapshots (unmounted by default)

I also create an additional /system directory where the entire btrfs filesystem is mounted at for backups automatically.

Putting this in a table:

| Partition      | Gdisk Hex Code | Size         | Filesystem   | Description          |
|----------------|----------------|--------------|--------------|----------------------|
| /dev/nvme0n1p1 | EF00           | 512 MB       | Fat32        | EFI System Partition |
| /dev/nvme0n1p2 | EA00           | 4 GB         | Fat32        | XBOOTLDR Partition   |
| /dev/nvme0n1p2 | 8300           | Rest of Disk | LUKS + BTRFS | Root Partition       |
