---
title: Linux Fundamentals
created: 2026-11-04
updated: 2026-11-14
tags:
  - linux
  - cli
  - devops
  - study-notes
aliases:
  - Linux Notes
  - Introduction to Linux
  - Linux Fundamentals
source: https://github.com/trimstray/the-book-of-secret-knowledge
---

# Linux Fundamentals

> [!info] عن هذه الملاحظة
> مرجع ==الأساسيات==: الأوامر، الملفات، الصلاحيات، المستخدمين، الخدمات والشبكات.
> دمج لملاحظتي `Linux Fundamentals` و`Linux Administration Notes` (الأجزاء الأساسية منها).
> الإدارة والجدولة: [[Linux System Administration]] — السكريبتات: [[Linux Shell Scripting - Complete Guide]]

## Contents

- [[#1. Introduction to Linux]]
- [[#2. Linux Architecture]]
- [[#3. Linux File System]]
- [[#4. Navigation Commands]]
- [[#5. File & Directory Commands]]
- [[#6. Shell Environment & PATH]]
- [[#7. Searching & Text Processing]]
- [[#8. Process Management]]
- [[#9. Disk & Memory Commands]]
- [[#10. Networking Commands]]
- [[#11. Package Management]]
- [[#12. Shell Scripting Basics]]
- [[#13. Linux in Real Projects]]
- [[#14. Most Asked Linux Interview Questions]]
- [[#15. Cheat Sheet & Roadmap]]
- [[#16. User & Group Management]]
- [[#17. Permissions & Ownership]]
- [[#18. Systemd & Service Management]]
- [[#19. Archive & Compression]]
- [[#20. Troubleshooting & System Logs]]
- [[#21. Security & Firewall]]

## 1. Introduction to Linux

> [!quote]
> "Linux is a ==free and open-source== operating system based on the Unix kernel. It is ==secure, stable, and highly customizable==."

### History

- Created by ==Linus Torvalds== in ==1991==.
- Inspired by Unix and developed as a hobby project.
- Released under the ==GNU General Public License (GPL)==.
- Today, Linux powers servers, desktops, cloud, mobile devices and more!

### Features

- **Open Source** – free to use and modify.
- **Multi-user** – multiple users can work simultaneously.
- **Multi-tasking** – handles multiple tasks efficiently.
- **Secure** – strong permissions and user management.
- **Stable** – runs for long time without reboot.
- **Customizable** – can be adapted to any need.

### Why Linux?

- It's free and open-source.
- Highly secure and reliable.
- Preferred choice for servers & cloud.
- Supports a wide range of hardware.
- Strong community support.
- Ideal for developers, DevOps and QA.

### Where Linux is Used

Servers · Web Hosting · Cloud Computing · DevOps · Embedded Systems · Mobile (Android) · Super Computers

### Popular Linux Distributions (Distros)

A Linux distribution is a packaged version of the ==Linux kernel== bundled with tools, libraries, and applications.

| Distro         | Highlights                          |
| -------------- | ----------------------------------- |
| **Ubuntu**     | Beginner-friendly, widely used      |
| **Debian**     | Stable and secure                   |
| **CentOS**     | Server-focused (RHEL-based)         |
| **RHEL**       | Enterprise-grade (Commercial)       |
| **Fedora**     | Latest features and innovation      |
| **Arch Linux** | Lightweight and highly customizable |
### Linux vs Windows

| Basis | Linux | Windows |
| --- | --- | --- |
| License | Open Source (Free) | Proprietary (Paid) |
| Cost | Free | Paid |
| Security | More secure, less vulnerable | More vulnerable to malware |
| Customization | Highly customizable | Limited customization |
| Performance | Lightweight, faster | Resource intensive |
| User Interface | Varies (Multiple Desktop Environments) | Graphical, user-friendly |
| Use Case | Servers, DevOps, Cloud, Development | Gaming, Office, Desktop use |

> [!tip] Remember
> Linux is not just an OS, ==it's an ecosystem==.
> ==Most of the internet runs on Linux!==

## 2. Linux Architecture

Linux follows a ==layered architecture== where each layer plays a specific and important role.

### The Five Layers

1. **User Space** – contains all user applications, programs and commands. Examples: text editors, browsers, compilers, etc.
2. **Shell** – acts as an interface between the user and the kernel. It takes commands from the user and passes them to the kernel. Examples: bash, zsh, sh, etc.
3. **Kernel** – ==the heart of Linux==. It manages system resources, handles system calls and acts as a bridge between software and hardware. Examples: process management, memory management, device drivers, etc.
4. **System Libraries (e.g. `glibc`)** – sit between User Space/Shell and the Kernel; provide pre-built common functions that applications use to communicate with the kernel via system calls.
5. **File System** – organizes and manages data on storage devices. It handles files, directories, permissions and disk space. Examples: ext4, xfs, btrfs, etc.
6. **Hardware** – physical components of the system like CPU, RAM, Disk, Network Cards, I/O Devices, etc. Kernel communicates with hardware through device drivers.


```text
User Space ↔ Shell ↔ Kernel ↔ File System ↔ Hardware
```

### How It Works?

`User → Shell → Kernel → File System → Hardware`

1. User gives a command.
2. Shell interprets it.
3. Kernel processes it.
4. File system reads/writes data.
5. Hardware executes the task.

### Quick Commands

```bash
uname -a          # System information
cat /proc/version # Kernel details
whoami            # Current user
ls /              # List root directory
ps aux            # Running processes
df -h             # Disk usage
free -m           # Memory usage
```

> [!example] Check kernel version
> ```bash
> uname -r    # show kernel version
> ```
> Output: `6.5.0-25-generic`

> [!tip] Remember
> ==Everything in Linux is a file.==
> ==Kernel is always in control.==
> Without kernel, system cannot function.
> Shell is optional, but very powerful.

## 3. Linux File System

==Everything in Linux is a file.== Files are organized in a ==hierarchical tree structure== following the ==FHS (Filesystem Hierarchy Standard)== starting from the root (`/`).

### File System Hierarchy

```text
/ (root)
├── /home   # User home directories
├── /etc    # System configuration files
├── /var    # Variable data (logs, spool, cache)
├── /usr    # User programs and utilities
├── /opt    # Optional add-on software
├── /tmp    # Temporary files
└── /bin    # Essential user binaries
```

### Important Directories

| Directory  | Purpose                                                                   |
| ---------- | ------------------------------------------------------------------------- |
| `/` (root) | The top of the directory tree. All files and directories start from here. |
| `/home`    | Contains home directories for users. e.g. `/home/rakesh`                  |
| `/etc`     | Stores system-wide configuration files. Important for system settings.    |
| `/var`     | Holds variable data like logs, spool files, cache, databases, mail, etc.  |
| `/usr`     | Contains most user programs, libraries, documentation and utilities.      |
| `/opt`     | Used to install optional third-party software.                            |
| `/tmp`     | Stores temporary files. Files may be deleted on reboot.                   |
| `/bin`     | Contains essential command binaries needed for system operation.          |
| `/sbin`    | Essential system/admin binaries (e.g. `fdisk`, `reboot`).                 |
| `/root`    | Home directory of the ==root (admin)== user.                              |
| `/dev`     | Device files representing hardware (e.g. `/dev/sda`, `/dev/tty`).         |
| `/proc`    | Virtual filesystem exposing process and system information.               |
| `/sys`     | Virtual filesystem exposing kernel and hardware information.              |
| `/lib`     | Essential shared libraries required by `/bin` and `/sbin`.                |
| `/mnt`     | Temporary mount point for filesystems.                                    |
| `/media`   | Auto-mount point for removable media (USB, CD/DVD).                       |


### Example Structure

```text
/
├── home
│   └── rakesh
├── etc
├── var
│   ├── log
│   └── cache
├── usr
│   ├── bin
│   └── local
├── opt
├── tmp
└── bin
```

> [!tip] Tip
> ==Root (`/`)== is the base of the entire system.
> Linux is ==case-sensitive==.
> Every file or directory inherits permissions from its parent.
> Use `"/"` in paths (not `\\` like Windows).

### Quick Commands

```bash
ls /       # List root directory
ls /etc    # List /etc directory
cd /home   # Go to /home
tree /     # Show directory tree (if installed)
tree / -L 2   # Show directory structure (2 levels deep)
```

> [!tip] Remember
> Root (`/`) is the starting point of everything.
> Keep system directories safe. Do not modify files unless you know what you are doing.
> Use meaningful names for files & directories.

## 4. Navigation Commands

Navigation commands help us ==move around== the Linux file system and ==explore== directories.

### 1) `pwd` — Print Working Directory

- Shows the current directory you are in.
- Displays the full path.

```bash
$ pwd
/home/rakesh/projects
```

### 2) `ls` — List files and directories

- Lists files and directories in the current directory.
- Shows the content of a directory.

```bash
$ ls
file1.txt  docs  scripts  test

ls -ltr /app/logs   # Sort by modification time (newest files at the bottom)

```

### 3) `cd` — Change Directory

- Change your current directory to another.
- Use `..` to go back to parent directory.

```bash
$ cd /home/rakesh/docs
$ pwd
/home/rakesh/docs
```

### 4) `tree` — Tree View

- Displays directories in a tree like structure.
- Useful to understand folder structure.

```bash
$ tree
.
├── docs
│   ├── file1.txt
│   └── file2.txt
└── scripts
    └── run.sh
```

### 5) `clear` — Clear Screen

- Clears the terminal screen for a fresh start.
- Useful when the terminal looks cluttered.

```bash
$ clear   # Screen will be cleared
```

### 6) `history` — Command History

- Shows previously executed commands.
- Helps to reuse old commands.
- Useful in troubleshooting.

```bash
$ history
  1  ls
  2  cd /home/rakesh
  3  pwd
  4  tree
  5  clear
```

### Real Example (Practical Usage)

```bash
pwd       # see current location   → /home/rakesh
ls        # list files & folders   → docs  scripts  test  notes.txt
cd docs   # go to docs folder      → -
pwd       # confirm new location   → /home/rakesh/docs
tree      # show tree structure    → file1.txt  file2.txt
cd ..     # go back to parent      → /home/rakesh
history   # view command history   → 1 ls | 2 cd docs | 3 pwd | 4 tree | 5 clear
```

> [!tip] Remember
> Use ==Tab== key for auto-completion.
> Use `cd ..` to go one level up.
> Use `cd ~` to go to home directory.

## 5. File & Directory Commands

These commands help us to ==create, manage, copy, move and view== files & directories in Linux.

### 1) `mkdir` — Make Directory

- Creates a new directory.
- `-p` creates parent directories if not exist.

```bash
$ mkdir myfolder
$ mkdir -p a/b/c
```

### 2) `touch` — Create File

- Creates an empty file or updates timestamp.
- Use to create multiple files.

```bash
$ touch file.txt
$ touch a.txt b.txt
$ touch {1..3}.txt   # multiple files
```

### 3) `cp` — Copy Files or Directories

- Copies files or directories from one place to another.
- `-r` for recursive copy (directory).

```bash
$ cp file.txt backup.txt
$ cp -r myfolder newfolder
$ cp -r dir1/ dir2/
```

### 4) `mv` — Move or Rename

- Moves files/directories or renames them.
- Used to move a file to another location.

```bash
$ mv file.txt docs/
$ mv oldname.txt newname.txt
$ mv myfolder newfolder
```

### 5) `rm` — Remove Files

- Deletes files.
- Use `-r` to delete directories.
- Use `-f` to force delete (without confirmation).

```bash
$ rm file.txt
$ rm a.txt b.txt
$ rm -r myfolder
$ rm -rf dir1/   # force delete
```

### 6) `rmdir` — Remove Directory

- Deletes empty directories only.
- Used when directory is empty.
- For non-empty directory, use `rm -r`.

```bash
$ rmdir emptydir
$ rmdir -p a/b/c
$ rm -r nonemptydir   # if not empty
```

### 7) `cat` — Concatenate & View

- Displays the content of a file on terminal.
- Can also view multiple files.

```bash
$ cat file.txt
$ cat file1.txt file2.txt
$ cat -n file.txt   # with line no.
```

### 8) `nano` — Nano Editor

- Simple and easy text editor in terminal.
- Useful for editing configuration files.

```bash
$ nano file.txt
# Ctrl + O  →  Save
# Ctrl + X  →  Exit
```

### 9) `vi` — Vi Editor

- Powerful text editor (Advanced).
- Used in most Linux systems.
- Different modes: insert, command, visual.

```bash
$ vi file.txt
# i    →  Insert mode
# Esc  →  Command mode
# :wq  →  Save & Exit
# :q!  →  Quit without save
```

### File Types in Linux

| Type | Symbol | Description |
| --- | --- | --- |
| Regular File | `-` | Normal file (text, code, binary) |
| Directory | `d` | Folder containing files/directories |
| Symbolic Link | `l` | Shortcut pointing to another file/path |
| Device File | `c` / `b` | Character or block device (in `/dev`) |
| Named Pipe | `p` | Used for inter-process communication (IPC) |

### 10) Inspecting & Viewing Files (`file`, `stat`, `less`, `more`)

```bash
file file.txt                        # Show file type (text, binary, script, etc.)
stat file.txt                        # Show detailed metadata (size, inode, timestamps)
less logs.txt                        # View file content page by page (scrollable, q to quit)
more logs.txt                        # View file content page by page (forward only)
ls -la                               # Long list including hidden files (starting with .)
ls -lh                               # Long list with human-readable sizes
mkdir -p project/{src,logs,config}   # Create project directory structure at once
du -h --max-depth=1                  # Check size of each folder one level deep
rename 's/old/new/' *.txt            # Batch rename multiple files using regex
```

### Common Options

| Option | Meaning |
| --- | --- |
| `-r` | Recursive (for directories) |
| `-f` | Force (no confirmation) |
| `-i` | Interactive (ask before delete) |
| `-v` | Verbose (show details) |
| `-p` | Create parent directories |
| `-n` | No overwrite |
| `-a` | Archive (copy with attributes) |
| `--help` | Show help |
| `-l` | Long listing format |
| `-h` | Human readable size |

> [!warning] Be careful
> ==Use `rm -rf` carefully== (permanent delete). Be careful while using `vi` and `rm` commands.

> [!tip] Remember
> Use `cp -r` to copy directories.
> Use `rm -r` to delete directories.
> Use ==Tab== key for auto-completion.
> Use `man <command>` to get help.

## 6. Shell Environment & PATH

#linux/shell #linux/path

بيئة الـ shell نفسها: أين يعيش الأمر الذي تكتبه، وكيف يراه النظام، وكيف يتصرف الـ shell مع المتغيرات والاقتباسات.

### 6.1 Locating Commands & PATH

```bash
which ls
whereis ls
echo $PATH
env               # Display all current environment variables
type cd             # Identify if a command is a shell builtin, alias, or binary
printenv            # Display environment variables (alternative to env)

```

- `which` → المسار التنفيذي الذي سيُنفَّذ فعلًا عند كتابة الأمر.
- `whereis` → المسار + صفحات الـ manual + ملفات المصدر إن وُجدت.
- `$PATH` → قائمة المجلدات التي يبحث فيها الـ shell عن الأوامر بالترتيب.

### 6.2 Simple Script Example & Adding a Directory to PATH

مثال سكريبت بسيط موجود في `/root`:

```bash
# cat script01
#!/bin/bash
pwd
cal
```

إضافة مسار للـ PATH:

```bash
export PATH="$PATH:/root"
```

#### Making PATH Changes Permanent

```bash
# لكل user
vim ~/.bashrc
export PATH="$PATH:/root"        # permanent
```

```bash
# global (لكل المستخدمين)
vim /etc/bashrc
export PATH="$PATH:/path/to/dir"
```

> [!tip] أفضل من إضافة `/root`
> ضع سكريبتاتك في `~/bin` أو `/opt/scripts` وأضف هذا المجلد للـ PATH بدل مجلد عشوائي.
> الشرح الكامل (per-user / system-wide / تحذيرات الأمان / cron لا يقرأ `.bashrc`): [[Linux Shell Scripting - Complete Guide#19. Advanced Shell Scripting]]

### 6.3 Variables & Command Substitution

```bash
[root@control ~]# echo $var
string1
[root@control ~]# echo ${var}
string1
[root@control ~]# echo $(var)
bash: var: command not found...
[root@control ~]# echo $(date)
Tue Apr 29 02:57:47 PM EEST 2025
[root@control ~]# echo "the date is $(date)"
the date is Tue Apr 29 03:00:04 PM EEST 2025
[root@control ~]# echo "the date is $(date +%x)"
the date is 04/29/2025
[root@control ~]# echo 'the date is $(date +%x)'
the date is $(date +%x )
```

> [!tip] Double Quotes vs Single Quotes
> `"..."` بتنفّذ الـ `$(...)` اللي جواها، لكن `'...'` بتطبع النص ==حرفيًا== من غير تنفيذ.
> ولاحظ إن `$(var)` بتحاول تنفّذ `var` كأمر — الصح `$var` أو `${var}`.

المتغيرات داخل السكريبتات (الأنواع، `read`، متغيرات البيئة): [[Linux Shell Scripting - Complete Guide#3. Variables & User Input]]

### 6.4 Exit Status (Return Value)

```bash
[root@control ~]# echo $?
0
[root@control ~]# ls-l
bash: ls-l: command not found...
[root@control ~]# echo $?
127
```

> [!info] قراءة الـ Exit Code
> ==0== يعني نجاح، وأي قيمة تانية يعني خطأ — و==127== معناها command not found.

جدول الأكواد الكامل ومعالجة الأخطاء (`set -e` / `trap` / `pipefail`): [[Linux Shell Scripting - Complete Guide#14. Exit Status & Error Handling in Shell Scripting]]

## 7. Searching & Text Processing

These commands help us to ==search== for files and text, and ==process data== efficiently in Linux.

| #   | Command | Description                                 | Example                            | What it does                                        |
| --- | ------- | ------------------------------------------- | ---------------------------------- | --------------------------------------------------- |
| 1   | find    | Search for files and directories.           | `find /home -name "file.txt"`      | Finds `file.txt` in `/home`.                        |
| 2   | locate  | Find files using pre-built database.        | `locate file.txt`                  | Fast file search using index.                       |
| 3   | grep    | Search for a pattern in files.              | `grep "error" app.log`             | Finds lines containing `"error"`.                   |
| 4   | awk     | Pattern scanning and processing language.   | `awk '{print $1}' data.txt`        | Prints first column from `data.txt`.                |
| 5   | sed     | Stream editor for filtering & transforming. | `sed 's/old/new/g' file.txt`       | Replaces old with new in `file.txt`.                |
| 6   | sort    | Sort lines of text files.                   | `sort names.txt`                   | Sorts names in alphabetical order.                  |
| 7   | uniq    | Remove duplicate lines.                     | `uniq list.txt`                    | Shows only unique lines.                            |
| 8   | wc      | Count lines, words and characters.          | `wc file.txt`                      | Counts lines, words and characters.                 |
| 9   | head    | Display first few lines of a file.          | `head -n 10 file.txt`              | Shows first 10 lines.                               |
| 10  | tail    | Display last few lines of a file.           | `tail -n 10 file.txt`              | Shows last 10 lines.                                |
| 11  | diff    | Compare two files line by line.             | `diff file1.txt file2.txt`         | Shows differences between two files.                |
| 12  | cut     | Extract sections/columns from each line.    | `cut -d: -f1 /etc/passwd`          | Prints first field using `:` as delimiter.          |
| 13  | tr      | Translate or delete characters.             | `echo "hello" \| tr 'a-z' 'A-Z'`   | Converts lowercase characters to uppercase.         |
| 14  | xargs   | Build and execute commands from stdin.      | `find . -name "*.tmp" \| xargs rm` | Passes piped items as arguments to another command. |
| 15  | tee     | Write stdout to file AND screen at once.    | `ls -l \| tee out.txt`             | Saves output while still displaying it in terminal. |
| 16  | paste   | Merge lines of multiple files side by side. | `paste file1 file2`                | Joins corresponding lines from files.               |
| 17  | nl      | Number lines of a file.                     | `nl file.txt`                      | Prints file content with line numbers.              |

### Sample Workflow

1. Find all log files: `find /var/log -name "*.log"`
2. Search for "error": `grep "error" app.log`
3. Sort and remove duplicates: `grep "error" app.log | sort | uniq`
4. Count total lines: `wc -l app.log`

### Example: `error.log`

```text
2024-05-25  ERROR  Failed to start
2024-05-25  INFO   Service running
2024-05-25  ERROR  Connection lost
2024-05-25  WARN   Low memory
2024-05-25  ERROR  Timeout occurred
```

Search ERROR lines:

```bash
$ grep "ERROR" error.log
2024-05-25  ERROR  Failed to start
2024-05-25  ERROR  Connection lost
2024-05-25  ERROR  Timeout occurred
```

> [!tip] Remember
> `grep` is ==case-sensitive==.
> `locate` is faster but may be outdated.
> Use `|` (pipe) to combine commands.
> Always read `man` page (`man command`) for options.
> Use quotes (`" "`) for patterns with spaces.
> These commands are very useful for ==log analysis and troubleshooting==.

### 7.1 grep — بحث متكرر داخل مجلد

```bash
grep madel -r /etc/
```

### 7.2 Common Patterns

| Pattern                                            | Meaning                                                        |
| -------------------------------------------------- | -------------------------------------------------------------- |
| `grep ^# file`                                     | السطور اللي بتبدأ بـ `#`                                       |
| `grep ^% file`                                     | السطور اللي بتبدأ بـ `%`                                       |
| `grep AppStream$ file`                             | السطور اللي بتنتهي بـ `AppStream`                              |
| `grep -v ^% file`                                  | عرض أي سطر ==لا== يبدأ بـ `%` (عكس المطابقة)                   |
| `grep -w 6010`                                     | مطابقة الكلمة كاملة فقط                                        |
| `grep -r word /path`                               | بحث recursive داخل المجلدات                                    |
| `grep -i "error" app.log`                          | بحث غير حساس لحالة الأحرف (case-insensitive)                   |
| `grep -n "error" app.log`                          | عرض أرقام السطور المطابقة                                      |
| `grep -c "error" app.log`                          | عدّ السطور المطابقة فقط                                        |
| `grep -C 5 "ERROR" app.log`                        | عرض 5 سطور قبل وبعد كل مطابقة (أو `-A 5` بعد، `-B 5` قبل)      |
| `grep -rl "ERROR" /app/logs/`                      | طباعة أسماء الملفات التي تحتوي على النمط فقط                   |
| `grep -o "ERROR" app.log \| wc -l`                 | عدّ ==مرات التكرار الفعلية== (بينما `grep -c` يعدّ السطور فقط) |
| `grep -v '^$' file.txt`                            | إزالة السطور الفارغة من المخرجات                               |
| `tail -f app.log \| grep --line-buffered -i ERROR` | متابعة الأخطاء لحظيًا بدون تأخير الـ buffer                    |

### 7.3 Examples on anaconda-ks.cfg

```bash
[root@control ~]# grep ^# anaconda-ks.cfg
# Generated by Anaconda 34.25.4.7
# Generated by pykickstart v3.32
#version=RHEL9
# Use graphical install
# Keyboard layouts
# System language
# Network information
# Use CDROM installation media
# Run the Setup Agent on first boot
# Generated using Blivet version 3.6.0
# Partition clearing information
# System timezone
# Root password
```

```bash
[root@control ~]# grep ^% anaconda-ks.cfg
%addon com_redhat_kdump --enable --reserve-mb='auto'
%end
%packages
%end
```

```bash
[root@control ~]# grep AppStream$ anaconda-ks.cfg
repo --name="AppStream" --baseurl=file:///run/install/sources/mount-0000-cdrom/AppStream
```

```bash
grep -v ^% anaconda-ks.cfg   # عرض أي شيء لا يحتوي على % في البداية
grep -v ^# anaconda-ks.cfg   # استبعاد سطور التعليقات
```

> [!tip] grep داخل السكريبتات
> خيارات `-i` / `-n` / `-C` واستخدام grep مع awk وsed في التحليل: [[Linux Shell Scripting - Complete Guide#13. Text Processing Commands in Shell Scripting]]


### 7.4 Quick Reference — `awk` & `sed`

**`awk` — استخراج ومعالجة الأعمدة:**

| Command | Description |
| --- | --- |
| `awk '{print $1, $3}' file.txt` | طباعة العمود الأول والثالث |
| `awk -F',' '{print $2}' data.csv` | استخدام الفاصلة (`,`) كفاصل أعمدة |
| `awk -F: '{print $1}' /etc/passwd` | استخراج أسماء المستخدمين من `/etc/passwd` |
| `awk '{sum+=$2} END {print sum}' file.txt` | حساب مجموع العمود الثاني |
| `awk '/error/ {print $0}' app.log` | طباعة السطور التي تحتوي على `error` |
| `free -h \| awk '/Mem:/ {print $2}'` | استخراج إجمالي الذاكرة (RAM) |

**`sed` — تعديل وتحويل النصوص:**

| Command                              | Description                                |
| ------------------------------------ | ------------------------------------------ |
| `sed 's/old/new/' file.txt`          | استبدال أول تطابق في كل سطر                |
| `sed 's/old/new/g' file.txt`         | استبدال كل التطابقات في الملف              |
| `sed -n '5p' file.txt`               | طباعة السطر رقم 5 فقط                      |
| `sed '10d' file.txt`                 | حذف السطر رقم 10                           |
| `sed -i 's/error/warning/g' app.log` | تعديل الملف ==فعليًا في مكانه (in-place)== |

```
# find — البحث بالوقت والحجم والملفات الفارغة
find /app/logs -type f -mmin -60                     # Files modified in the last 60 minutes
find /app -type f -empty                             # Find zero-byte (empty) files

# sed — طباعة نطاق محدد
sed -n '100,200p' file.txt                           # Print lines 100 to 200
sed -n '/START/,/END/p' file.txt                     # Print lines between two patterns

# awk — تحليل Access Logs (مقابلات الإنتاج)
awk '{print $NF}' file.txt                           # Print the last column
awk '$3==500' access.log                             # Filter HTTP 500 responses (if col 3 = status)
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10   # Top 10 client IPs
```

**دمج `grep` و `awk` و `sed` معًا:**

```bash
# استخراج سطور ERROR ثم العمود 1 و3 ثم استبدال ERROR بـ FAIL
grep "ERROR" app.log | awk '{print $1, $3}' | sed 's/ERROR/FAIL/'
```

## 8. Process Management

These commands help us ==view, control and manage== processes running in a Linux system.

| #   | Command  | Description                                   | Example               | What it does                                              |
| --- | -------- | --------------------------------------------- | --------------------- | --------------------------------------------------------- |
| 1   | ps       | Report snapshot of current processes.         | `ps aux`              | Shows currently running processes.                        |
| 2   | top      | Real-time view of processes.                  | `top`                 | Displays dynamic process information.                     |
| 3   | htop     | Interactive process viewer (better than top). | `htop`                | User-friendly real-time process monitor.                  |
| 4   | jobs     | List background and stopped jobs.             | `jobs`                | Shows jobs started from current shell.                    |
| 5   | bg       | Resume a stopped job in background.           | `bg %1`               | Continues job in background.                              |
| 6   | fg       | Bring a background job to foreground.         | `fg %1`               | Brings job to current terminal.                           |
| 7   | kill     | Terminate a process using PID.                | `kill 1234`           | Sends TERM signal (default).                              |
| 8   | kill -9  | Force kill a process (SIGKILL).               | `kill -9 1234`        | Forcefully terminates unresponsive process.               |
| 9   | nice     | Run a process with modified priority.         | `nice -n 10 command`  | Adjusts process priority (lower value = higher priority). |
| 10  | pstree   | Display processes as a tree.                  | `pstree`              | Shows parent-child process hierarchy.                     |
| 11  | kill -15 | Graceful stop (SIGTERM).                      | `kill -15 1234`       | Explicitly sends SIGTERM (default for `kill`).            |
| 12  | pkill    | Kill processes by name.                       | `pkill nginx`         | Terminates matching processes without needing PID.        |
| 13  | killall  | Kill all instances by exact name.             | `killall nginx`       | Terminates all processes with that exact name.            |
| 14  | renice   | Change priority of a running process.         | `renice -n 5 -p 1234` | Adjusts priority (NI) of an active PID.                   |
| 15  | pgrep    | Find PID by process name.                     | `pgrep nginx`         | Returns matching PIDs directly (great for scripts).       |
| 16  | nohup    | Run command immune to hangups.                | `nohup ./script.sh &` | Keeps process running even after closing terminal/SSH.    |
| 17  | stress   | Generate synthetic CPU/RAM load.              | `stress --cpu 4`      | Tests system behavior under CPU stress.                   |

### Examples

```bash
$ ps aux          # View all running processes
$ top             # Real-time monitoring
$ htop            # Better process viewer
$ jobs            # List background jobs
$ bg %1           # Resume job in background
$ fg %1           # Bring job to foreground
$ kill 1234       # Kill process with PID
$ kill -9 1234    # Force kill process
$ nice -n 10 cmd  # Run with low priority

$ ps -ef | grep nginx                  # Search for a specific process
$ pgrep nginx && kill $(pgrep nginx)   # Find PID and kill it
$ top -bn1 | grep "Cpu(s)"             # One-shot CPU snapshot (for scripts)


pidof java                           # Get PID(s) of a running program
kill -l                              # List all available Linux signals
ps -p <PID> -o pid,ppid,stat,lstart,etime,cmd   # Start time, elapsed time & parent PID
readlink /proc/<PID>/cwd             # Check current working directory of a running process
readlink /proc/<PID>/exe             # Check exact binary path of a running process
time <command>                       # Measure how long a command takes to run

```

### Job States

- **Running (R)** – running in foreground.
- **Sleeping (S)** – running in background / waiting.
- **Stopped (T)** – stopped (use `Ctrl + Z`).
- **Zombie (Z)** – process completed but not removed.
- **Uninterruptible Sleep (D)** – waiting for I/O (e.g. disk); cannot be interrupted or killed until I/O finishes. 

### Types of Processes 
- **Foreground Process** – runs in the terminal and accepts user input. 
- **Background Process** – runs without blocking the terminal (started with `&`). 
- **Daemon Process** – long-running background service (e.g. `sshd`, `nginx`, `cron`). 
- **Zombie Process** – finished execution but still has an entry in the process table. 
- **Orphan Process** – running process whose parent has terminated (adopted by `systemd`/PID 1).


```bash
$ jobs
[1]+  Running   sleep 100 &
[2]-  Stopped   vim file.txt
```

> [!tip] Remember
> Use `ps aux` for detailed process information.
> `top` is useful but `htop` is easier to use.
> Use `Ctrl + Z` to stop a process and `bg` to run in background.
> `kill` sends default ==TERM== signal (kind request).
> ==`kill -9` should be used only when the process does not stop.==
> `nice` starts a process with lower priority (higher number = lower priority).

> [!see-also] الأولويات بالتفصيل
> `nice` / `renice` / قيم NI وPRI والفرق بين `ps` و`top`: [[Linux System Administration#9. Process Priority — nice & renice]]
> تشخيص اختناق الـ Disk I/O في الإنتاج (`iostat` / `iotop` / `pidstat` / `smartctl` / `fio`): [[Linux L3 Troubleshooting Notes#20. Disk I/O Bottleneck Troubleshooting]]

## 9. Disk & Memory Commands

These commands help us monitor ==disk usage==, ==memory usage== and ==system performance==.

| #   | Command | Description                              | Example                     | What it does                                                 |
| --- | ------- | ---------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| 1   | df      | Disk free space report.                  | `df -h`                     | Shows disk space usage of file systems.                      |
| 2   | du      | Estimate file and directory space usage. | `du -sh /home/user`         | Shows size of files or directories.                          |
| 3   | free    | Display memory usage.                    | `free -h`                   | Shows total, used and free memory.                           |
| 4   | uptime  | Show how long system has been running.   | `uptime`                    | Shows system uptime and load average.                        |
| 5   | vmstat  | Report virtual memory statistics.        | `vmstat 1 5`                | Shows processes, memory, CPU and I/O stats.                  |
| 6   | iostat  | I/O statistics for CPU and devices.      | `iostat -xz 1 5`            | Shows CPU and device I/O performance.                        |
| 7   | mount   | Mount a file system.                     | `mount /dev/sdb1 /mnt`      | Attaches a filesystem to a directory.                        |
| 8   | umount  | Unmount a file system.                   | `umount /mnt`               | Detaches a filesystem from a directory.                      |
| 9   | lsblk   | List block devices.                      | `lsblk`                     | Shows information about block devices.                       |
| 10  | df -i   | Show inode usage.                        | `df -i`                     | Shows inode usage of file systems.                           |
| 11  | ncdu    | Interactive disk usage explorer.         | `ncdu -x /var`              | Browse and sort disk usage interactively.                    |
| 12  | fdisk   | View/manage disk partitions.             | `sudo fdisk -l`             | Lists disks and partition tables.                            |
| 13  | mkfs    | Create a filesystem on a partition.      | `sudo mkfs.ext4 /dev/sdb1`  | Formats a partition (ext4 / xfs / btrfs).                    |
| 14  | mpstat  | Per-CPU usage statistics (sysstat).      | `mpstat -P ALL 1`           | Shows CPU utilization per core every second.                 |
| 15  | sar     | Historical performance stats (sysstat).  | `sar -u 1 5` / `sar -r 1 5` | Reports CPU (`-u`) or memory (`-r`) statistics.              |
| 16  | nmon    | All-in-one interactive monitor.          | `nmon`                      | Monitors CPU, RAM, Disk, and Network together.               |
| 17  | iotop   | Real-time disk I/O monitor by process.   | `sudo iotop -o`             | Shows which processes are actively reading/writing to disk.  |
| 18  | smem    | Detailed memory usage (PSS/USS).         | `smem -r`                   | Reports actual per-process memory accounting for shared RAM. |
### Examples

```bash
$ df -h                 # disk usage in human readable
$ du -sh /var/log       # size of /var/log
$ free -h               # memory usage
$ uptime                # system uptime
$ vmstat 1 5            # memory & CPU stats
$ iostat -xz 1 5        # I/O statistics
$ mount /dev/sdb1 /mnt  # mount device
$ umount /mnt           # unmount device
$ lsblk                 # list block devices
$ df -i                 # inode usage
ps -ef | grep nginx
```

### Sample Outputs

```bash
$ df -h
Filesystem   Size  Used  Avail  Use%  Mounted on
/dev/sda1    50G   20G   28G    42%   /
/dev/sdb1    100G  60G   40G    60%   /data
```

```bash
$ free -h
       total  used  free  shared  buff/cache  available
Mem:   7.7G   2.1G  2.3G  0.3G    3.3G        4.8G
Swap:  2.0G   0B    2.0G
```

```bash
$ uptime
14:35:21 up 3 days, 2:45, 2 users,
load average: 0.08, 0.12, 0.15
```

> [!tip] Remember
> Use `df -h` to check disk space regularly.
> Use `free -h` to monitor memory usage.
> ==High load average means the system is busy.==
> Use `vmstat` and `iostat` for performance analysis.
> ==Always unmount before removing devices.==
> Use `lsblk` to identify devices before mounting.
> Use `df -i` to check inode usage.

> [!example] Combine commands with pipes for powerful reports
> ```bash
> $ df -h | grep sda
> $ du -hs * | sort -h
> $ free -h | grep Mem
> ```

### Hardware Info & Long-Term Monitoring

```bash
cat /proc/cpuinfo    # Detailed CPU model, cores, and flags
cat /proc/meminfo    # Detailed kernel memory statistics

lscpu                        # Detailed CPU architecture & cores summary
nproc                        # Print number of available CPU cores
cat /proc/loadavg            # Raw 1, 5, 15-min load averages
timedatectl                  # Check/set system clock and timezone
df -hT                       # Disk usage + filesystem type (ext4, xfs, etc.)
mount | grep 'ro[,)]'        # Detect any filesystem mounted read-only

```

- للمراقبة المستمرة طويلة المدى واللوحات الرسومية (Dashboards) في بيئات الإنتاج: ==Prometheus + Grafana==.

### Partitioning, Formatting & LVM Basics

خط سير تجهيز قرص جديد: `fdisk → mkfs.ext4 → mount → /etc/fstab`

**1) التقسيم والفحص والتهيئة (Partition, Check & Format):**

```bash
sudo fdisk -l                  # List disks and partition tables
sudo parted -l                 # Display partition tables (supports GPT & large disks)
sudo blkid                     # Show UUID and filesystem type of block devices
sudo fdisk /dev/sdb            # Interactive partitioning (n: new, p: primary, 1: number, w: write)
sudo fsck /dev/sdb1            # Check and repair a filesystem (must be unmounted first)
```

| File System | Command | Use Case |
| --- | --- | --- |
| `ext4` | `sudo mkfs.ext4 /dev/sdb1` | General purpose (default on Debian/Ubuntu) |
| `xfs` | `sudo mkfs.xfs /dev/sdb1` | High performance, large files (default on RHEL) |
| `btrfs` | `sudo mkfs.btrfs /dev/sdb1` | Advanced features, snapshots, compression |

> [!tip] Persistent Mount
> لجعل الـ mount دائمًا بعد إعادة التشغيل، أضف سطرًا بالـ `UUID` في ملف ==`/etc/fstab`==.

**2) أساسيات LVM (`PV → VG → LV`):**

| Task                        | Command                                                                        |
| --------------------------- | ------------------------------------------------------------------------------ |
| Create Physical Volume (PV) | `sudo pvcreate /dev/sdb1`                                                      |
| Create Volume Group (VG)    | `sudo vgcreate vg_data /dev/sdb1`                                              |
| Create Logical Volume (5G)  | `sudo lvcreate -L 5G -n lv_data vg_data`                                       |
| Format & Mount LV           | `sudo mkfs.ext4 /dev/vg_data/lv_data && sudo mount /dev/vg_data/lv_data /data` |
| Extend LV (+5G)             | `sudo lvextend -L +5G /dev/vg_data/lv_data`                                    |
### Interactive Disk Usage — `ncdu`

Instead of re-running `du` at every level, ==`ncdu`== opens an ==interactive explorer==: navigate directories, sort by size, and drill down from the same scan.

```bash
sudo apt install ncdu          # not installed by default (Debian/Ubuntu)
du -xhd1 /var | sort -h        # traditional way: re-run for each level
ncdu -x /var                   # interactive explorer for /var
ncdu -x /                      # scan root without crossing mount points
ncdu / --exclude /backup       # exclude a path even on the same filesystem
```

> [!tip] The `-x` flag
> Stays on the ==same filesystem== — it does not descend into other mount points. Important when a backup disk is mounted under `/`.

#### Keys Inside the Interface

| Key | Action |
| --- | --- |
| ↑ ↓ + Enter | Navigate; `Enter` opens a directory, ← goes back |
| s | Sort by size (press again for descending) |
| g | Cycle size graph / percentage / both / none |
| a | Toggle disk usage vs apparent size |
| e | Show/hide hidden & excluded items |
| d | Delete selected item (asks for confirmation) |
| q | Quit |

> [!tip] When to use `a`?
> Compare the space ==actually allocated on disk== with the ==apparent size== of the file contents — they differ for sparse files and block-size padding.

> [!note] `--exclude` behavior (ncdu 2.x)
> Excluded items are ==still displayed== (flagged `<`) but ==not counted== in the totals. Press `e` to hide them.

> [!warning] `df` shows more used space than `ncdu`?
> Don't assume the gap is all visible files — often ==deleted files still held open by processes==:
> ```bash
> sudo lsof +L1   # list deleted-but-open files
> ```
> Restart the holding process/service to release the space.

> [!see-also] ذات صلة
> أداة `lsof` بالكامل (بورتات / ملفات محجوزة / اتصالات): [[Linux System Administration#lsof — List Open Files]]
> تاريخ الأداء (sar / iostat / mpstat): [[Linux System Administration#8. Performance History with sysstat]]

## 10. Networking Commands

These commands help us to ==troubleshoot, test connectivity== and ==manage== network connections in Linux.

| #   | Command    | Description                                       | Example                          | What it does                                                        |
| --- | ---------- | ------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------- |
| 1   | ping       | Test connectivity between hosts.                  | `ping google.com`                | Checks if host is reachable and measures latency.                   |
| 2   | curl       | Transfer data to/from a server.                   | `curl -I https://example.com`    | Fetches data or HTTP headers from URL.                              |
| 3   | wget       | Download files from the web.                      | `wget https://file.com/file.zip` | Downloads files non-interactively.                                  |
| 4   | ssh        | Securely connect to a remote server.              | `ssh user@server.com`            | Opens secure remote shell session.                                  |
| 5   | scp        | Securely copy files between hosts.                | `scp file.txt user@host:/path`   | Copies files over SSH securely.                                     |
| 6   | netstat    | Show network connections, routing and interfaces. | `netstat -tulnp`                 | Displays active connections and listening ports.                    |
| 7   | ss         | Modern replacement for netstat.                   | `ss -tulnp`                      | Faster way to show sockets and connections.                         |
| 8   | nslookup   | Query DNS to find IP or domain information.       | `nslookup example.com`           | Looks up DNS records (A, AAAA, CNAME, MX etc).                      |
| 9   | dig        | Advanced DNS lookup tool.                         | `dig example.com`                | Detailed DNS query and troubleshooting.                             |
| 10  | traceroute | Trace the path packets take to a host.            | `traceroute google.com`          | Shows each hop to the destination.                                  |
| 11  | ip         | Inspect/configure interfaces & routes.            | `ip a` / `ip route`              | Modern replacement for `ifconfig` and `route`.                      |
| 12  | tracepath  | Trace network path (no root needed).              | `tracepath google.com`           | Discovers path and MTU to a host.                                   |
| 13  | nc         | Test open ports / TCP-UDP connectivity.           | `nc -zv google.com 80`           | Checks if a specific port is open on a host.                        |
| 14  | lsof -i    | Show processes using network ports.               | `sudo lsof -i :80`               | Lists open network sockets and owning processes.                    |
| 15  | nmap       | Network & port scanner.                           | `nmap <host>`                    | Discovers open ports and services on a host.                        |
| 16  | iftop      | Real-time bandwidth monitor.                      | `sudo iftop`                     | Displays live network bandwidth usage per connection.               |
| 17  | nload      | Live network traffic monitor.                     | `nload`                          | Visualizes incoming/outgoing bandwidth in real time.                |
| 18  | telnet     | Test raw TCP port connectivity (legacy).          | `telnet host 80`                 | Connects to a TCP port (prefer `nc -zv` or `ssh` for remote login). |
| 19  | arp        | View/manage ARP cache (legacy).                   | `arp -a`                         | Lists IP-to-MAC mappings (modern equivalent: `ip neigh`).           |
| 20  | host       | Simple DNS lookup utility.                        | `host google.com`                | Quick domain-to-IP resolution (simpler than `dig`).                 |
| 21  | ethtool    | Inspect/configure NIC hardware & stats.           | `ethtool -S eth0`                | Shows NIC traffic counters, packet drops, and hardware errors.      |
### Examples

```bash
$ ping -c 4 google.com                   # send 4 packets
$ curl -I https://example.com            # fetch headers
$ wget https://file.com/file.zip         # download file
$ ssh -p 22 user@192.168.1.10            # connect on port 22
$ scp report.pdf user@server:/home/user/ # copy file to remote server
$ netstat -tulnp                         # show listening ports
$ ss -tulnp | grep 80                    # check process on port 80
$ dig +short example.com                 # get only IP address
```

### Check & Configure Network (`ip` & DNS)

```bash
# Inspect network info
ip a                                         # Show all interfaces & IPs
ip link                                      # Show interface states (UP/DOWN)
ip addr show eth0                            # Show IP of a specific interface
ip route                                     # Show routing table
cat /etc/resolv.conf                         # Show configured DNS servers

# Basic temporary configuration (non-persistent across reboot)
sudo ip addr add 192.168.1.100/24 dev eth0   # Assign IP address
sudo ip link set eth0 up                     # Bring interface up
sudo ip link set eth0 down                   # Bring interface down
sudo ip route add default via 192.168.1.1    # Add default gateway

sysctl -a | grep net.ipv4                    # Inspect kernel networking parameters
cat /proc/net/if_inet6                       # Check if IPv6 is active in the kernel
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1   # Temporarily disable IPv6

```

> [!warning] إعدادات مؤقتة
> التعديلات بأمر `ip` ==مؤقتة وتُفقد بعد إعادة التشغيل==، وملف `/etc/resolv.conf` غالبًا يُدار تلقائيًا عبر NetworkManager. للإعداد الدائم راجع: [[Linux System Administration#16. Networking (NetworkManager)]].

### SSH & Remote Access (Keys, `sshd_config`, `rsync` & Tunneling)

**1) المصادقة بالمفاتيح (Passwordless SSH):**
يُخزَّن المفتاح الخاص في `~/.ssh/id_rsa` والعام في `~/.ssh/id_rsa.pub`. لنسخ المفتاح العام إلى السيرفر البعيد:

```bash
ssh-copy-id user@host
```

**2) أهم إعدادات `/etc/ssh/sshd_config`:**

| Setting | Purpose |
| --- | --- |
| `Port 22` | SSH listening port (default: 22) |
| `PermitRootLogin no` | تعطيل دخول `root` المباشر (موصى به أمنيًا) |
| `PasswordAuthentication yes/no` | تفعيل أو تعطيل الدخول بكلمة المرور |
| `PubkeyAuthentication yes` | تفعيل المصادقة بالمفاتيح |
| `AllowUsers user1 user2` | السماح لمستخدمين محددين فقط بالدخول |

**3) نقل ومزامنة الملفات عبر SSH (`scp` & `rsync`):**

```bash
scp file.txt user@host:/path/             # رفع ملف إلى السيرفر
scp user@host:/path/file.txt .            # تحميل ملف من السيرفر
rsync -av data/ user@host:/backup/        # مزامنة المجلدات بكفاءة (ينقل التغييرات فقط)
```

**4) تمرير المنافذ (SSH Port Forwarding / Tunneling):**

| Type               | Command Example                        | Use Case                                      |
| ------------------ | -------------------------------------- | --------------------------------------------- |
| **Local (`-L`)**   | `ssh -L 8080:localhost:80 user@host`   | الوصول لخدمة على السيرفر البعيد عبر بورت محلي |
| **Remote (`-R`)**  | `ssh -R 9090:localhost:3000 user@host` | إتاحة خدمة محلية على السيرفر البعيد           |
| **Dynamic (`-D`)** | `ssh -D 1080 user@host`                | إنشاء SOCKS proxy للتصفح الآمن                |

### Common Ports

| Port | Service |
| --- | --- |
| 20 / 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 6379 | Redis |
| 8080 | Application (Alt) |

```bash
cat /etc/services | grep -w 443 # Look up well-known service ports directly from the OS
```

> [!tip] Remember
> `ping` checks connectivity, not speed.
> `curl` is great for APIs and headers.
> Use `ssh` and `scp` for secure file transfer.
> `ss` is faster and more powerful than `netstat`.
> `dig` gives more detailed DNS information than `nslookup`.
> `traceroute` helps find network issues (hops where it fails).
> Always check ==firewall and security group== if connection fails.

> [!warning] تصحيح: منفذ SSH في scp
> `ssh` يستخدم ==`-p` الصغيرة==، بينما `scp` يستخدم ==`-P` الكبيرة==:
> ```bash
> ssh -p 2222 user@host
> scp -P 2222 file user@host:/tmp/
> ```
> أمثلة النقل الكاملة: [[Linux System Administration#13. Quick File Transfer & Watching Jobs]]

> [!example] Combine commands with pipes for network troubleshooting
> ```bash
> $ ping google.com | grep ttl
> $ ss -tulnp | grep 80
> $ dig example.com | grep ANSWER
> ```

> [!see-also] ذات صلة
> ضبط IP ثابت بـ nmcli وملفات الـ DHCP leases: [[Linux System Administration#16. Networking (NetworkManager)]]

## 11. Package Management

These commands help us ==install, update, remove== and manage software packages in Linux.

| #   | Command | Description                               | Example                 | What it does                                       |
| --- | ------- | ----------------------------------------- | ----------------------- | -------------------------------------------------- |
| 1   | apt     | Advanced Package Tool (Debian / Ubuntu).  | `apt search nginx`      | Package manager for Debian based systems.          |
| 2   | yum     | Yellowdog Updater Modified (RHEL/CentOS). | `yum list installed`    | Package manager for RHEL/CentOS (v7 and below).    |
| 3   | dnf     | Dandified YUM (Fedora/RHEL 8+).           | `dnf repolist`          | Next-generation package manager (Fedora, RHEL 8+). |
| 4   | rpm     | Red Hat Package Manager.                  | `rpm -qa \| grep httpd` | Manages RPM packages (install, query, verify).     |
| 5   | dpkg    | Debian Package Manager.                   | `dpkg -l \| grep vim`   | Manages DEB packages on Debian based systems.      |
| 6   | install | Install a package.                        | `apt install nginx`     | Installs a software package.                       |
| 7   | update  | Update package lists or upgrade packages. | `yum update`            | Updates package lists or installed packages.       |
| 8   | remove  | Remove a package.                         | `apt remove nginx`      | Removes a package from the system.                 |

### Package Managers by Distribution

- **Debian / Ubuntu:** `apt`, `dpkg`
- **RHEL / CentOS 7:** `yum`, `rpm`
- **Fedora / RHEL 8+:** `dnf`, `rpm`
- **openSUSE / SLES:** `zypper`, `rpm`
- **Cross-distro (Ubuntu / others):** `snap` (e.g. `sudo snap install <pkg>`, `snap list`)
### Common Operations Examples

**APT (Debian/Ubuntu):**

```bash
$ sudo apt update          # update list
$ sudo apt upgrade         # upgrade all
$ sudo apt install nginx   # install package
$ sudo apt remove nginx    # remove package
$ apt search docker
$ sudo apt purge nginx     # remove package + config files
$ apt show nginx           # show package details (version, size, deps)
sudo apt clean           # remove cached .deb package files to free disk space
```

**YUM/DNF (RHEL/CentOS/Fedora):**

```bash
$ sudo yum repolist        # show repos
$ sudo yum list installed  # list packages
$ sudo yum install nginx   # install package
$ sudo yum update          # update packages
$ sudo yum remove nginx    # remove package
$ dnf search docker        # search package
$ yum search docker        # search package (YUM)
$ yum info nginx           # show package info (YUM)
$ sudo dnf upgrade         # upgrade all packages (DNF)
$ dnf info nginx           # show package info (DNF)
$ sudo yum clean all       # clear all cached repos and package files
```

### Useful Options

- `-y` – automatic yes
- `-q` – quiet mode
- `--search` – search packages
- `list` – list installed packages
- `update` – update repository
- `upgrade` – upgrade all packages
- `remove` – remove a package
- `--reinstall` – reinstall a package

### RPM vs DEB

|  | RPM (Red Hat) | DEB (Debian) |
| --- | --- | --- |
| Used in | RHEL, CentOS, Fedora | Debian, Ubuntu |
| File extension | .rpm | .deb |
| Install | `rpm -ivh file.rpm` | `dpkg -i file.deb` |
| Query | `rpm -qa` | `dpkg -l` |
| Remove | `rpm -e package` | `dpkg -r package` |

> [!tip] Remember
> Use ==sudo== with package commands.
> Run ==update== before install/upgrade.
> `apt` = Debian / Ubuntu · `yum` = RHEL / CentOS 7 · `dnf` = RHEL 8+ / Fedora
> `rpm` & `dpkg` work on package files directly.
> Always check package name before install/remove.

> [!see-also] إضافات RHEL
> Package groups · `dnf history undo` · استعلامات rpm المتقدمة (`-qpi` / `-qpl` / `-qf`): [[Linux System Administration#12. Package Management Extras (RHEL)]]
> مثال عملي (تواريخ تثبيت حزم الـ kernel): [[Linux Shell Scripting - Complete Guide#21. Quick One-Liners & Snippets]]


## 12. Shell Scripting Basics

> [!info] المرجع الكامل في ملف مخصص
> شرح الـ scripting بالتفصيل (متغيرات · شروط · حلقات · دوال · وسائط · أخطاء · مشاريع) موجود في [[Linux Shell Scripting - Complete Guide]].
> هنا ==خريطة وصول سريعة== + المثال العملي الكامل فقط — لتجنّب تكرار نفس الشرح في ملفين.

### 12.1 خريطة الوصول السريع

- المتغيرات، أنواعها، متغيرات البيئة، `read` (و`-s` للباسورد) → [[Linux Shell Scripting - Complete Guide#3. Variables & User Input|3. Variables & User Input]]
- المعاملات: حسابية · مقارنة · منطقية · نصوص (`$(( ))` و`bc`) → [[Linux Shell Scripting - Complete Guide#4. Operators in Shell Scripting|4. Operators]]
- الشروط `if` / `elif` / nested + فحص الـ root (`$EUID`) → [[Linux Shell Scripting - Complete Guide#5. Conditional Statements in Shell Scripting|5. Conditional Statements]]
- `case` والقوائم التفاعلية (`select`) → [[Linux Shell Scripting - Complete Guide#6. Case Statements in Shell Scripting|6. Case Statements]]
- الحلقات `for` / `while` / `until` + قراءة ملف سطرًا سطرًا → [[Linux Shell Scripting - Complete Guide#7. Loops in Shell Scripting|7. Loops]]
- `break` و`continue` (والمصائد مع `set -e`) → [[Linux Shell Scripting - Complete Guide#8. Loop Control Statements|8. Loop Control Statements]]
- الدوال، `local`، `return` → [[Linux Shell Scripting - Complete Guide#9. Functions in Shell Scripting|9. Functions]]
- المصفوفات ومعالجة النصوص (`${str,,}` / `${str//old/new}`) → [[Linux Shell Scripting - Complete Guide#10. Arrays & String Manipulation in Shell Scripting|10. Arrays & Strings]]
- وسائط سطر الأوامر `$0` `$1` `$#` `$@` `$*` و`shift` → [[Linux Shell Scripting - Complete Guide#11. Command-Line Arguments in Shell Scripting|11. Command-Line Arguments]]
- اختبارات الملفات `-f` / `-d` / `-s` داخل السكريبتات → [[Linux Shell Scripting - Complete Guide#12. File & Directory Operations in Shell Scripting|12. File & Directory Operations]]
- `grep` / `awk` / `sed` / `cut` / `sort` / `uniq` في التحليل → [[Linux Shell Scripting - Complete Guide#13. Text Processing Commands in Shell Scripting|13. Text Processing]]
- `$?` ومعالجة الأخطاء (`set -e` / `-u` / `pipefail` / `trap`) → [[Linux Shell Scripting - Complete Guide#14. Exit Status & Error Handling in Shell Scripting|14. Exit Status & Error Handling]]
- جدولة السكريبتات (cron) وسكريبتات المراقبة والنسخ الاحتياطي → [[Linux Shell Scripting - Complete Guide#15. Cron Jobs & Task Scheduling in Shell Scripting|15. Cron Jobs]] · [[Linux Shell Scripting - Complete Guide#16. System Monitoring Scripts in Shell Scripting|16. Monitoring]] · [[Linux Shell Scripting - Complete Guide#17. Log Management & Backup Automation in Shell Scripting|17. Backup]]
- `source` / `getopts` / here-docs و`PATH` للسكريبتات → [[Linux Shell Scripting - Complete Guide#19. Advanced Shell Scripting|19. Advanced Shell Scripting]]
- مشروع كامل: أداة إدارة المستخدمين → [[Linux Shell Scripting - Complete Guide#20. Hands-on Project — User Management Utility|20. User Management Project]]

> [!note] سلوك الـ shell التفاعلي
> الاقتباسات (`"..."` مقابل `'...'`)، command substitution، والـ exit status من الطرفية: [[#6. Shell Environment & PATH]]

### 12.2 المثال العملي الكامل — `myscript.sh`

Take user input, show date, and list files in current directory.

```bash
#!/bin/bash
echo "Enter your name: "
read name
echo "Hello $name!"
echo "Today's date and time: "
date
echo "Files in current directory: "
ls -l
echo "Script execution completed."
exit 0
```

Run the script:

```bash
chmod +x myscript.sh
bash myscript.sh   # run explicitly with bash (no +x needed)
./myscript.sh
```

> [!example] Output (example)
> ```text
> Enter your name: Rakesh
> Hello Rakesh!
> Today's date and time:
> Fri Oct 30 21:45:00 IST 2026
> Files in current directory:
> total 8
> -rw-r--r-- 1 user user 0 file1.txt
> -rw-r--r-- 1 user user 0 file2.txt
> Script execution completed.
> ```

> [!tip] Remember
> Give execute permission: `chmod +x script.sh`
> Run script: `./script.sh`
> Use `#` for comments.
> Use meaningful variable and function names.

> [!tip] Tip
> Start small, test often.
> Break big tasks into small scripts.
> Use variables, loops and functions to automate real work.

## 13. Linux in Real Projects

Linux is the ==backbone== of modern IT infrastructure. It powers ==tools, servers, pipelines and platforms==.

### Where is Linux Used?

#### 1) Software Development

- Preferred OS for developers.
- Supports Git, Vim, GCC, Make, Node.js, Python, Java etc.
- Easy package management and environment setup.
- Lightweight, stable and highly customizable.

#### 2) Cloud & DevOps

- Most Linux servers run on AWS, Azure and GCP.
- Used in CI/CD, monitoring, logging and automation.
- Core for Docker, Kubernetes, Terraform, Ansible.
- Used for building and managing cloud infrastructure.

#### 3) Cyber Security

- Preferred by security professionals.
- Tools like Kali Linux, Parrot OS.
- Used for penetration testing, vulnerability assessment and digital forensics.
- Tools: Nmap, Wireshark, Metasploit, Burp Suite.

#### 4) Data Engineering

- Linux handles big data workloads.
- Runs Hadoop, Spark, Kafka, Airflow, Hive etc.
- Stable, secure and high performance for large datasets.
- Widely used in data pipelines and analytics platforms.

#### 5) AI / Machine Learning

- Linux is the go-to OS for AI/ML engineers.
- Supports Python, TensorFlow, PyTorch, CUDA etc.
- Works seamlessly on GPUs and high performance clusters.
- Used for model training, experimentation and deployment.

#### 6) Web Hosting

- Most web servers run on Linux.
- Hosts Nginx, Apache, MySQL, PHP, Node.js applications.
- Reliable, lightweight, secure and cost-effective.
- Used for websites, web apps and APIs.

### Real Project Workflow

```text
Developer → Git → Linux Server → Docker → Jenkins → Kubernetes → Cloud
```

- **Developer** – writes code and tests locally on Linux.
- **Git** – code is committed and pushed to repository.
- **Linux Server** – code is pulled on Linux server for build and execution.
- **Docker** – application is containerized using Docker.
- **Jenkins** – builds, tests and creates artifacts.
- **Kubernetes** – deploys and manages containers at scale.
- **Cloud** – application runs on cloud (AWS / Azure / GCP).

### Why Linux?

- Open Source and Free.
- Secure and Stable.
- Powerful Command Line.
- Automates Repetitive Tasks.
- Runs Everywhere (On-prem, Cloud, Edge).
- Huge Community Support.

> [!tip] Tip
> Learn Linux well, and you can work in ==any tech role== with confidence!

> [!tip] Remember
> Linux is not just an OS, it's the ==foundation of modern IT==.
> From coding to deploying, Linux is ==everywhere==!
> It helps in building reliable, scalable and secure systems.
> Master Linux = Career Booster.

> [!quote]
> "Behind every great project, there is a powerful Linux system running silently."

## 14. Most Asked Linux Interview Questions

| #   | Question                          | Concise Answer                                                                                                                                                                                                                                                                                                    |
| --- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | What is Linux?                    | Linux is an ==open-source== operating system based on the Linux kernel. It is stable, ==secure== and widely used in servers, cloud, desktops and embedded devices.                                                                                                                                                |
| 2   | Kernel vs Shell?                  | • **Kernel** is the ==core== of the OS. It manages hardware, memory, processes and system resources.<br>• **Shell** is a command-line interface (CLI) that allows users to interact with the kernel.                                                                                                              |
| 3   | Soft Link vs Hard Link?           | • **Soft Link (Symbolic Link):** points to the path of original file; can point across different filesystems. If original file is deleted, link breaks. (`ln -s`)<br>• **Hard Link:** points to the inode of original file; must be on the same filesystem. If original file is deleted, link still works. (`ln`) |
| 4   | chmod vs chown?                   | • `chmod`: changes file/directory permissions. Example: `chmod 755 file.txt`<br>• `chown`: changes ownership of a file/directory. Example: `chown user:group file.txt`                                                                                                                                            |
| 5   | grep vs find?                     | • `grep`: searches for a pattern inside file content. Example: `grep "error" log.txt`<br>• `find`: searches for files/directories by name, type, size, time, etc. Example: `find /var -name "*.log"`                                                                                                              |
| 6   | Process vs Thread?                | • **Process** is an independent program with its own memory space.<br>• **Thread** is the smallest unit of execution within a process and shares the same memory space with other threads.                                                                                                                        |
| 7   | Zombie Process?                   | A terminated process whose parent has not yet read its exit status. It holds a PID entry in the process table but does not consume CPU or memory resources.                                                                                                                                                       |
| 8   | Cron Job?                         | Cron is a ==time-based job scheduler== in Linux. It runs commands or shell scripts automatically at specified time intervals. Edit cron using: `crontab -e`                                                                                                                                                       |
| 9   | Environment Variables?            | Variables that store system or user specific values used by processes and shell. Examples: `HOME`, `PATH`, `USER`, `SHELL`. Use: `echo $PATH` or `export VAR=value`                                                                                                                                               |
| 10  | Pipe ( \| ) vs Redirection ( > )? | • **Pipe ( \| ):** sends output of one command as input to another. Example: `ls -l \| grep "txt"`<br>• **Redirection ( > ):** sends output of a command to a file. Example: `ls -l > out.txt`                                                                                                                    |
| 11  | Linux vs Unix?                    | • **Unix:** proprietary, older, and paid OS (e.g. AIX, Solaris).<br>• **Linux:** ==open-source==, free, Unix-like OS widely used across servers and cloud (e.g. Ubuntu, RHEL, CentOS).                                                                                                                            |
| 12  | `>` vs `>>` & `2>` vs `2>&1`?     | • `>` overwrites file; `>>` appends to file.<br>• `2>` redirects `stderr`; `2>&1` merges `stderr` into `stdout`.                                                                                                                                                                                                  |
| 13  | `&&` vs `;` vs `\|\|`?            | • `;` runs next command regardless of exit code.<br>• `&&` runs next only if previous succeeded (`$?=0`).<br>• `\|\|` runs next only if previous failed (`$?!=0`).                                                                                                                                                |
| 14  | `grep -c` vs `grep -o \| wc -l`?  | • `grep -c` counts ==matching lines==.<br>• `grep -o \| wc -l` counts ==every matching occurrence== (even multiple per line).                                                                                                                                                                                     |

> [!tip] Remember
> Practice commands daily.
> Understand concepts, not just syntax.
> ==Hands-on is the key== to master Linux.
> Use `man` pages (e.g. `man ls`) for detailed info.

> [!tip] Tip
> Use `man command` to read the manual.
> Try commands in a Linux VM or cloud server.
> Practice with real-world scenarios.
> Make short notes and revise regularly.
> Combine commands using pipes and redirection.

> [!quote]
> "The more you practice in Linux, the more powerful you become!"

> [!see-also] أسئلة الإدارة والجدولة
> أسئلة المقابلات الخاصة بـ at/cron/anacron/timers/ACLs/nice/tuned: [[Linux System Administration#15. Interview Questions]]

## 15. Cheat Sheet & Roadmap

### Top 50 Linux Commands

| #   | Command                | Purpose                                 |
| --- | ---------------------- | --------------------------------------- |
| 1   | pwd                    | Print working directory                 |
| 2   | ls                     | List files and directories              |
| 3   | cd                     | Change directory                        |
| 4   | mkdir                  | Create a directory                      |
| 5   | rmdir                  | Remove an empty directory               |
| 6   | touch                  | Create an empty file                    |
| 7   | cp                     | Copy files or directories               |
| 8   | mv                     | Move or rename files                    |
| 9   | rm                     | Remove files or directories             |
| 10  | cat                    | Display file contents                   |
| 11  | less                   | View file content (scrollable)          |
| 12  | head                   | Show first lines of a file              |
| 13  | tail                   | Show last lines of a file               |
| 14  | nano                   | Edit files in terminal                  |
| 15  | vi                     | Edit files (advanced)                   |
| 16  | chmod                  | Change file permissions                 |
| 17  | chown                  | Change file owner                       |
| 18  | ps                     | Show running processes                  |
| 19  | top                    | Monitor system processes                |
| 20  | kill                   | Terminate a process                     |
| 21  | df                     | Disk space usage                        |
| 22  | du                     | Directory space usage                   |
| 23  | free                   | Show memory usage                       |
| 24  | uname                  | System information                      |
| 25  | hostname / hostnamectl | Show or set hostname and system details |
| 26  | whoami                 | Show current user                       |
| 27  | id                     | Show user and group IDs                 |
| 28  | grep                   | Search text in files                    |
| 29  | find                   | Search files and directories            |
| 30  | locate                 | Find files by name (updatedb)           |
| 31  | which                  | Locate command path                     |
| 32  | echo                   | Display text                            |
| 33  | history                | Show command history                    |
| 34  | man                    | Show manual of a command                |
| 35  | whatis                 | Brief description of command            |
| 36  | alias / unalias        | Create or remove command shortcuts      |
| 37  | tar                    | Archive files                           |
| 38  | gzip                   | Compress files                          |
| 39  | gunzip                 | Decompress .gz files                    |
| 40  | zip                    | Zip files                               |
| 41  | unzip                  | Unzip files                             |
| 42  | scp                    | Secure copy (remote files)              |
| 43  | ssh                    | Secure shell login                      |
| 44  | ping                   | Check connectivity                      |
| 45  | netstat                | Network connections                     |
| 46  | ss                     | Socket statistics                       |
| 47  | curl                   | Transfer data from URL                  |
| 48  | wget                   | Download files from web                 |
| 49  | crontab                | Schedule tasks (cron jobs)              |
| 50  | clear                  | Clear the terminal screen               |

### Linux Learning Roadmap

1. **Basics** – Linux introduction, file system, navigation, basic commands
2. **Commands** – file operations, text processing, permissions, users & groups
3. **System Admin** – processes, services, logs & monitoring, scheduling (cron)
4. **Networking** – IP, DNS, SSH, networking tools, firewall basics, troubleshooting
5. **DevOps Tools** – Git, Docker, Jenkins, CI/CD basics
6. **Orchestration** – Kubernetes, YAML basics, deploy apps, scaling & HA
7. **Cloud** – AWS / Azure / GCP, Linux on cloud, automation, real projects

> [!tip] Remember
> Practice commands daily.
> Use `man <command>` for help.
> Learn by doing real tasks.
> Small steps, big progress!

> [!tip] Tips
> Start with basic commands.
> Be consistent and practice.
> Make your own cheat sheet.
> Try real projects and scenarios.
> Use a Linux VM or cloud server.

> [!success] Best Practices
> Use documentation.
> Automate repetitive tasks.
> Keep system updated.
> Take regular backups.
> Follow security best practices.
> Practice troubleshooting.

> [!quote]
> "Linux is not just an OS, it's a skill that powers your ==DevOps== journey!"

## 16. User & Group Management

These commands help us ==create, modify and delete== users and groups, and control ==access== to the system.

| #   | Command         | Description                       | Example                            | What it does                                               |
| --- | --------------- | --------------------------------- | ---------------------------------- | ---------------------------------------------------------- |
| 1   | useradd         | Create a new user.                | `sudo useradd -m -s /bin/bash ali` | Creates user with home directory and shell.                |
| 2   | usermod         | Modify an existing user.          | `sudo usermod -aG sudo ali`        | Adds user to a group (e.g. sudo).                          |
| 3   | userdel         | Delete a user.                    | `sudo userdel -r ali`              | Removes user and home directory.                           |
| 4   | passwd          | Set/change password.              | `sudo passwd ali`                  | Prompts for a new password.                                |
| 5   | chage           | Change password expiry info.      | `chage -l ali`                     | Shows password aging details.                              |
| 6   | groupadd        | Create a new group.               | `sudo groupadd devs`               | Creates group "devs".                                      |
| 7   | groupdel        | Delete a group.                   | `sudo groupdel devs`               | Removes the group.                                         |
| 8   | groups          | Show groups of a user.            | `groups ali`                       | Lists group memberships.                                   |
| 9   | id              | Show UID, GID and groups.         | `id ali`                           | Displays user and group IDs.                               |
| 10  | w / who / users | Show logged-in users.             | `w` / `users`                      | Shows active users (`users` prints usernames on one line). |
| 11  | last            | Show login history.               | `last -n 10`                       | Shows last 10 login sessions.                              |
| 12  | su              | Switch user.                      | `su - ali`                         | Switches to another user's shell.                          |
| 13  | sudo            | Run command with elevated rights. | `sudo apt update`                  | Executes as root (per sudoers).                            |
### Key Account Files

- `/etc/passwd` – user account info (name, UID, GID, home, shell).
- `/etc/shadow` – ==encrypted passwords== and aging info (root only).
- `/etc/group` – group definitions and memberships.
- `/etc/sudoers` – sudo privileges (edit with ==`visudo`== only).
- `/etc/gshadow` – stores ==encrypted group passwords== and group admins (optional, root only).

### Practical Examples

```bash

# Create user with home dir and bash shell
sudo useradd -m -s /bin/bash rakesh
sudo passwd rakesh

# Add user to sudo (admin) group
sudo usermod -aG sudo rakesh

# Create group and add members
sudo groupadd developers
sudo usermod -aG developers rakesh

# Check membership
id rakesh
groups rakesh

# Lock / unlock account
sudo passwd -l rakesh   # lock
sudo passwd -u rakesh   # unlock

# Delete user with home
sudo userdel -r rakesh

sudo gpasswd -d rakesh developers   # Remove user from a group

```

> [!warning] Important
> Always use ==`-aG`== with `usermod` (append to group). Without `-a` the user is ==removed from all other groups==.
> Never edit `/etc/sudoers` directly — use ==`visudo`== to avoid locking yourself out.
> `userdel -r` deletes the home directory ==permanently==.

> [!tip] Remember
> UID 0 = ==root==. UIDs 1–999 are usually system accounts. Regular users start from 1000.
> Every user has one ==primary group== and optional supplementary groups.
> `sudo` logs every command it runs — good for auditing.
> On Debian/Ubuntu, `adduser` and `deluser` are user-friendly interactive wrappers around `useradd` and `userdel`.

> [!see-also] تطبيق عملي
> مشروع كامل يستخدم أوامر المستخدمين (مع تصحيح أخطاء `useradd -p` و`chpasswd`): [[Linux Shell Scripting - Complete Guide#20. Hands-on Project — User Management Utility]]
> إدارة مستخدمين مركزية بـ FreeIPA: [[Linux System Administration#17. FreeIPA (Identity Management)]]

## 17. Permissions & Ownership

Linux controls access with ==read (r), write (w), execute (x)== permissions for ==owner, group and others==.

### Permission Values

| Symbol | Meaning | Value |
| --- | --- | --- |
| r | Read | 4 |
| w | Write | 2 |
| x | Execute | 1 |

### Reading `ls -l` Output

```text
-rwxr-xr--  1 rakesh devs  1024 Nov 4 10:00 script.sh
│└─┬─┘└─┬─┘└─┬─┘
│  │    │    └─ others: r-- (read only)
│  │    └────── group:  r-x (read + execute)
│  └─────────── owner:  rwx (full control)
└────────────── file type (- file, d dir, l link)
```

### chmod — Change Permissions

```bash
# Numeric (octal) mode
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 600 secret.key   # rw-------  (owner only)
chmod -R 755 folder/   # recursive

# Symbolic mode
chmod u+x script.sh    # add execute for owner
chmod go-w file.txt    # remove write from group & others
chmod a+r file.txt     # read for everyone

chmod o=r file.txt     # set exact permission (read-only) for others

```

### Common Octal Values

| Octal | Meaning | Typical use |
| --- | --- | --- |
| 777 | Everyone full | ==Dangerous — avoid== |
| 755 | Owner full, others read+execute | Scripts, directories |
| 644 | Owner rw, others read | Regular files |
| 600 | Owner only | Private files, SSH keys |
| 400 | Owner read-only | Certificates |

### chown & chgrp — Ownership

```bash
chown rakesh file.txt          # change owner
chown rakesh:devs file.txt     # change owner + group
chown :devs file.txt           # change group only
chown -R www-data:www-data /var/www   # recursive
chgrp devs file.txt            # change group only
```

### Special Permissions: SUID, SGID, Sticky Bit

- ==SUID (4)==: run the file with the ==owner's== privileges (e.g. `passwd`).
- ==SGID (2)==: run with group privileges; on directories, new files inherit the directory's group.
- ==Sticky Bit (1)==: on shared dirs, only the file owner can delete it (e.g. `/tmp`).

```bash
chmod 4755 mybin     # SUID
chmod 2775 shared/   # SGID
chmod 1777 /tmp      # sticky bit

ls -l /usr/bin/passwd   # -rwsr-xr-x  (s = SUID)
ls -ld /tmp             # drwxrwxrwt  (t = sticky)
```

### File Attributes (`lsattr` & `chattr`)

```bash
lsattr file.txt         # Show extended file attributes
sudo chattr +i file.txt # Make file immutable (cannot be modified or deleted even by root)
sudo chattr -i file.txt # Remove immutable attribute
```

### umask — الصلاحيات الافتراضية

```bash
umask                      # shows default mask (e.g. 022)
umask 027                  # new files: 640, dirs: 750
```

> [!see-also] ACLs — الصلاحيات الموسّعة
> لما تحتاج تمنح صلاحيات لـ ==أكثر من مستخدم أو جروب مسمّى== (بدل نموذج u/g/o الواحد):
> [[Linux System Administration#11. Access Control Lists (ACLs)]] — `getfacl` · `setfacl -m` · `setfacl -x` · الـ mask · default ACLs · النسخ والاستعادة.

> [!tip] Remember
> Directories need ==x== to enter (`cd`), ==r== to list (`ls`), ==w== to create/delete inside.
> Files need ==x== to run as a program or script.
> ==777 is almost never acceptable== — it's a security risk.
> SSH private keys must be ==600== or SSH refuses to use them.

> [!warning] Watch out
> A wrong recursive `chmod`/`chown` on system directories can ==break the whole system==. Always test on a copy first.
> على ملف فيه ACL، أمر `chmod g-w` بيعدّل الـ ==mask== مش الجروب المالك — التفاصيل في ملف الإدارة.

## 18. Systemd & Service Management

==systemd== is the init system and service manager of modern Linux: it boots the system and manages services, mounts, timers and logs.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | systemctl start | Start a service now. | `sudo systemctl start nginx` | Runs the service immediately. |
| 2 | systemctl stop | Stop a running service. | `sudo systemctl stop nginx` | Halts the service. |
| 3 | systemctl restart | Restart a service. | `sudo systemctl restart nginx` | Full stop + start. |
| 4 | systemctl reload | Reload config without stopping. | `sudo systemctl reload nginx` | Applies new config gracefully. |
| 5 | systemctl enable | Start service at boot. | `sudo systemctl enable nginx` | Creates boot symlink. |
| 6 | systemctl disable | Don't start at boot. | `sudo systemctl disable nginx` | Removes boot symlink. |
| 7 | systemctl status | Show service state. | `systemctl status nginx` | Active state, PID, recent logs. |
| 8 | systemctl list-units | List loaded units. | `systemctl list-units --type=service` | Shows running services. |
| 9 | systemctl list-unit-files | List unit files + boot state. | `systemctl list-unit-files --type=service` | Shows enabled/disabled. |
| 10 | journalctl | View systemd logs. | `journalctl -u nginx -f` | Follow logs of a service. |
| 11 | systemd-analyze | Analyze boot time. | `systemd-analyze blame` | Shows slowest units at boot. |
| 12 | systemctl list-timers | List scheduled timers. | `systemctl list-timers` | Modern cron alternative. |

### Query Subcommands — أوامر الاستعلام

| Command                              | Function                                                       |
| ------------------------------------ | -------------------------------------------------------------- |
| `systemctl is-active <unit>`         | هل الخدمة شغالة دلوقتي؟                                        |
| `systemctl is-enabled <unit>`        | هل بتبدأ تلقائي مع الإقلاع؟                                    |
| `systemctl is-failed <unit>`         | هل فشلت؟                                                       |
| `systemctl isolate <target>`         | التحويل لـ target معين                                         |
| `systemctl is-system-running`        | الحالة العامة للنظام                                           |
| `systemctl is-active --quiet <unit>` | فحص صامت لحالة الخدمة (يُرجع exit code فقط — مثالي للسكريبتات) |

```bash
[root@control ~]# systemctl is-active sshd
active
[root@control ~]# systemctl is-enabled sshd
enabled
```

### Most Used Workflows

```bash
# Check if a service is running & enabled
systemctl status nginx
systemctl is-active nginx     # active / inactive
systemctl is-enabled nginx    # enabled / disabled

# Typical fix after editing a config file
sudo nginx -t                  # test config syntax (service-specific)
sudo systemctl reload nginx    # or restart if reload unsupported

# See why a service failed
systemctl status nginx -l
journalctl -u nginx -n 50 --no-pager
journalctl -u nginx --since today

# Boot troubleshooting
journalctl -b                  # logs of current boot
journalctl -b -1               # previous boot
journalctl -p err -b           # only errors of this boot
```

### Custom Service Unit File

```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
After=network.target

[Service]
ExecStart=/opt/myapp/run.sh
Restart=always
User=myapp

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload        # after editing unit files
sudo systemctl enable --now myapp   # enable + start in one step
```

> [!tip] Remember
> ==`reload` vs `restart`==: reload keeps connections alive; restart drops them.
> `enable` = at boot, `start` = now. Usually you want ==`enable --now`==.
> Always run ==`daemon-reload`== after editing unit files.
> `journalctl -f` is the modern `tail -f` for service logs.

> [!see-also] ذات صلة
> جدولة حديثة بـ systemd Timers (بديل cron مع `Persistent=true`): [[Linux System Administration#5. Modern Scheduling with systemd Timers]]
> سكريبتات فحص الخدمات وwatchdog (ومنها Toggle psacct): [[Linux Shell Scripting - Complete Guide#21. Quick One-Liners & Snippets]]

## 19. Archive & Compression

Archiving bundles files together; compression shrinks them. ==tar== is the standard tool, combined with gzip / bzip2 / xz.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | tar -czf | Create gzip archive. | `tar -cvzf backup.tar.gz folder/` | Creates a compressed archive. |
| 2 | tar -x | Extract archive. | `tar -xvzf backup.tar.gz` | Extracts to current directory. |
| 3 | tar -t | List archive contents. | `tar -tvf backup.tar.gz` | Shows files inside without extracting. |
| 4 | gzip | Compress a single file. | `gzip file.txt` | Creates `file.txt.gz`. |
| 5 | gunzip | Decompress .gz. | `gunzip file.txt.gz` | Restores the original file. |
| 6 | bzip2 | Stronger compression. | `bzip2 file.txt` | Creates `file.txt.bz2`. |
| 7 | xz | Best compression ratio. | `xz file.txt` | Creates `file.txt.xz`. |
| 8 | zip / unzip | Zip archive (cross-platform). | `zip -r archive.zip folder/` | Zips folder recursively. |
| 9 | zcat | View .gz without extracting. | `zcat file.gz` | Prints decompressed content. |

### tar Flags Explained

- `-c` create · `-x` extract · `-t` list
- `-v` verbose · `-f` filename (==must be the last flag==)
- `-z` gzip · `-j` bzip2 · `-J` xz
- `-C` extract into a directory

```bash
# Create
tar -cvzf project.tar.gz project/
tar -cvjf project.tar.bz2 project/   # smaller, slower
tar -cvJf project.tar.xz project/    # smallest, slowest

# List & extract
tar -tvf project.tar.gz              # list contents
tar -xvzf project.tar.gz             # extract here
tar -xvzf project.tar.gz -C /opt     # extract to /opt
tar -xvzf project.tar.gz project/config.yml   # single file

# Verify integrity
gzip -t project.tar.gz && echo OK
```

### Compression Comparison

| Tool | Extension | Speed | Ratio | Best for |
| --- | --- | --- | --- | --- |
| gzip | .gz | Fast | Good | Default choice |
| bzip2 | .bz2 | Slower | Better | Legacy systems |
| xz | .xz | Slowest | Best | Software distribution |
| zip | .zip | Fast | Good | Sharing with Windows |

> [!tip] Remember
> ==tar archives, gzip compresses== — `.tar.gz` = archive + compression together.
> `-f` must come last because it consumes the filename argument.
> List before extracting (`tar -tvf`) to avoid ==absolute paths / tar bombs==.
> Use `zip` for Windows users, `tar.gz` for Linux backups.

> [!see-also] أتمتة النسخ الاحتياطي
> سكريبتات tar بالتاريخ + retention + النقل للسيرفر البعيد: [[Linux Shell Scripting - Complete Guide#17. Log Management & Backup Automation in Shell Scripting]]

## 20. Troubleshooting & System Logs

Troubleshooting = ==collect evidence== from logs, processes and network, then isolate the failing layer.

### Key Log Files

| Path                                                                   | Contains                                           |
| ---------------------------------------------------------------------- | -------------------------------------------------- |
| `/var/log/syslog` (or `messages`)                                      | General system activity                            |
| `/var/log/auth.log` (or `secure`)                                      | Logins, sudo, SSH attempts                         |
| `/var/log/kern.log`                                                    | Kernel messages                                    |
| `dmesg` / `/var/log/dmesg`                                             | Boot & hardware messages                           |
| `/var/log/boot.log`                                                    | Boot process output                                |
| `/var/log/cron`                                                        | Cron job execution                                 |
| `/var/log/nginx/`, `/var/log/apache2/`                                 | Web server logs                                    |
| `/var/log/nginx/`, `/var/log/apache2/`, `/var/log/httpd/`              | Web server logs (Nginx / Apache)                   |
| `/var/log/dpkg.log`                                                    | Package installation & update logs (Debian/Ubuntu) |

### Essential Commands

```bash
# Live monitoring
tail -f /var/log/syslog
tail -n 100 /var/log/auth.log

# Search for problems
grep -i "error" /var/log/syslog
grep "Failed password" /var/log/auth.log    # SSH brute-force check
dmesg | grep -i error                       # hardware/kernel errors
dmesg | tail -n 20                          # recent kernel events

# The systemd way
journalctl -xe                   # latest errors with context
journalctl -p err --since today  # errors only, today
journalctl --disk-usage          # how much space logs use
sudo journalctl --vacuum-time=7d # delete logs older than 7 days

journalctl --since "1 hour ago"                  # logs from the last hour
find /var/log -type f -name "*.log" -mtime +7 -delete  # delete .log files older than 7 days

# Who & when
last -n 20      # recent logins
lastb           # failed login attempts
w               # currently logged in + activity

sudo logrotate -f /etc/logrotate.conf   # force immediate log rotation
```

### Layer-by-Layer Troubleshooting Method

1. ==Define the symptom== – what exactly fails? For whom? Since when?
2. ==Check the basics== – power, cables, `ping`, is the disk full? (`df -h`)
3. ==Read the logs== – `journalctl` / `/var/log` around the failure time
4. ==Check resources== – `free -h`, `top`/`htop`, `df -i`
5. ==Check services== – `systemctl status`, listening ports (`ss -tulnp`)
6. ==Check network== – `ping`, `traceroute`, `dig`, firewall (`ufw status`)
7. ==Check permissions== – `ls -l`, ownership, SELinux/AppArmor
8. ==Reproduce & isolate== – change ONE thing at a time, test after each change

### Quick Diagnostic One-Liners

```bash
df -h && free -h && uptime         # disk + memory + load at once
ss -tulnp | grep LISTEN            # what is listening?
ps aux --sort=-%mem | head -n 6    # top memory consumers
ps aux --sort=-%cpu | head -n 6    # top CPU consumers
lsof +D /var/www                   # which process uses this dir?
fuser -v /dev/sdb1                 # who is using this device?

strace -p <PID>                    # trace live system calls of a running process
strace -e trace=file <command>     # see which files a command tries to open/access

```

> [!warning] Common real-world causes
> ==Disk 100% full== → services crash, logs stop (`df -h`, `df -i`)
> ==Wrong permissions/ownership== after manual edits (`ls -l`)
> ==Service not enabled== → "worked yesterday, dead after reboot"
> ==Firewall rule== blocking a port (`ufw status`, `ss -tulnp`)
> ==DNS misconfiguration== → "internet works but names fail"

> [!tip] Remember
> Logs carry ==timestamps== — always correlate the failure time with log entries.
> Change ==one variable at a time== and document every change.
> Stuck? Search the ==exact error message== from the logs.
> Practice the loop: symptom → logs → isolate → fix → verify → document.

> [!quote]
> "Logs don't lie — learn to read them."

> [!see-also] ذات صلة
> أداة `lsof` كاملة (مين ماسك بورت/ملف/الديسك): [[Linux System Administration#lsof — List Open Files]]
> جدول "المهمة لم تشتغل" (at/cron/anacron/timers/ACL/logrotate): [[Linux System Administration#14. Troubleshooting & Cheat Sheet]]
> تدوير اللوجات بدل حذفها اليدوي: [[Linux System Administration#6. Log Rotation with logrotate]]
> لقطات أمنية وسكريبتات مراقبة: [[Linux Shell Scripting - Complete Guide#21. Quick One-Liners & Snippets]]

---
## 21. Security & Firewall

Linux security focuses on ==access control, firewalls, SSH hardening, updates, and monitoring== to protect systems and data.

### Linux Firewall (`UFW` vs `firewalld`)

- **UFW (Ubuntu / Debian):** simple and easy-to-use default firewall.
- **firewalld (RHEL / CentOS / Fedora):** dynamic firewall supporting zones and services.

| Task | UFW (Ubuntu / Debian) | firewalld (RHEL / CentOS) |
| --- | --- | --- |
| Check status | `sudo ufw status` | `sudo firewall-cmd --state` |
| Enable firewall | `sudo ufw enable` | `sudo systemctl enable --now firewalld` |
| Allow port 22 (SSH) | `sudo ufw allow 22` | `sudo firewall-cmd --add-port=22/tcp --permanent` |
| Deny / Block port 80 | `sudo ufw deny 80` | `sudo firewall-cmd --remove-port=80/tcp --permanent` |
| Remove rule / Apply | `sudo ufw delete allow 22` | `sudo firewall-cmd --reload` |
| List all rules | `sudo ufw status numbered` | `sudo firewall-cmd --list-all` |

### System Hardening & Security Tools

```bash
# Keep system updated & remove unused packages/services
sudo apt update && sudo apt upgrade
sudo apt purge <package>
sudo systemctl disable <unused-service>

# Security scanning & brute-force protection
sudo fail2ban-client status    # Check fail2ban jails & banned IPs
sudo chkrootkit                # Scan system for known rootkits
```

> [!tip] SSH Hardening Checklist
> - Use ==key-based authentication== (SSH keys) instead of passwords.
> - Disable direct root login (`PermitRootLogin no` in `/etc/ssh/sshd_config`).
> - Change the default SSH port (e.g. `2222`) if appropriate for your environment.
> - Use ==`fail2ban`== to limit failed login attempts and block brute-force attacks.
> - Regularly review `/var/log/auth.log` (or `/var/log/secure`) and `/var/log/syslog`.

> [!note] Legacy Firewall (`iptables`)
> على الأنظمة الأقدم أو عند الحاجة لقواعد شبكية منخفضة المستوى، يُستخدم ==`iptables`== (مثل `sudo iptables -L -n -v` لعرض القواعد الحالية)، بينما تعتمد التوزيعات الحديثة على `ufw` أو `firewalld` (فوق `nftables`).


## Related Notes

- [[Linux Shell Scripting - Complete Guide]] — ==المرجع الكامل== للسكريبتات والأتمتة (المتغيرات · الشروط · الحلقات · الدوال · المشاريع)
-   [[Linux Shell Scripting - Complete Guide#12. File & Directory Operations in Shell Scripting]] — اختبارات الملفات داخل السكريبتات
-   [[Linux Shell Scripting - Complete Guide#20. Hands-on Project — User Management Utility]] — مشروع يطبّق أوامر [[#16. User & Group Management]]
- [[Linux System Administration]] — ==الإدارة==: الجدولة · الأداء · ACLs · الشبكات · FreeIPA · التخزين
-   [[Linux System Administration#11. Access Control Lists (ACLs)]] — التوسع بعد [[#17. Permissions & Ownership]]
- [[Linux System Administration#5. Modern Scheduling with systemd Timers]] — التوسع بعد [[#18. Systemd & Service Management]]
- [[Linux System Administration#8. Performance History with sysstat]] — التوسع بعد [[#9. Disk & Memory Commands]]