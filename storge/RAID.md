---
title: "دليل إعداد RAID في RHEL 9 باستخدام mdadm"
tags:
  - linux
  - raid
  - mdadm
  - rhel
  - storage
  - sysadmin
aliases:
  - RAID Configuration
  - دليل RAID
---

# دليل إعداد RAID في RHEL 9 باستخدام mdadm

> [!info] نظرة عامة
> يغطي هذا الدليل إعداد أنواع RAID المختلفة (0، 1، 5، 6، 10) على نظام RHEL 9 باستخدام أداة `mdadm`. جميع الأوامر والoutpout موثقة كما وردت في الدليل الأصلي.

---

## RAID 0 (ZERO)

> [!warning] تنبيه مهم حول RAID 0
> لا يوفر RAID 0 أي تكرار (Redundancy). في حال فشل أي قرص، تُفقد المصفوفة بالكامل ولا يمكن إعادة بنائها. يجب دائمًا الاحتفاظ بنسخ احتياطية عند استخدام RAID 0.

### المتطلبات
- قرصان فارغان على الأقل (مثال: `/dev/sda` و `/dev/sdb`).
- التأكد من أن الأقراص غير مستخدمة وغير محمّلة (unmounted).

### 1. التحقق من الأقراص المتاحة

```bash
lsblk
```

> [!example]- outpout `lsblk`
> ```text
> [root@appsv01 ~]# lsblk
> NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
> sda           8:0    0    5G  0 disk
> sdb           8:16   0    5G  0 disk
> sdc           8:32   0    5G  0 disk
> sdd           8:48   0    5G  0 disk
> sr0          11:0    1   11G  0 rom
> nvme0n1     259:0    0   30G  0 disk
> ├─nvme0n1p1 259:1    0    1G  0 part /boot
> └─nvme0n1p2 259:2    0   29G  0 part
>   ├─rhel-root 253:0    0   26G  0 lvm  /
>   └─rhel-swap 253:1    0    3G  0 lvm  [SWAP]
> [root@appsv01 ~]#
> ```

### 2. إنشاء مصفوفة RAID 0

```bash
mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sda /dev/sdb
```

> [!example]- outpout RAID 0
> ```text
> [root@appsv01 ~]# mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sda /dev/sdb
> mdadm: chunk size defaults to 512K
> mdadm: Defaulting to version 1.2
> mdadm: array /dev/md0 started.
> [root@appsv01 ~]#
> ```

> [!note] شرح المعاملات
> - `/dev/md0`: اسم جهاز RAID.
> - `--level=0`: تحديد مستوى RAID 0.
> - `--raid-devices=2`: استخدام قرصين.

### 3. التحقق من حالة المصفوفة

```bash
cat /proc/mdstat
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsv01 ~]# cat /proc/mdstat
> Personalities : [raid0]
> md0 : active raid0 sdb[1] sda[0]
>       10475520 blocks super 1.2 512k chunks
>
> unused devices: <none>
> [root@appsv01 ~]#
> ```

### 4. إنشاء نظام ملفات

```bash
mkfs.ext4 /dev/md0
```

> [!example]- outpout `mkfs.ext4`
> ```text
> [root@appsv01 ~]# mkfs.ext4 /dev/md0
> mke2fs 1.46.5 (30-Dec-2021)
> Creating filesystem with 2618880 4k blocks and 655360 inodes
> Filesystem UUID: d8ba745-b5b1-44ca-b566-bdae58a7998e
> Superblock backups stored on blocks:
>         32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632
>
> Allocating group tables: done
> Writing inode tables: done
> Creating journal (16384 blocks): done
> Writing superblocks and filesystem accounting information: done
>
> [root@appsv01 ~]#
> ```

### 5. تحميل المصفوفة

```bash
mkdir /mnt/raid0
mount /dev/md0 /mnt/raid0
df -h
```

> [!example]- outpout `df -h`
> ```text
> [root@appsv01 ~]# mkdir /mnt/raid0
> [root@appsv01 ~]# mount /dev/md0 /mnt/raid0
> [root@appsv01 ~]# df -h
> Filesystem           Size  Used Avail Use% Mounted on
> devtmpfs             4.0M     0  4.0M   0% /dev
> tmpfs                1.8G     0  1.8G   0% /dev/shm
> tmpfs                726M  9.7M  716M   2% /run
> /dev/mapper/rhel-root 26G  5.0G   22G  19% /
> /dev/nvme0n1p1       960M  455M  506M  48% /boot
> tmpfs                363M   52K  363M   1% /run/user/42
> tmpfs                363M   36K  363M   1% /run/user/0
> /dev/md0             9.8G   24K  9.3G   1% /mnt/raid0
> [root@appsv01 ~]#
> ```

### 6. جعل التحميل دائمًا (Persistent)

```bash
blkid /dev/md0
```

> [!example]- outpout `blkid`
> ```text
> [root@appsv01 ~]# blkid /dev/md0
> /dev/md0: UUID="d81ba745-b5b1-44ca-b566-bdae58a7998e" TYPE="ext4"
> [root@appsv01 ~]#
> ```

أضف السطر التالي إلى `/etc/fstab`:

```bash
UUID=d81ba745-b5b1-44ca-b566-bdae58a7998e /mnt/raid0 ext4 defaults 0 0
```

ثم تحقق:

```bash
mount -a
df -h
```

> [!example]- outpout `mount -a` و `df -h`
> ```text
> [root@appsv01 ~]# vim /etc/fstab
> [root@appsv01 ~]# mount -a
> [root@appsv01 ~]# df -h
> Filesystem      Size  Used Avail Use% Mounted on
> devtmpfs        4.0M     0  4.0M   0% /dev
> tmpfs           1.8G     0  1.8G   0% /dev/shm
> tmpfs           726M   9.7M  716M   2% /run
> /dev/mapper/rhel-root 26G  5.0G   22G  19% /
> /dev/nvme0nlp1   960M  455M  506M  48% /boot
> tmpfs           363M   52K  363M   1% /run/user/42
> tmpfs           363M   36K  363M   1% /run/user/0
> /dev/md0        9.8G   24K  9.3G   1% /mnt/raid0
> [root@appsv01 ~]#
> ```

### 7. عرض تفاصيل RAID

```bash
mdadm --detail /dev/md0
```

> [!example]- outpout `mdadm --detail`
> ```text
> [root@appsv01 ~]# mdadm --detail /dev/md0
> /dev/md0:
>            Version : 1.2
>      Creation Time : Fri Apr 25 22:24:58 2025
>         Raid Level : raid0
>         Array Size : 10475520 (9.99 GiB 10.73 GB)
>       Raid Devices : 2
>      Total Devices : 2
>     Persistence : Superblock is persistent
>        Update Time : Fri Apr 25 22:24:58 2026
>              State : clean
>     Active Devices : 2
>    Working Devices : 2
>     Failed Devices : 0
>      Spare Devices : 0
>             Layout : original
>         Chunk Size : 512K
> Consistency Policy : none
>               Name : appsv01.example.com:0 (local to host appsv01.example.com)
>               UUID : 88f908e7:78684f17:c66acb5f:15f95023
>             Events : 0
>
>     Number   Major   Minor   RaidDevice State
>        0       8       0        0      active sync   /dev/sda
>        1       8      16        1      active sync   /dev/sdb
> [root@appsv01 ~]#
> ```

---

## RAID 1 (ONE)

> [!info] تعريف RAID 1
> يعتمد RAID 1 على النسخ المتطابق (Mirroring)، مما يوفر تكرارًا كاملًا للبيانات. يتطلب قرصين على الأقل.

### المتطلبات
- قرصان فارغان على الأقل.
- التأكد من عدم استخدامهما.

### 1. التحقق من الأقراص

```bash
lsblk
```

> [!example]- outpout `lsblk`
> ```text
> [root@appsvr01 ~]# lsblk
> NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
> sda           8:0    0    5G  0 disk
> sdb           8:16   0    5G  0 disk
> sdc           8:32   0    5G  0 disk
> sdd           8:48   0    5G  0 disk
> sr0          11:0    1   11G  0 rom
> nvme0n1     259:0    0   30G  0 disk
> ├─nvme0n1p1 259:1    0    1G  0 part /boot
> └─nvme0n1p2 259:2    0   29G  0 part
>   ├─rhel-root 253:0    0   26G  0 lvm  /
>   └─rhel-swap 253:1    0    3G  0 lvm  [SWAP]
> [root@appsvr01 ~]#
> ```

### 2. إنشاء مصفوفة RAID 1

```bash
mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
```

> [!example]- outpout إنشاء RAID 1
> ```text
> [root@appsvr01 ~]# mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
> mdadm: Note: this array has metadata at the start and
>     may not be suitable as a boot device. If you plan to
>     store '/boot' on this device please ensure that
>     your boot-loader understands md/v1.x metadata, or use
>     --metadata=0.90
> mdadm: size set to 5237760K
> Continue creating array [y/N]? y
> mdadm: Defaulting to version 1.2 metadata
> mdadm: array /dev/md0 started.
> [root@appsvr01 ~]#
> ```

### 3. التحقق من الحالة

```bash
cat /proc/mdstat
mdadm --detail /dev/md0
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsvr01 ~]# cat /proc/mdstat
> Personalities : [raid1]
> md0 : active raid1 sdb[1] sda[0]
>       5237760 blocks super 1.2 [2/2] [UU]
>
> unused devices: <none>
> [root@appsvr01 ~]#
> ```

### 4. إنشاء نظام ملفات XFS

```bash
mkfs.xfs /dev/md0
```

> [!example]- outpout `mkfs.xfs`
> ```text
> [root@appsvr01 ~]# mkfs.xfs /dev/md0
> meta-data=/dev/md0               isize=512    agcount=4, agsize=327360 blks
>          =                       sectsz=512   attr=2, projid32bit=1
>          =                       crc=1        finobt=1, sparse=1, rmapbt=0
>          =                       reflink=1    bigtime=1 inobtcount=1
> data     =                       bsize=4096   blocks=1309440, imaxpct=25
>          =                       sunit=0      swidth=0 blks
> naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
> log      =internal log           bsize=4096   blocks=16384, version=2
>          =                       sectsz=512   sunit=0 blks, lazy-count=1
> realtime =none                   extsz=4096   blocks=0, rtextents=0
> [root@appsvr01 ~]#
> ```

### 5. التحميل

```bash
mkdir /mnt/raid1
mount /dev/md0 /mnt/raid1
```

> [!example]- outpout التحميل
> ```text
> [root@appsvr01 ~]# mkdir /mnt/raid1
> [root@appsvr01 ~]# mount /dev/md0 /mnt/raid1
> [root@appsvr01 ~]#
> ```

### 6. جعل التحميل دائمًا

```bash
blkid /dev/md0
```

> [!example]- outpout `blkid`
> ```text
> [root@appsvr01 ~]# blkid /dev/md0
> /dev/md0: UUID="ef71b008-6f88-4fbc-a63d-99adf07d15a8" TYPE="xfs"
> [root@appsvr01 ~]#
> ```

أضف إلى `/etc/fstab`:

```bash
UUID=ef71b008-6f88-4fbc-a63d-99adf07d15a8 /mnt/raid1 xfs defaults 0 0
```

ثم:

```bash
mount -a
df -h
```

> [!example]- outpout `df -h`
> ```text
> [root@appsvr01 ~]# mount -a
> [root@appsvr01 ~]# df -h
> Filesystem Size Used Avail Use% Mounted on
> devtmpfs 4.0M 0 4.0M 0% /dev
> tmpfs 1.8G 0 1.8G 0% /dev/shm
> tmpfs 726M 9.6M 716M 2% /run
> /dev/mapper/rhel-root 26G 5.0G 22G 19% /
> /dev/nvme0nlp1 960M 455M 506M 48% /boot
> tmpfs 363M 52K 363M 1% /run/user/42
> tmpfs 363M 36K 363M 1% /run/user/0
> /dev/md0 5.0G 68M 4.9G 2% /mnt/raid1
> [root@appsvr01 ~]#
> ```

### 7. التحقق بعد إعادة التشغيل

```bash
mdadm --detail /dev/md0
df -h
```

### 8. محاكاة فشل قرص والتعافي (RAID 1)

#### محاكاة فشل `/dev/sdb`

```bash
mdadm --fail /dev/md0 /dev/sdb
mdadm --detail /dev/md0
```

> [!example]- outpout محاكاة الفشل
> ```text
> [root@appsvr01 ~]# mdadm --fail /dev/md0 /dev/sdb
> mdadm: set /dev/sdb faulty in /dev/md0
> [root@appsvr01 ~]# mdadm --detail /dev/md0
> /dev/md0:
>     Version : 1.2
>     Creation Time : Sat Apr 26 10:17:45 2025
>         Raid Level : raid1
>         Array Size : 5237760 (5.00 GiB 5.36 GB)
>     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
>       Raid Devices : 2
>      Total Devices : 2
>     Persistence : Superblock is persistent
>        Update Time : Sat Apr 26 10:42:42 2025
>              State : Clean, degraded
>     Active Devices :
>    Working Devices : 1
>     Failed Devices : 1
>      Spare Devices : 0
> Consistency Policy : resync
>               Name : appsvr01.example.com:0 (local to host appsvr01.example.com)
>               UUID : 4fb6227f:29a0a617:0029c79e:36c52c55c
>             Events : 21
>     Number : Major Minor RaidDevice State
>         0 0 0 0 active sync /dev/sda
>         0 0 0 1 removed
>         1 8 16 - faulty /dev/sdb
> [root@appsvr01 ~]#
> ```

#### إزالة القرص الفاشل

```bash
mdadm --remove /dev/md0 /dev/sdb
mdadm --detail /dev/md0
```

> [!example]- outpout الإزالة
> ```text
> [root@appsvr01 ~]# mdadm --remove /dev/md0 /dev/sdb
> mdadm: hot removed /dev/sdb from /dev/md0
> [root@appsvr01 ~]#
>
> Number   Major   Minor   RaidDevice State
>     0      8      0      0   active sync   /dev/sda
>     -      0      0      1   removed
> [root@appsvr01 ~]#
> ```

#### إضافة قرص جديد (مثال: `/dev/sdc`)

```bash
wipefs -a /dev/sdc
mdadm --add /dev/md0 /dev/sdc
```

> [!example]- outpout إضافة القرص
> ```text
> [root@appsvr01 ~]# wipefs -a /dev/sdc
> [root@appsvr01 ~]#
> [root@appsvr01 ~]# mdadm --add /dev/md0 /dev/sdc
> mdadm: added /dev/sdc
> [root@appsvr01 ~]#
> ```

#### متابعة إعادة البناء

```bash
cat /proc/mdstat
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsvr01 ~]# cat /proc/mdstat
> Personalities : [raid1]
> md0 : active raid1 sdc[2] sda[0]
>       5237760 blocks super 1.2 [2/2] [UU]
>
> unused devices: <none>
> [root@appsvr01 ~]#
> ```

#### التحقق من الصحة

```bash
mdadm --detail /dev/md0
```

> [!example]- outpout التحقق النهائي
> ```text
> [root@appsvr01 ~]# mdadm --detail /dev/md0
> /dev/md0:
>     Version : 1.2
>     Creation Time : Sat Apr 26 10:17:45 2025
>         Raid Level : raid1
>         Array Size : 5237760 (5.00 GiB 5.36 GB)
>     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
>       Raid Devices : 2
>      Total Devices : 2
>     Persistence : Superblock is persistent
>        Update Time : Sat Apr 26 10:52:06 2025
>              State : clean
>     Active Devices : 2
>    Working Devices : 2
>     Failed Devices : 0
>      Spare Devices : 0
> Consistency Policy : resync
>               Name : appsv01.example.com:0 (local to host appsv01.example.com)
>               UUID : 4fb6227f:29a0a617:0029c79e:62c5255c
>             Events : 43
>     Number   Major   Minor   RaidDevice State
>         0      8      0      0   active sync   /dev/sda
>         2      8     32      1   active sync   /dev/sdc
> [root@appsvr01 ~]#
> ```

---

## RAID 5

> [!info] تعريف RAID 5
> يجمع RAID 5 بين التجزئة (Striping) والتكافؤ (Parity)، مما يوفر تحملًا لفشل قرص واحد مع أداء قراءة أفضل من RAID 1. يتطلب ثلاثة أقراص على الأقل.

### المتطلبات
- ثلاثة أقراص فارغة على الأقل (`/dev/sda`, `/dev/sdb`, `/dev/sdc`).

### 1. إنشاء مصفوفة RAID 5

```bash
mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
```

> [!example]- outpout إنشاء RAID 5
> ```text
> [root@appsvr01 ~]# mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
> mdadm: layout defaults to left-symmetric
> mdadm: layout defaults to left-symmetric
> mdadm: chunk size defaults to 512K
> mdadm: size set to 523776K
> mdadm: Defaulting to version 1.2 metadata
> mdadm: array /dev/md0 started.
> [root@appsvr01 ~]#
> ```

### 2. مراقبة التزامن

```bash
cat /proc/mdstat
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsvr01 ~]# cat /proc/mdstat
> Personalities : [raid6] [raid5] [raid4]
> md0 : active raid5 sdc[3] sdb[1] sda[0]
>       10475520 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]
>
> unused devices: <none>
> [root@appsvr01 ~]#
> ```

### 3. إنشاء نظام ملفات

```bash
mkfs.xfs /dev/md0
```

> [!example]- outpout `mkfs.xfs`
> ```text
> [root@appsvr01 ~]# mkfs.xfs /dev/md0
> log stripe unit (524288 bytes) is too large (maximum is 256KiB)
> log stripe unit adjusted to 32KiB
> meta-data=/dev/md0
>          = size=512 agcount=16, agsize=163712 blks
>          = sectsz=512 attr=2, projid32bit=1
>          = crc=1 finobt=1, sparse=1, rmapbt=0
>          = reflink=1 bigtime=1 inobtcount=1 nrext64=0
> data     = bsize=4096 blocks=2618880, imapct=25
>          = sunit=128 swidth=256 blks
> naming   =version 2 bsize=4096 ascii-ci=0, ftype=1
> log      =internal log bsize=4096 blocks=16384, version=2
> realtime =none extsz=4096 bsize=8 blks, lazy-count=1
> [root@appsvr01 ~]#
> ```

### 4. التحميل

```bash
mkdir /mnt/raid5
mount /dev/md0 /mnt/raid5
df -h
```

> [!example]- outpout `df -h`
> ```text
> [root@appsvr01 ~]# mkdir /mnt/raid5
> [root@appsvr01 ~]# mount /dev/md0 /mnt/raid5
> [root@appsvr01 ~]# df -h
> Filesystem Size Used Avail Use% Mounted on
> devtmpfs 4.0M 0 4.0M 0% /dev
> tmpfs 1.8G 0 1.8G 0% /dev/shm
> tmpfs 726M 9.6G 716M 2% /run
> /dev/mapper/rhel-root 26G 5.0G 22G 19% /
> /dev/nvme0n1p1 960M 455M 506M 48% /boot
> tmpfs 363M 52K 363M 1% /run/user/42
> tmpfs 363M 36K 363M 1% /run/user/0
> /dev/md0 10G 104M 9.9G 2% /mnt/raid5
> [root@appsvr01 ~]#
> ```

### 5. جعل التحميل دائمًا

```bash
blkid /dev/md0
```

> [!example]- outpout `blkid`
> ```text
> [root@appsvr01 ~]# blkid /dev/md0
> /dev/md0: UUID="413f0795-330f-40c3-9b4c-acc59c89ddbe" TYPE="xfs"
> [root@appsvr01 ~]#
> ```

أضف إلى `/etc/fstab`:

```bash
UUID=413f0795-330f-40c3-9b4c-acc59c89ddbe /mnt/raid5 xfs defaults 0 0
```

### 6. التحقق بعد إعادة التشغيل

```bash
mdadm -D /dev/md0
```

> [!example]- outpout `mdadm -D`
> ```text
> [root@appsvr01 ~]# mdadm -D /dev/md0
> /dev/md0:
>     Version : 1.2
>     Creation Time : Sat Apr 26 11:43:12 2025
>         Raid Level : raid5
>         Array Size : 10475520 (9.99 GiB 10.73 GB)
>     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
>       Raid Devices : 3
>      Total Devices : 3
>     Persistence : Superblock is persistent
>        Update Time : Sat Apr 26 11:48:06 2025
>              State : clean
>     Active Devices : 3
>    Working Devices : 3
>     Failed Devices : 0
>      Spare Devices : 0
>             Layout : left-symmetric
>         Chunk Size : 512K
> Consistency Policy : resync
>               Name : appsvr01.example.com:0 (local to host appsvr01.example.com)
>               UUID : 08248bb9:18299022:6c66a9f9:e8ce77cd
>             Events : 18
>     Number Major Minor RaidDevice State
>         0 8 0 0 active sync /dev/sda
>         1 8 16 1 active sync /dev/sdb
>         3 8 32 2 active sync /dev/sdc
> [root@appsvr01 ~]#
> ```

### 7. محاكاة الفشل والتعافي (RAID 5)

> [!tip] اتبع نفس خطوات RAID 1
> راجع قسم **محاكاة فشل قرص والتعافي (RAID 1)** أعلاه، وطبّق نفس الأوامر مع تغيير اسم الجهاز إذا لزم الأمر.

---

## RAID 6

> [!info] تعريف RAID 6
> يشبه RAID 5 لكنه يوفر **تكافؤًا مزدوجًا (Double Parity)**، مما يسمح بتحمل فشل قرصين. يتطلب أربعة أقراص على الأقل.

### المتطلبات
- أربعة أقراص فارغة على الأقل (`/dev/sda`, `/dev/sdb`, `/dev/sdc`, `/dev/sdd`).

### 1. إنشاء مصفوفة RAID 6

```bash
mdadm --create --verbose /dev/md0 --level=6 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd
```

> [!example]- outpout إنشاء RAID 6
> ```text
> [root@appsvr01 ~]# mdadm --create --verbose /dev/md0 --level=6 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd
> mdadm: layout defaults to left-symmetric
> mdadm: layout defaults to left-symmetric
> mdadm: chunk size defaults to 512K
> mdadm: size set to 5237760K
> mdadm: Defaulting to version 1.2 metadata
> mdadm: array /dev/md0 started.
> [root@appsvr01 ~]#
> ```

### 2. مراقبة التزامن

```bash
cat /proc/mdstat
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsvr01 ~]# cat /proc/mdstat
> Personalities : [raid6] [raid5] [raid4]
> md0 : active raid6 sdd[3] sdc[2] sdb[1] sda[0]
>       10475520 blocks super 1.2 level 6, 512k chunk, algorithm 2 [4/4] [UUUU]
>
> unused devices: <none>
> [root@appsvr01 ~]#
> ```

### 3. إنشاء نظام ملفات

```bash
mkfs.xfs /dev/md0
```

> [!example]- outpout `mkfs.xfs`
> ```text
> [root@appsvr01 ~]# mkfs.xfs /dev/md0
> log stripe unit (524288 bytes) is too large (maximum is 256KiB)
> log stripe unit adjusted to 32KiB
> meta-data=/dev/md0 isize=512 agcount=16, agsize=163712 blks
>          = sectsz=512 attr=2, projid32bit=1
>          = crc=1 finobt=1, sparse=1, rmapbt=0
>          = reflink=1 bigtime=1 inobtcount=1 nrext64=0
> data     = bsize=4096 blocks=2618880, imaxpct=25
>          = sunit=128 swidth=256 blks
> naming   =version 2 bsize=4096 ascii-ci=0, ftype=1
> log      =internal log bsize=4096 blocks=16384, version=2
>          = sectsz=512 sunit=8 blks, lazy-count=1
> realtime =none extsz=4096 blocks=0, rtextents=0
> [root@appsvr01 ~]#
> ```

### 4. التحميل

```bash
mkdir /mnt/raid6
mount /dev/md0 /mnt/raid6
```

### 5. التحقق

```bash
df -h
```

> [!example]- outpout `df -h`
> ```text
> [root@appsrv01 ~]# df -h
> Filesystem      Size  Used Avail Use% Mounted on
> devtmpfs        4.0M     0  4.0M   0% /dev
> tmpfs           1.8G     0  1.8G   0% /dev/shm
> tmpfs           726M   9.6G  716M   2% /run
> /dev/mapper/rhel-root  26G   5.0G   22G  19% /
> /dev/nvme0n1p1   960M   455M  506M  48% /boot
> tmpfs           363M   52K  363M   1% /run/user/42
> tmpfs           363M   36K  363M   1% /run/user/0
> /dev/md0         10G  104M  9.9G   2% /mnt/raid6
> [root@appsrv01 ~]#
> ```

### 6. جعل التحميل دائمًا

```bash
blkid /dev/md0
```

> [!example]- outpout `blkid`
> ```text
> [root@appsrv01 ~]# blkid /dev/md0
> /dev/md0: UUID="5b07cc05-9d0d-4d48-b089-1b1b9bc20d24" TYPE="xfs"
> [root@appsrv01 ~]#
> ```

أضف إلى `/etc/fstab`:

```bash
UUID=5b07cc05-9d0d-4d48-b089-1b1b9bc20d24 /mnt/raid6 xfs defaults 0 0
```

### 7. التحقق بعد إعادة التشغيل

```bash
mdadm -D /dev/md0
```

> [!example]- outpout `mdadm -D`
> ```text
> [root@appsrv01 ~]# mdadm -D /dev/md0
> /dev/md0:
>     Version : 1.2
>     Creation Time : Sat Apr 26 12:32:34 2025
>         Raid Level : raid6
>         Array Size : 10475520 (9.99 GiB 10.73 GB)
>     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
>       Raid Devices : 4
>      Total Devices : 4
>     Persistence : Superblock is persistent
>        Update Time : Sat Apr 26 12:39:02 2025
>              State : clean
>     Active Devices : 4
>    Working Devices : 4
>     Failed Devices : 0
>      Spare Devices : 0
>             Layout : left-symmetric
>         Chunk Size : 512K
> Consistency Policy : resync
>               Name : appsrv01.example.com:0 (local to host appsrv01.example.com)
>               UUID : 7f841d68:b17eed4d:6131a5cd:cb900e2f
>             Events : 17
>     Number Major Minor RaidDevice State
>         0 8 0 1 active sync /dev/sda
>         1 8 16 1 active sync /dev/sdb
>         2 8 32 2 active sync /dev/sdc
>         3 8 48 3 active sync /dev/sdd
> [root@appsrv01 ~]#
> ```

### 8. محاكاة الفشل والتعافي (RAID 6)

> [!tip] اتبع نفس خطوات RAID 1
> راجع قسم **محاكاة فشل قرص والتعافي (RAID 1)** وطبّق نفس الخطوات. يمكن لـ RAID 6 تحمل فشل قرصين في نفس الوقت.

---

## RAID 10 (1+0)

> [!info] تعريف RAID 10
> يجمع بين النسخ المتطابق (Mirroring) والتجزئة (Striping)، مما يوفر أداءً عاليًا وتكرارًا. يتطلب أربعة أقراص على الأقل.

### المتطلبات
- أربعة أقراص فارغة على الأقل (`/dev/sda`, `/dev/sdb`, `/dev/sdc`, `/dev/sdd`).

### 1. إنشاء مصفوفة RAID 10

```bash
mdadm --create --verbose /dev/md0 --level=10 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd
```

> [!example]- outpout إنشاء RAID 10
> ```text
> [root@appsvr01 ~]# mdadm --create --verbose /dev/md0 --level=10 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd
> mdadm: layout defaults to n2
> mdadm: layout defaults to n2
> mdadm: chunk size defaults to 512K
> mdadm: size set to 5237760K
> mdadm: Defaulting to version 1.2 metadata
> mdadm: array /dev/md0 started.
> [root@appsvr01 ~]#
> ```

### 2. مراقبة التزامن

```bash
cat /proc/mdstat
```

> [!example]- outpout `cat /proc/mdstat`
> ```text
> [root@appsvr01 ~]# cat /proc/mdstat
> Personalities : [raid10]
> md0 : active raid10 sdd[3] sdc[2] sdb[1] sda[0]
>       10475520 blocks super 1.2 512K chunks 2 near-copies [4/4] [UUUU]
>
> unused devices: <none>
> [root@appsvr01 ~]#
> ```

### 3. إنشاء نظام ملفات

```bash
mkfs.xfs /dev/md0
```

> [!example]- outpout `mkfs.xfs`
> ```text
> [root@appsvr01 ~]# mkfs.xfs /dev/md0
> log stripe unit (524288 bytes) is too large (maximum is 256KiB)
> log stripe unit adjusted to 32KiB
> meta-data=/dev/md0 isize=512 agcount=16, agsize=163712 blks
>          = sectsz=512 attr=2, projid32bit=1
>          = crc=1 finobt=1, sparse=1, rmapbt=0
>          = reflink=1 bigtime=1 inobtcount=1 nrext64=0
> data     = bsize=4096 blocks=2618880, imaxpct=25
>          = sunit=128 swidth=256 blks
> naming   =version 2 bsize=4096 ascii-ci=0, ftype=1
> log      =internal log bsize=4096 blocks=16384, version=2
>          = sectsz=512 sunit=8 blks, lazy-count=1
> realtime =none extsz=4096 blocks=0, rtextents=0
> [root@appsvr01 ~]#
> ```

### 4. التحميل

```bash
mkdir /mnt/raid10
mount /dev/md0 /mnt/raid10
df -h
```

> [!example]- outpout `df -h`
> ```text
> [root@appsvr01 ~]# mkdir /mnt/raid10
> [root@appsvr01 ~]# mount /dev/md0 /mnt/raid10
> [root@appsvr01 ~]# df -h
> Filesystem Size Used Avail Use% Mounted on
> devtmpfs 4.0M 0 4.0M 0% /dev
> tmpfs 1.8G 0 1.8G 0% /dev/shm
> tmpfs 726M 9.6M 716M 2% /run
> /dev/mapper/rhel-root 26G 5.0G 22G 19% /
> /dev/nvme0n1p1 960M 455M 506M 48% /boot
> tmpfs 363M 52K 363M 1% /run/user/42
> tmpfs 363M 36K 363M 1% /run/user/0
> /dev/md0 10G 104M 9.9G 2% /mnt/raid10
> [root@appsvr01 ~]#
> ```

### 5. جعل التحميل دائمًا

```bash
blkid /dev/md0
```

> [!example]- outpout `blkid`
> ```text
> [root@appsvr01 ~]# blkid /dev/md0
> /dev/md0: UUID="3e78f31c-cdfa-4ab6-b9ac-f33390142857" TYPE="xfs"
> [root@appsvr01 ~]#
> ```

أضف إلى `/etc/fstab`:

```bash
UUID=3e78f31c-cdfa-4ab6-b9ac-f33390142857 /mnt/raid10 xfs defaults 0 0
```

ثم:

```bash
mount -a
```

### 6. التحقق بعد إعادة التشغيل

```bash
mdadm --detail /dev/md0
```

> [!example]- outpout `mdadm --detail`
> ```text
> [root@appsvr01 ~]# mdadm --detail /dev/md0
> /dev/md0:
>     Version : 1.2
>     Creation Time : Sat Apr 26 18:57:55 2025
>         Raid Level : raid10
>         Array Size : 10475520 (9.99 GiB 10.73 GB)
>     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
>       Raid Devices : 4
>      Total Devices : 4
>     Persistence : Superblock is persistent
>
>        Update Time : Sat Apr 26 19:04:26 2025
>              State : clean
>     Active Devices : 4
>    Working Devices : 4
>     Failed Devices : 0
>      Spare Devices : 0
>
>             Layout : near=2
>         Chunk Size : 512K
>
> Consistency Policy : resync
>
>               Name : appsvr01.example.com:0 (local to host appsvr01.example.com)
>               UUID : cf6335ee:ea884e92:6d218880:fc7a1104
>             Events : 17
>
>     Number Major Minor RaidDevice State
>         0 8 0 0 active sync set-A /dev/sda
>         1 8 16 1 active sync set-B /dev/sdb
>         2 8 32 2 active sync set-A /dev/sdc
>         3 8 48 3 active sync set-B /dev/sdd
> [root@appsvr01 ~]#
> ```

### 7. محاكاة الفشل والتعافي (RAID 10)

> [!tip] اتبع نفس خطوات RAID 1
> راجع قسم **محاكاة فشل قرص والتعافي (RAID 1)**. يمكن لـ RAID 10 تحمل فشل قرص واحد على الأقل في كل مجموعة نسخ متطابق.

---

## 🔒 حفظ إعدادات المصفوفة (mdadm.conf)

> [!important] خطوة مكملة للـ fstab
> `fstab` يحفظ الـ mount — لكن `mdadm.conf` يحفظ ==تعريف المصفوفة نفسها==.

```bash
mdadm --detail --scan >> /etc/mdadm.conf
cat /etc/mdadm.conf
```

> [!example]- Output
> ```text
> ARRAY /dev/md0 metadata=1.2 name=appsv01.example.com:0 UUID=88f908e7:78684f17:c66acb5f:15f95023
> ```

> [!warning] `>>` يضيف ولا يستبدل
> تنفيذ الأمر مرتين = سطر مكرر. نفذه ==مرة واحدة لكل مصفوفة== وراجع الملف بعدها.

> [!note] هل هو إلزامي في RHEL 9؟
> المصفوفات تُجمَّع تلقائياً عبر udev حتى بدونه — لكنه يضمن ==اسماً ثابتاً==
> (`/dev/md0` بدل مفاجأة `/dev/md127`) وسلوكاً متوقعاً بعد الـ reboot.

اختبار التجميع:
```bash
mdadm --assemble --scan
```
## ملخص سريع

| مستوى RAID | الحد الأدنى للأقراص | التكرار | ملاحظات |
| :--- | :--- | :--- | :--- |
| RAID 0 | 2 | لا يوجد | أداء عالٍ، لا يوجد تحمل فشل |
| RAID 1 | 2 | قرص واحد | نسخ متطابق كامل |
| RAID 5 | 3 | قرص واحد | توازن بين الأداء والتكرار |
| RAID 6 | 4 | قرصان | تكرار مزدوج |
| RAID 10 | 4 | قرص واحد لكل مجموعة | أداء عالٍ + تكرار |

---

## جدول مقارنة RAID المطوّر (مع المساحة الفعلية والهدر)

| مستوى RAID | الحد الأدنى للأقراص | التكرار | المساحة الكلية | **المساحة الفعلية (Usable)** | **المفقودة (Parity/Mirror)** | **نسبة الهدر** | **نسبة الاستفادة** | ملاحظات |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **RAID 0** | 2 | لا يوجد | 10 GB (2×5) | **10 GB** | 0 GB | **0%** | **100%** | أداء عالٍ، لا يوجد تحمل فشل |
| **RAID 1** | 2 | قرص واحد | 10 GB (2×5) | **5 GB** | 5 GB | **50%** | **50%** | نسخ متطابق كامل |
| **RAID 5** | 3 | قرص واحد | 15 GB (3×5) | **10 GB** | 5 GB | **33.3%** | **66.7%** | توازن بين الأداء والتكرار |
| **RAID 6** | 4 | قرصان | 20 GB (4×5) | **10 GB** | 10 GB | **50%** | **50%** | تكرار مزدوج |
| **RAID 10** | 4 | قرص واحد لكل مجموعة | 20 GB (4×5) | **10 GB** | 10 GB | **50%** | **50%** | أداء عالٍ + تكرار |

---

## الصيغ العامة لحساب المساحة الفعلية

> [!info] معادلات سريعة
> حيث **N** = عدد الأقراص، و **S** = حجم القرص الواحد.
>
> | المستوى | الصيغة | مثال (5GB × N) |
> | :--- | :--- | :--- |
> | RAID 0 | `N × S` | 2×5 = **10 GB** |
> | RAID 1 | `(N / 2) × S` | (2/2)×5 = **5 GB** |
> | RAID 5 | `(N − 1) × S` | (3−1)×5 = **10 GB** |
> | RAID 6 | `(N − 2) × S` | (4−2)×5 = **10 GB** |
> | RAID 10 | `(N / 2) × S` | (4/2)×5 = **10 GB** |

---

## شرح مبسط لهدر المساحة

> [!warning] لماذا تُفقد المساحة؟
> - **RAID 1 / RAID 10**: تُفقد المساحة لأن كل قرص له **نسخة مطابقة (Mirror)**. النصف يُستخدم للنسخ الاحتياطي.
> - **RAID 5**: يُخصص حجم **قرص واحد** لتخزين التكافؤ (Parity) موزعًا على الأقراص. لذلك مساحة قرص واحد "تُفقد" من إجمالي السعة.
> - **RAID 6**: يُخصص حجم **قرصين** للتكافؤ المزدوج. لذلك تُفقد مساحة قرصين من إجمالي السعة.
> - **RAID 0**: لا يوجد تكرار ولا تكافؤ → لا هدر، لكن **لا تحمل فشل** إطلاقًا.

---

## تحقق من الأمثلة في الدليل

> [!example]- مطابقة المخرجات الفعلية من الدليل
> | المستوى | الأقراص | الحجم النظري | `df -h` من الدليل | ملاحظة |
> | :--- | :--- | :--- | :--- | :--- |
> | RAID 0 | 2×5GB | 10 GB | `/dev/md0 9.8G` | ✅ مطابق (فرق بسيط بسبب حساب GB vs GiB) |
> | RAID 1 | 2×5GB | 5 GB | `/dev/md0 5.0G` | ✅ مطابق |
> | RAID 5 | 3×5GB | 10 GB | `/dev/md0 10G` | ✅ مطابق |
> | RAID 6 | 4×5GB | 10 GB | `/dev/md0 10G` | ✅ مطابق |
> | RAID 10 | 4×5GB | 10 GB | `/dev/md0 10G` | ✅ مطابق |

> [!note] ملاحظة حول الفرق بين GB و GiB
> الأدوات مثل `df -h` تعرض الحجم بوحدة **GiB** (1024³ بايت)، بينما الشركات المصنعة للأقراص تستخدم **GB** (1000³ بايت). لذلك قد ترى `9.8G` بدلًا من `10G` في بعض الحالات.

---

## استنتاج سريع للمساحات بناءً على عدد الأقراص

> [!tip] قاعدة سريعة
> إذا كان لديك **4 أقراص بحجم 5GB** (إجمالي 20GB)، فالمساحة الفعلية ستكون:
> - RAID 0 (4 أقراص): **20 GB** — أقصى استفادة، لا أمان.
> - RAID 1 (4 أقراص): **10 GB** — أمان كامل، هدر 50%.
> - RAID 5 (3 أقراص فقط): **10 GB** — أمان قرص واحد، استفادة 66.7%.
> - RAID 6 (4 أقراص): **10 GB** — أمان قرصين، استفادة 50%.
> - RAID 10 (4 أقراص): **10 GB** — أداء عالٍ + أمان، استفادة 50%.

---

يمكنك الآن استبدال الجدول القديم بهذا الجدول المطوّر في الملف. إذا أردت، أستطيع دمج هذا الجدول مباشرة داخل ملف `RAID.md` الكامل وإعادة تسليمه لك جاهزًا للنسخ.