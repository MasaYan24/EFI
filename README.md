# linux_basics

## EFI パーティションを切る方法
以下の方法で EFI パーティションを作る。
```sh
sudo parted /dev/sdh mklabel gpt
sudo parted /dev/sdh mkpart efi 0% 512M
sudo mkfs.fat -F32 /dev/sdh1
sudo parted /dev/sdh set 1 esp on
```
確認
```sh
# sudo fdisk -l
Disk /dev/sdh: 1.86 TiB, 2048408248320 bytes, 4000797360 sectors
Disk model: P80-2TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: A578BFDD-780D-47D4-B922-96CEB395C4B8

Device      Start        End    Sectors  Size Type
/dev/sdh1    2048     999423     997376  487M EFI System
```
残りのパーティションを Linux filesystem にする。
```sh
sudo parted /dev/sdh mkpart primary ext4 512M 100%
sudo mkfs.ext4 /dev/sdh2
```
