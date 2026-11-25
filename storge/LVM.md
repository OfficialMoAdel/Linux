---
title: دليل إعداد LVM (Logical Volume Manager) في RHEL
tags:
  - linux
  - lvm
  - storage
  - rhel
  - sysadmin
aliases:
  - LVM Guide
  - دليل LVM
reference: https://www.server-world.info/en/note?os=CentOS_Stream_9&p=lvm&f=1
---

# دليل إعداد LVM (Logical Volume Manager)

> [!info] نظرة عامة
> LVM اختصار لـ Logical Volume Manager، وهو طريقة لإدارة التخزين (الأقراص الصلبة، SSDs، الأقسام) بمرونة أكبر من طرق التقسيم التقليدية. بدلًا من تقطيع القرص إلى أقسام ثابتة (مثل `/dev/sda`، `/dev/sdb`)، يتيح لك LVM إنشاء مساحات تخزين "افتراضية" يمكن أن تنمو وتتقلص وتنتقل دون القلق من حدود القرص الفيزيائي.


```mermaid
flowchart LR
    A["Physical Disks - sda, sdb"] --> B["PV - pvcreate"]
    B --> C["VG - vgcreate"]
    C --> D["LV - lvcreate"]
    D --> E["Filesystem - mkfs.xfs"]
    E --> F["Mount - /data"]
```


> [!tip] افتراض
> يفترض الدليل أن `/dev/sda` هو القرص الحر المتاح.

---

## الإعداد العملي الكامل لـ LVM (بدءًا من إنشاء قسم)

### الخطوة 1: التحقق من الأقراص المتاحة

```bash
lsblk
```

> [!example]- outpout الأمر `lsblk`
> ```text
> [root@appsvr01 ~]# lsblk
> NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
> sda           8:0    0    5G  0 disk
> sdb           8:16   0    5G  0 disk
> sdc           8:32   0    5G  0 disk
> sdd           8:48   0    5G  0 disk
> sr0          11:0    1   11G  0 rom
> nvme0nl     259:0    0   30G  0 disk
> ├─nvme0nlp1 259:1    0    1G  0 part /boot
> └─nvme0nlp2 259:2    0   29G  0 part
>   ├─rhel-root 253:0    0   26G  0 lvm  /
>   └─rhel-swap 253:1    0    3G  0 lvm  [SWAP]
> [root@appsvr01 ~]#
> ```

---

### الخطوة 2: إنشاء قسم جديد (Partition)

> [!tip] لفهم fdisk و MBR/GPT بعمق → [[Partitioning]]

استخدم `fdisk` أو `parted` لتقسيم القرص. مثال باستخدام `fdisk`:

```bash
fdisk /dev/sda
```

> [!note] خطوات العمل داخل fdisk
> 1. اضغط `n` → (قسم جديد).
> 2. اختر `p` → (primary).
> 3. اختر رقم القسم (الافتراضي: 1).
> 4. اقبل القطاعات الافتراضية الأولى والأخيرة (أو حدد حجمًا مخصصًا).
> 5. اضغط `t` → لتغيير نوع القسم.
> 6. اكتب `8e` → (8e = Linux LVM).
> 7. اضغط `w` → (الكتابة والخروج).
> 8. ستحصل الآن على `/dev/sda1` كقسم جاهز لـ LVM.

> [!example]- outpout جلسة `fdisk` الكاملة
> ```text
> [root@appsvr01 ~]# fdisk /dev/sda
> Welcome to fdisk (util-linux 2.37.4).
> Changes will remain in memory only, until you decide to write them.
> Be careful before using the write command.
>
> Device does not contain a recognized partition table.
> Created a new DOS disklabel with disk identifier 0x9379158e.
>
> Command (m for help): n
> Partition type:
>     p   primary (0 primary, 0 extended, 4 free)
>     e   extended (container for logical partitions)
> Select (default p): P
> Partition number (1-4, default 1): 1
> First sector (2048-10485759, default 2048):
> Last sector, +/-sectors or +/-size(K,M,G,T,P) (2048-10485759, default 10485759):
> Created a new partition 1 of type 'Linux' and of size 5 GiB.
>
> Command (m for help): p
> Disk /dev/sda: 5 GiB, 5368709120 bytes, 10485760 sectors
> Disk model: VMware Virtual S
> Units: sectors of 1 * 512 = 512 bytes
> Sector size (logical/physical): 512 bytes / 512 bytes
> I/O size (minimum/optimal): 512 bytes / 512 bytes
> Disk label type: dos
> Disk identifier: 0x9379158e
>
> Device     Boot Start      End  Sectors Size Id Type
> /dev/sda1        2048 10485759 10483712   5G 83 Linux
>
> Command (m for help): t
> Selected partition 1
> Hex code or alias (type L to list all): 8e
> Changed type of partition 'Linux' to 'Linux LVM'.
>
> Command (m for help): p
> Disk /dev/sda: 5 GiB, 5368709120 bytes, 10485760 sectors
> Disk model: VMware Virtual S
> Units: sectors of 1 * 512 = 512 bytes
> Sector size (logical/physical): 512 bytes / 512 bytes
> I/O size (minimum/optimal): 512 bytes / 512 bytes
> Disk label type: dos
> Disk identifier: 0x9379158e
>
> Device     Boot Start      End  Sectors Size Id Type
> /dev/sda1        2048 10485759 10483712   5G 8e Linux LVM
>
> Command (m for help): w
> The partition table has been altered.
> Calling ioctl() to re-read partition table.
> Syncing disks.
>
> [root@appsvr01 ~]#
> ```

بعد الخروج من `fdisk`، أعد قراءة جدول الأقسام:

```bash
partprobe
```

---

### الخطوة 3: إنشاء Physical Volume (PV)

```bash
pvcreate /dev/sda1
pvs
```

> [!example]- outpout `pvcreate` و `pvs`
> ```text
> [root@appsvr01 ~]# pvcreate /dev/sda1
> Physical volume "/dev/sda1" successfully created.
> [root@appsvr01 ~]# pvs
> PV             VG  Fmt  Attr PSize   PFree
> /dev/nvme0n1p2 rhel lvm2 a--  <29.00g    0
> /dev/sda1          lvm2 ---   <5.00g <5.00g
> [root@appsvr01 ~]#
> ```

---

### الخطوة 4: إنشاء Volume Group (VG)

```bash
vgcreate vg_data /dev/sda1
vgs
```

> [!example]- outpout `vgcreate` و `vgs`
> ```text
> [root@appsvr01 ~]# vgcreate vg_data /dev/sda1
> Volume group "vg_data" successfully created
> [root@appsvr01 ~]# vgs
> VG      #PV #LV #SN Attr   VSize   VFree
> rhel      1   2   0 wz--n- <29.00g    0
> vg_data   1   0   0 wz--n-  <5.00g <5.00g
> [root@appsvr01 ~]#
> ```

---

### الخطوة 5: إنشاء Logical Volume (LV)

```bash
lvcreate -l +100%FREE -n lv_data vg_data
```

> [!note] خيارات إنشاء LV
> - `-l +100%FREE`: استخدام كل المساحة المتاحة.
> - `-L 4GB`: تحديد حجم ثابت (استخدم حسب حاجتك).

---

### الخطوة 6: إنشاء نظام ملفات على الـ Logical Volume

```bash
mkfs.xfs /dev/vg_data/lv_data
```

> [!example]- outpout `mkfs.xfs`
> ```text
> [root@appsvr01 ~]# mkfs.xfs /dev/vg_data/lv_data
> meta-data=/dev/vg_data/lv_data  isize=512    agcount=4, agsize=327424 blks
>          =                     sectsz=512   attr=2, projid32bit=1
>          =                     crc=1        finobt=1, sparse=1, rmapbt=0
>          =                     reflink=1    bigtime=1 inobtcount=1 nrext64=0
> data     =                     bsize=4096   blocks=1309696, imaxpct=25
>          =                     sunit=0      swidth=0 blks
> naming   =version 2            bsize=4096   ascii-ci=0, ftype=1
> log      =internal log         bsize=4096   blocks=16384, version=2
>          =                     sectsz=512   sunit=0 blks, lazy-count=1
> realtime =none                 extsz=4096   blocks=0, rtextents=0
> ```

---

### الخطوة 7: إنشاء نقطة تحميل وتحميل الـ Logical Volume

```bash
mkdir /data
mount /dev/vg_data/lv_data /data
df -h
```

> [!example]- outpout التحميل و `df -h`
> ```text
> [root@appsvr01 ~]# mkdir /data
> [root@appsvr01 ~]# mount /dev/vg_data/lv_data /data
> [root@appsvr01 ~]# df -h
> Filesystem                  Size  Used Avail Use% Mounted on
> devtmpfs                    4.0M     0  4.0M   0% /dev
> tmpfs                       1.8G     0  1.8G   0% /dev/shm
> tmpfs                       726M  9.6M  716M   2% /run
> /dev/mapper/rhel-root        26G  5.0G   22G  19% /
> /dev/nvme0n1p1              960M  455M  506M  48% /boot
> tmpfs                       363M   52K  363M   1% /run/user/42
> tmpfs                       363M   36K  363M   1% /run/user/0
> /dev/mapper/vg_data-lv_data  5.0G   68M  4.9G   2% /data
> [root@appsvr01 ~]#
> ```

---

### الخطوة 8: جعل التحميل دائمًا (Persistent)

```bash
blkid /dev/vg_data/lv_data
```

> [!example]- outpout `blkid`
> ```text
> [root@appsvr01 ~]# blkid /dev/vg_data/lv_data
> /dev/vg_data/lv_data: UUID="852cb752-e09d-40a9-9db5-ab7f12662226" TYPE="xfs"
> [root@appsvr01 ~]#
> ```

أضف السطر التالي إلى `/etc/fstab`:

```bash
UUID=852cb752-e09d-40a9-9db5-ab7f12662226 /data xfs defaults 0 0
```

> [!warning] تحذير مهم
> اختبر تغييرات `fstab` دائمًا باستخدام `mount -a` قبل إعادة التشغيل، لتجنب فشل الإقلاع.

```bash
vim /etc/fstab
mount -a
tail -2 /etc/fstab
```

> [!example]- outpout التحقق من `/etc/fstab`
> ```text
> [root@appsvr01 ~]# vim /etc/fstab
> [root@appsvr01 ~]# mount -a
> [root@appsvr01 ~]# tail -2 /etc/fstab
> /dev/mapper/rhel-swap none swap defaults 0 0
> UUID=852cb752-e09d-40a9-9db5-ab7f12662226 /data xfs defaults 0 0
> [root@appsvr01 ~]#
> ```

> [!success] تم!
> تحقق من صحة الإعداد قبل إعادة التشغيل.

---

## توسيع `/data` بإضافة `/dev/sdb` دون توقف (Zero Downtime)

### 1. تحديد مسار LV الحالي

```bash
lsblk
```

> [!example]- outpout `lsblk`
> ```text
> [root@appsv01 ~]# lsblk
> NAME               MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
> sda                  8:0    0    5G  0 disk
> └─sda1               8:1    0    5G  0 part
>   └─vg_data-lv_data 253:2    0    5G  0 lvm  /data
> sdb                 8:16   0    5G  0 disk
> sdc                 8:32   0    5G  0 disk
> sdd                 8:48   0    5G  0 disk
> sr0                 11:0   1   11G  0 rom
> nvme0n1            259:0    0   30G  0 disk
> ├─nvme0n1p1        259:1    0    1G  0 part /boot
> └─nvme0n1p2        259:2    0   29G  0 part
>   ├─rhel-root      253:0    0   26G  0 lvm  /
>   └─rhel-swap      253:1    0    3G  0 lvm  [SWAP]
> [root@appsv01 ~]#
> ```

---

### 2. تهيئة القرص الجديد كـ Physical Volume

```bash
pvcreate /dev/sdb
pvs
```

> [!example]- outpout `pvcreate` و `pvs`
> ```text
> [root@appsv01 ~]# pvcreate /dev/sdb
> Physical volume "/dev/sdb" successfully created.
> [root@appsv01 ~]# pvs
> PV             VG      Fmt  Attr PSize   PFree
> /dev/nvme0nlp2 rhel    lvm2 a--  <29.00g    0
> /dev/sda1      vg_data lvm2 a--   <5.00g    0
> /dev/sdb               lvm2 ---    5.00g 5.00g
> [root@appsv01 ~]#
> ```

---

### 3. توسيع الـ Volume Group

```bash
vgextend vg_data /dev/sdb
vgs
```

> [!example]- outpout `vgextend` و `vgs`
> ```text
> [root@appsv01 ~]# vgextend vg_data /dev/sdb
> Volume group "vg_data" successfully extended
> [root@appsvr01 ~]# vgs
> VG      #PV #LV #SN Attr   VSize   VFree
> rhel      1   2   0 wz--n- <29.00g    0
> vg_data   2   1   0 wz--n-   9.99g <5.00g
> [root@appsvr01 ~]#
> ```

---

### 4. توسيع الـ Logical Volume

```bash
lvextend -l +100%FREE /dev/vg_data/lv_data
lvs
```

> [!example]- outpout `lvextend` و `lvs`
> ```text
> [root@appsvr01 ~]# lvextend -l +100%FREE /dev/vg_data/lv_data
> Size of logical volume vg_data/lv_data changed from <5.00 GiB (1279 extents) to 9.99 GiB (2558 extents).
> Logical volume vg_data/lv_data successfully resized.
> [root@appsvr01 ~]# lvs
> LV      VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
> root    rhel    -wi-ao---- <26.00g
> swap    rhel    -wi-ao----   3.00g
> lv_data vg_data -wi-ao----   9.99g
> [root@appsvr01 ~]#
> ```

---

### 5. توسيع نظام الملفات (XFS)

```bash
xfs_growfs /data
```

> [!example]- outpout `xfs_growfs`
> ```text
> [root@appsvr01 ~]# xfs_growfs /data
> meta-data=/dev/mapper/vg_data-lv_data isize=512    agcount=4, agsize=327424 blks
>          =                          sectsz=512   attr=2, projid32bit=1
>          =                          crc=1        finobt=1, sparse=1, rmapbt=0
>          =                          reflink=1    bigtime=1 inobtcount=1 nrext64=0
> data     =                          bsize=4096   blocks=1309696, imaxpct=25
>          =                          sunit=0      swidth=0 blks
> naming   =version 2                 bsize=4096   ascii-ci=0, ftype=1
> log      =internal log              bsize=4096   blocks=16384, version=2
>          =                          sectsz=512   sunit=0 blks, lazy-count=1
> realtime =none                      extsz=4096   blocks=0, rtextents=0
> data blocks changed from 1309696 to 2619392
> [root@appsvr01 ~]#
> ```

---

## ملخص سريع لأوامر LVM

| الأمر | الوظيفة |
| :--- | :--- |
| `lsblk` | عرض الأقراص والأقسام و LVM |
| `fdisk /dev/sdX` | إنشاء قسم جديد (نوع 8e = Linux LVM) |
| `partprobe` | إعادة قراءة جدول الأقسام |
| `pvcreate /dev/sdX1` | تهيئة قسم كـ Physical Volume |
| `pvs` | عرض قائمة PVs |
| `vgcreate <vg_name> <pv>` | إنشاء Volume Group |
| `vgs` | عرض قائمة VGs |
| `lvcreate -l +100%FREE -n <lv_name> <vg_name>` | إنشاء Logical Volume |
| `lvs` | عرض قائمة LVs |
| `mkfs.xfs /dev/<vg>/<lv>` | إنشاء نظام ملفات XFS |
| `mount /dev/<vg>/<lv> /mountpoint` | تحميل LV |
| `blkid /dev/<vg>/<lv>` | الحصول على UUID |
| `vgextend <vg> <pv>` | توسيع VG بإضافة PV جديد |
| `lvextend -l +100%FREE /dev/<vg>/<lv>` | توسيع LV |
| `xfs_growfs /mountpoint` | توسيع نظام ملفات XFS أونلاين |

