
# host disk extend

这里的host是通过vmware进行管理的

## 1 通过vmware给虚拟机扩容

这步操作需要在停机状态下进行，否则会报错

## 2. 创建新的分区

### 2.1 查看磁盘分区情况

```
root@worker1:~# fdisk -l
...

GPT PMBR size mismatch (41943039 != 83886079) will be corrected by write.
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: D697BF20-0B38-4E90-B3D1-12E7B59ADF91

Device     Start      End  Sectors Size Type
/dev/sda1   2048     4095     2048   1M BIOS boot
/dev/sda2   4096 41940991 41936896  20G Linux filesystem

```
* 可以看到/dev/sda是40G，/dev/sda2是20G，还有20G的空间未使用

### 2.2 创建新的分区
```
root@worker1:~# fdisk /dev/sda

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

GPT PMBR size mismatch (41943039 != 83886079) will be corrected by write.

Command (m for help): m

Help:

  GPT
   M   enter protective/hybrid MBR

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table


Command (m for help): F

Unpartitioned space /dev/sda: 20 GiB, 21475868160 bytes, 41945055 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

   Start      End  Sectors Size
41940992 83886046 41945055  20G

Command (m for help): n
Partition number (3-128, default 3): 
First sector (41940992-83886046, default 41940992): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (41940992-83886046, default 83886046): 

Created a new partition 3 of type 'Linux filesystem' and of size 20 GiB.

Command (m for help): p
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: D697BF20-0B38-4E90-B3D1-12E7B59ADF91

Device        Start      End  Sectors Size Type
/dev/sda1      2048     4095     2048   1M BIOS boot
/dev/sda2      4096 41940991 41936896  20G Linux filesystem
/dev/sda3  41940992 83886046 41945055  20G Linux filesystem

Command (m for help): w
The partition table has been altered.
Syncing disks.
```
* fdisk用法，fdisk device, 这里的device是/dev/sda
* n, 创建新的分区， 如果打算将剩下的空间全部使用，则直接回车
* p, 查看分区情况，可以看到新的分区/dev/sda3已经创建成功
* w, 保存并退出

### 2.3 查看新分区
```
...
Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
Disk model: VMware Virtual S
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: D697BF20-0B38-4E90-B3D1-12E7B59ADF91

Device        Start      End  Sectors Size Type
/dev/sda1      2048     4095     2048   1M BIOS boot
/dev/sda2      4096 41940991 41936896  20G Linux filesystem
/dev/sda3  41940992 83886046 41945055  20G Linux filesystem
```

## 3. 格式化新分区
```
mkfs.ext4 /dev/sda3
```
* 格式化新分区，格式化成ext4文件系统


## 4. 挂载新分区

```
mount /dev/sda3 /data
```
* 挂载新分区到/data目录下


## 5. 查看新分区
```
root@worker1:~# df -h
Filesystem      Size  Used Avail Use% Mounted on
...
/dev/sda2        20G   15G  4.0G  79% /
/dev/sda3        20G   24K   19G   1% /data
...
```
* 查看新分区，可以看到新分区已经挂载成功


## 6. 开机自动挂载

```
# Add the new partition to /etc/fstab file to enable auto-mount on system boot
# Format:
# <device>  <mount point>  <filesystem type>  <mount options>  <dump>  <pass>
#
# /dev/sda3 - The device/partition to mount
# /data - The mount point directory 
# ext4 - The filesystem type
# defaults - Standard mount options
# 0 - Dump (backup) frequency (0 means no backup)
# 0 - fsck order (0 means do not check this filesystem)
echo "/dev/sda3 /data ext4 defaults 0 1" >> /etc/fstab
```
* 将新分区添加到fstab文件中，以便开机自动挂载
* 0 1 表示开机自动挂载，0 0 表示开机不自动挂载  

