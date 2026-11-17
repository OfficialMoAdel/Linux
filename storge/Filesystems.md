---
title: "Filesystems — Format & Resize (XFS, ext4, vfat)"
created: 2026-11-14
updated: 2026-11-14
tags:
  - linux
  - storage
  - rhel
  - sysadmin
  - filesystems
  - rhcsa
aliases:
  - Filesystems
  - mkfs
  - XFS
  - ext4
  - Format
category: Storage
difficulty: beginner
links:
  - "[[Storage]]"
  - "[[Partitioning]]"
  - "[[LVM]]"
  - "[[RAID]]"
  - "[[fstab]]"
status: complete
---

# 🧬 Filesystems — Format & Resize

> [!abstract] الخلاصة في سطرين
> Format = كتابة ==نظام الملفات== على القسم/LV/المصفوفة عشان يقدر يخزن ملفات.
> الافتراضي في RHEL 9 ==XFS== — يتوسع أونلاين، ==لكن لا يتقلص أبداً==.

## 📑 Table of Contents
- [[#🎯 فين أنا في الـ Stack؟]]
- [[#📊 مقارنة أنواع Filesystems]]
- [[#🛠️ Step 1 — Format (mkfs)]]
- [[#📐 Step 2 — Resize: المصفوفة الثلاثية]]
- [[#⬆️ Step 3 — التوسيع (Online)]]
- [[#⬇️ Step 4 — التقليص (Offline — ext4 فقط)]]
- [[#🚨 Troubleshooting]]
- [[#📋 Commands Cheat Sheet]]
- [[#❓ Interview Questions]]
- [[#🏆 Golden Rules]]

## 🎯 فين أنا في الـ Stack؟

```text
Partition ([[Partitioning]]) → [LVM اختياري ([[LVM]]) أو RAID ([[RAID]])]
    → 🧬 أنت هنا: mkfs → mount ([[fstab]]) → resize لاحقاً
```

> [!info] الطبقات اختيارية
> في [[RAID]] عملت `mkfs` مباشرة على `/dev/md0` — وفي [[LVM]] على `/dev/vg_data/lv_data`.
> نظام الملفات لا يهتم بما تحته — قسم عادي أو LV أو مصفوفة.

## 📊 مقارنة أنواع Filesystems

| النوع | Journal | أقصى حجم ملف | Resize | الاستخدام |
| --- | --- | --- | --- | --- |
| ext2 | ❌ | 2 TiB | توسيع + تقليص | قديم — flash بدون journal |
| ext3 | ✅ | 2 TiB | توسيع + تقليص | legacy (أول ext بـ journal) |
| ext4 | ✅ | 16 TiB | توسيع أونلاين + تقليص offline | بديل عام ممتاز |
| ==XFS== | ✅ | 8 EiB | ==توسيع فقط== | ==افتراضي RHEL 9== |
| vfat (FAT32) | ❌ | ==4 GiB== | محدود | ==EFI (/boot/efi)== + USB مشترك مع Windows |
| swap | — | — | — | مساحة تبديل الذاكرة (`mkswap`) |

> [!info] ملاحظات RHEL 9
> - ext2/ext3 ==deprecated== — مشغل ext4 هو الذي يتعامل معها
> - btrfs ==غير مدعوم== في kernel RHEL 9
> - ntfs/exfat تحتاج حزم إضافية — vfat يكفي للـ EFI

## 🛠️ Step 1 — Format (mkfs)

```bash
mkfs.xfs /dev/vg_data/lv_data     # الافتراضي في RHEL 9
mkfs.ext4 /dev/sdb1               # البديل العام
mkfs.ext3 /dev/sdb1               # legacy
mkfs.ext2 /dev/sdb1               # legacy بدون journal
mkfs.vfat -F 32 /dev/sdb1         # FAT32 — EFI / USB
mkswap /dev/vg_data/lv_swap       # swap (ليست ملفات!)
```

> [!example]- Output `mkfs.ext4` (قرص 5G)
> ```text
> mke2fs 1.46.5 (30-Dec-2021)
> Creating filesystem with 1310720 4k blocks and 327680 inodes
> Filesystem UUID: 7c9e2f1a-4b3d-4c8a-9e21-6f0d5a8b3c77
> Allocating group tables: done
> Writing inode tables: done
> Creating journal (16384 blocks): done
> Writing superblocks and filesystem accounting information: done
> ```

> [!danger] mkfs يمحو كل شيء
> إعادة Format = فقدان البيانات. امسح التوقيع القديم أولاً: `wipefs -a`
> (أو `mkfs.xfs -f` للإجبار — بحذر!)

```bash
blkid /dev/sdb1        # خذ UUID → fstab
swapon /dev/vg_data/lv_swap && swapon --show   # تفعيل swap
```

## 📐 Step 2 — Resize: المصفوفة الثلاثية

> [!important] القاعدة الجوهرية
> التكبير/التصغير لازم يتم على ==كل طبقة== بالترتيب — تكبير القسم وحده لا يكبّر نظام الملفات!

| الطبقة | الأداة | أونلاين؟ | ملاحظة |
| --- | --- | --- | --- |
| Partition | `parted resizepart N END` | يعدّل الجدول فقط | ==لا يمس الـ FS== — أكمل للطبقة الأعلى |
| LVM | `lvextend` / `lvreduce` | التوسيع ✅ | التقليص لـ ext4 فقط وبخطوات |
| FS — XFS | `xfs_growfs MOUNTPOINT` | ✅ | ==توسيع فقط== |
| FS — ext* | `resize2fs DEVICE [SIZE]` | توسيع ✅ / تقليص ❌ | التقليص يحتاج unmount + e2fsck |

## ⬆️ Step 3 — التوسيع (Online)

الخطوات الكاملة بالـ outputs موجودة في [[LVM]] (lab التوسيع بدون توقف):

```bash
pvcreate /dev/sdb
vgextend vg_data /dev/sdb
lvextend -l +100%FREE /dev/vg_data/lv_data
xfs_growfs /data                      # ← mountpoint وليس device!
```

> [!tip] اختصار المحترفين: حرف `-r`
> ```bash
> lvextend -r -l +100%FREE /dev/vg_data/lv_data
> ```
> `-r` = ينفذ `xfs_growfs` / `resize2fs` ==تلقائياً== — الخطوتين في أمر واحد.

> [!note] بدون LVM؟
> `parted /dev/sda resizepart 1 100%` ثم `xfs_growfs /mountpoint` — نفس المنطق: طبقة تحت ثم طبقة فوق.

## ⬇️ Step 4 — التقليص (Offline — ext4 فقط)

> [!danger] XFS لا يتقلص أبداً!
> الحل الوحيد: ==backup → mkfs من جديد بحجم أصغر → restore==.
>Traditional: صممت للنمو فقط.

ترتيب التقليص ==عكس== التوسيع — من فوق لتحت:

```bash
# 0) backup أولاً — إلزامي
umount /data                                  # 1) فك التركيب
e2fsck -f /dev/vg_data/lv_data                # 2) فحص إلزامي
resize2fs /dev/vg_data/lv_data 2G             # 3) قلّص الـ FS أولاً!
lvreduce -L 2G /dev/vg_data/lv_data           # 4) ثم قلّص الـ LV
mount -a                                      # 5) أعد التركيب
```

> [!danger] عكس الترتيب = كارثة
> `lvreduce` قبل `resize2fs` ==يقص بيانات== نظام الملفات — تلف شبه مؤكد.

## 🚨 Troubleshooting

| المشكلة | السبب | الحل |
| --- | --- | --- |
| xfs_growfs: not a mounted XFS filesystem | مررت device بدل mountpoint | `xfs_growfs /data` |
| resize2fs يطلب e2fsck | فحص إلزامي قبل التقليص | `e2fsck -f /dev/...` |
| كبرت القسم والمساحة كما هي | لم تكبّر الـ FS | `xfs_growfs` / `resize2fs` |
| mkfs يرفض: existing filesystem | توقيع قديم | `wipefs -a` ثم mkfs |
| صلاحيات مفقودة على.mount vfat | FAT لا يدعم Unix permissions | استخدم ext4/xfs للبيانات |
| تلف بعد lvreduce | قلصت LV قبل FS | استرجاع من backup — الترتيب مقدس |

## 📋 Commands Cheat Sheet

| الغرض | الأمر |
| --- | --- |
| Format XFS | `mkfs.xfs /dev/DEV` |
| Format ext4 | `mkfs.ext4 /dev/DEV` |
| Format FAT32 | `mkfs.vfat -F 32 /dev/DEV` |
| إنشاء swap | `mkswap /dev/DEV` + `swapon` |
| UUID | `blkid /dev/DEV` |
| توسيع XFS | `xfs_growfs /MOUNTPOINT` |
| توسيع/تقليص ext4 | `resize2fs /dev/DEV [SIZE]` |
| فحص ext4 | `e2fsck -f /dev/DEV` |
| إصلاح XFS (unmounted) | `xfs_repair /dev/DEV` |
| توسيع LV + FS معاً | `lvextend -r -L +2G /dev/vg/lv` |
| تعديل حجم قسم | `parted /dev/sdX resizepart N END` |
| عرض التركيب والنوع | `findmnt` / `df -hT` |

## ❓ Interview Questions

> [!question]- 1.ما الافتراضي في RHEL 9 ولماذا؟
> ==XFS== — أداء عالٍ مع الملفات الكبيرة والتوازي، وتوسيع أونلاين.

> [!question]- 2.لماذا لا يمكن تقليص XFS؟
> صُمم للنمو فقط — لا يوجد تنفيذ آمن للتقليص. الحل: backup → mkfs → restore.

> [!question]- 3.الفرق بين xfs_growfs و resize2fs؟
> `xfs_growfs` يأخذ ==mountpoint== ويوسّع فقط.
> `resize2fs` يأخذ ==device== ويوسّع ويقلّص (التقليص offline).

> [!question]- 4.ما ترتيب التقليص الصحيح ولماذا؟
> ==FS أولاً ثم LV== (عكس التوسيع) — لو قلصت LV أولاً تُقص بيانات الـ FS.

> [!question]- 5.لماذا vfat لقسم EFI تحديداً؟
> مواصفة ==UEFI== تشترط FAT لقسم الـ ESP — ليس اختياراً بل إلزام firmware.

> [!question]- 6.ماذا يفعل `lvextend -r`؟
> يوسّع LV ثم يوسّع نظام الملفات ==تلقائياً== (يستدعي xfs_growfs/resize2fs).

## 🏆 Golden Rules

> [!quote] احفظها في الذاكرة العضلية!
> 1. التوسيع: من تحت لفوق (LV → FS) | التقليص: ==العكس== (FS → LV)
> 2. ==XFS لا يتقلص== — خطط للأحجام من البداية
> 3. `xfs_growfs` ياخد ==mountpoint== مش device
> 4. `e2fsck -f` قبل أي تقليص ext4
> 5. `lvextend -r` = الخطوتين في أمر
> 6. `mkfs` يمحو — `wipefs` + backup أولاً
> 7. UUID من `blkid` → fstab → اختبر `mount -a` ([[fstab]])

## 📚 Related Notes
- [[Storage]] — الفهرس الرئيسي
- [[Partitioning]] — الطبقة اللي تحت
- [[LVM]] — lab التوسيع الكامل بالـ outputs
- [[RAID]] — mkfs مباشرة على المصفوفة
- [[fstab]] — التثبيت الدائم