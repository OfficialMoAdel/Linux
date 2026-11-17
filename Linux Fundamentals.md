---
title: Linux Fundamentals
created: 2026-11-04
tags:
  - linux
  - cli
  - devops
  - study-notes
aliases:
  - Linux Notes
  - Introduction to Linux
---

# Linux Fundamentals

> [!info] Note status
> This note is built in parts. It currently covers sections 1–5 extracted from the hand-written infographic series (Rakesh Kumar). ==More sections will be appended as the remaining pages arrive.==
## Contents

- [[#1. Introduction to Linux]]
- [[#2. Linux Architecture]]
- [[#3. Linux File System]]
- [[#4. Navigation Commands]]
- [[#5. File & Directory Commands]]
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
> - Linux is not just an OS, ==it's an ecosystem==.
> - ==Most of the internet runs on Linux!==

## 2. Linux Architecture

Linux is built in layers. Each layer has one job, and they stack from the user down to the hardware.

### The Five Layers
1. **User Space** – contains all user applications, programs and commands. *Examples:* text editors, browsers, compilers, etc.
2. **Shell** – acts as an interface between the user and the kernel. It takes commands from the user and passes them to the kernel. *Examples:* bash, zsh, sh, etc.
3. **Kernel** – ==the heart of Linux==. It manages system resources, handles system calls and acts as a bridge between software and hardware. *Examples:* process management, memory management, device drivers, etc.
4. **File System** – organizes and manages data on storage devices. It handles files, directories, permissions and disk space. *Examples:* ext4, xfs, btrfs, etc.
5. **Hardware** – physical components of the system like CPU, RAM, Disk, Network Cards, I/O Devices, etc. Kernel communicates with hardware through device drivers.

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
> - ==Everything in Linux is a file.==
> - ==Kernel is always in control.==
> - Without kernel, system cannot function.
> - Shell is optional, but very powerful.

## 3. Linux File System

==Everything in Linux is a file.== Files are organized in a ==hierarchical tree structure== starting from the root (`/`).

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

| Directory | Purpose |
| --- | --- |
| `/` (root) | The top of the directory tree. All files and directories start from here. |
| `/home` | Contains home directories for users. e.g. `/home/rakesh` |
| `/etc` | Stores system-wide configuration files. Important for system settings. |
| `/var` | Holds variable data like logs, spool files, cache, databases, mail, etc. |
| `/usr` | Contains most user programs, libraries, documentation and utilities. |
| `/opt` | Used to install optional third-party software. |
| `/tmp` | Stores temporary files. Files may be deleted on reboot. |
| `/bin` | Contains essential command binaries needed for system operation. |

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
> - ==Root (`/`)== is the base of the entire system.
> - Linux is ==case-sensitive==.
> - Every file or directory inherits permissions from its parent.
> - Use `"/"` in paths (not `\\` like Windows).

### Quick Commands
```bash
ls /       # List root directory
ls /etc    # List /etc directory
cd /home   # Go to /home
tree /     # Show directory tree (if installed)
```

> [!tip] Remember
> - Root (`/`) is the starting point of everything.
> - Keep system directories safe. Do not modify files unless you know what you are doing.
> - Use meaningful names for files & directories.

## 4. Navigation Commands

Use these to move around the file system and see what's inside directories.

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
> - Use ==Tab== key for auto-completion.
> - Use `cd ..` to go one level up.
> - Use `cd ~` to go to home directory.

## 5. File & Directory Commands

Commands for creating, copying, moving, and deleting files and directories.

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
> - Use `cp -r` to copy directories.
> - Use `rm -r` to delete directories.
> - Use ==Tab== key for auto-completion.
> - Use `man <command>` to get help.

## 7. Searching & Text Processing

Commands for finding files, searching text, and processing data.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `find` | Search for files and directories. | `find /home -name "file.txt"` | Finds `file.txt` in `/home`. |
| 2 | `locate` | Find files using pre-built database. | `locate file.txt` | Fast file search using index. |
| 3 | `grep` | Search for a pattern in files. | `grep "error" app.log` | Finds lines containing "error". |
| 4 | `awk` | Pattern scanning and processing language. | `awk '{print $1}' data.txt` | Prints first column from `data.txt`. |
| 5 | `sed` | Stream editor for filtering & transforming. | `sed 's/old/new/g' file.txt` | Replaces old with new in `file.txt`. |
| 6 | `sort` | Sort lines of text files. | `sort names.txt` | Sorts names in alphabetical order. |
| 7 | `uniq` | Remove duplicate lines. | `uniq list.txt` | Shows only unique lines. |
| 8 | `wc` | Count lines, words and characters. | `wc file.txt` | Counts lines, words and characters. |
| 9 | `head` | Display first few lines of a file. | `head -n 10 file.txt` | Shows first 10 lines. |
| 10 | `tail` | Display last few lines of a file. | `tail -n 10 file.txt` | Shows last 10 lines. |

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
> - `grep` is ==case-sensitive==.
> - `locate` is faster but may be outdated.
> - Use `|` (pipe) to combine commands.
> - Always read `man` page (`man command`) for options.
> - Use quotes (`" "`) for patterns with spaces.
> - These commands are very useful for ==log analysis and troubleshooting==.

## 8. Process Management

Commands for viewing and managing running processes.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `ps` | Report snapshot of current processes. | `ps aux` | Shows currently running processes. |
| 2 | `top` | Real-time view of processes. | `top` | Displays dynamic process information. |
| 3 | `htop` | Interactive process viewer (better than top). | `htop` | User-friendly real-time process monitor. |
| 4 | `jobs` | List background and stopped jobs. | `jobs` | Shows jobs started from current shell. |
| 5 | `bg` | Resume a stopped job in background. | `bg %1` | Continues job in background. |
| 6 | `fg` | Bring a background job to foreground. | `fg %1` | Brings job to current terminal. |
| 7 | `kill` | Terminate a process using PID. | `kill 1234` | Sends TERM signal (default). |
| 8 | `kill -9` | Force kill a process (SIGKILL). | `kill -9 1234` | Forcefully terminates unresponsive process. |
| 9 | `nice` | Run a process with modified priority. | `nice -n 10 command` | Adjusts process priority (lower value = higher priority). |

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
```

### Job States
- **Running (R)** – running in foreground.
- **Sleeping (S)** – running in background / waiting.
- **Stopped (T)** – stopped (use `Ctrl + Z`).
- **Zombie (Z)** – process completed but not removed.

```bash
$ jobs
[1]+  Running   sleep 100 &
[2]-  Stopped   vim file.txt
```

> [!tip] Remember
> - Use `ps aux` for detailed process information.
> - `top` is useful but `htop` is easier to use.
> - Use `Ctrl + Z` to stop a process and `bg` to run in background.
> - `kill` sends default ==TERM== signal (kind request).
> - ==`kill -9` should be used only when the process does not stop.==
> - `nice` starts a process with lower priority (higher number = lower priority).

## 9. Disk & Memory Commands

Commands for checking disk, memory, and system performance.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `df` | Disk free space report. | `df -h` | Shows disk space usage of file systems. |
| 2 | `du` | Estimate file and directory space usage. | `du -sh /home/user` | Shows size of files or directories. |
| 3 | `free` | Display memory usage. | `free -h` | Shows total, used and free memory. |
| 4 | `uptime` | Show how long system has been running. | `uptime` | Shows system uptime and load average. |
| 5 | `vmstat` | Report virtual memory statistics. | `vmstat 1 5` | Shows processes, memory, CPU and I/O stats. |
| 6 | `iostat` | I/O statistics for CPU and devices. | `iostat -xz 1 5` | Shows CPU and device I/O performance. |
| 7 | `mount` | Mount a file system. | `mount /dev/sdb1 /mnt` | Attaches a filesystem to a directory. |
| 8 | `umount` | Unmount a file system. | `umount /mnt` | Detaches a filesystem from a directory. |
| 9 | `lsblk` | List block devices. | `lsblk` | Shows information about block devices. |
| 10 | `df -i` | Show inode usage. | `df -i` | Shows inode usage of file systems. |

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
```

### Sample Outputs
```text
$ df -h
Filesystem   Size  Used  Avail  Use%  Mounted on
/dev/sda1    50G   20G   28G    42%   /
/dev/sdb1    100G  60G   40G    60%   /data

$ free -h
       total  used  free  shared  buff/cache  available
Mem:   7.7G   2.1G  2.3G  0.3G    3.3G        4.8G
Swap:  2.0G   0B    2.0G

$ uptime
14:35:21 up 3 days, 2:45, 2 users,
load average: 0.08, 0.12, 0.15
```

> [!tip] Remember
> - Use `df -h` to check disk space regularly.
> - Use `free -h` to monitor memory usage.
> - ==High load average means the system is busy.==
> - Use `vmstat` and `iostat` for performance analysis.
> - ==Always unmount before removing devices.==
> - Use `lsblk` to identify devices before mounting.
> - Use `df -i` to check inode usage.

> [!example] Combine commands with pipes for powerful reports
> ```bash
> $ df -h | grep sda
> $ du -hs * | sort -h
> $ free -h | grep Mem
> ```

## 10. Networking Commands

Network tools for testing connections, moving data, and diagnosing problems.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `ping` | Test connectivity between hosts. | `ping google.com` | Checks if host is reachable and measures latency. |
| 2 | `curl` | Transfer data to/from a server. | `curl -I https://example.com` | Fetches data or HTTP headers from URL. |
| 3 | `wget` | Download files from the web. | `wget https://file.com/file.zip` | Downloads files non-interactively. |
| 4 | `ssh` | Securely connect to a remote server. | `ssh user@server.com` | Opens secure remote shell session. |
| 5 | `scp` | Securely copy files between hosts. | `scp file.txt user@host:/path` | Copies files over SSH securely. |
| 6 | `netstat` | Show network connections, routing and interfaces. | `netstat -tulnp` | Displays active connections and listening ports. |
| 7 | `ss` | Modern replacement for netstat. | `ss -tulnp` | Faster way to show sockets and connections. |
| 8 | `nslookup` | Query DNS to find IP or domain information. | `nslookup example.com` | Looks up DNS records (A, AAAA, CNAME, MX etc). |
| 9 | `dig` | Advanced DNS lookup tool. | `dig example.com` | Detailed DNS query and troubleshooting. |
| 10 | `traceroute` | Trace the path packets take to a host. | `traceroute google.com` | Shows each hop to the destination. |

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

> [!tip] Remember
> - `ping` checks connectivity, not speed.
> - `curl` is great for APIs and headers.
> - Use `ssh` and `scp` for secure file transfer.
> - `ss` is faster and more powerful than `netstat`.
> - `dig` gives more detailed DNS information than `nslookup`.
> - `traceroute` helps find network issues (hops where it fails).
> - Use `-p` option to specify port in `ssh` and `scp`.
> - Always check ==firewall and security group== if connection fails.

> [!example] Combine commands with pipes for network troubleshooting
> ```bash
> $ ping google.com | grep ttl
> $ ss -tulnp | grep 80
> $ dig example.com | grep ANSWER
> ```

## 11. Package Management

Commands for installing, updating, and removing software packages.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `apt` | Advanced Package Tool (Debian / Ubuntu). | `apt search nginx` | Package manager for Debian based systems. |
| 2 | `yum` | Yellowdog Updater Modified (RHEL/CentOS). | `yum list installed` | Package manager for RHEL/CentOS (v7 and below). |
| 3 | `dnf` | Dandified YUM (Fedora/RHEL 8+). | `dnf repolist` | Next-generation package manager (Fedora, RHEL 8+). |
| 4 | `rpm` | Red Hat Package Manager. | `rpm -qa \| grep httpd` | Manages RPM packages (install, query, verify). |
| 5 | `dpkg` | Debian Package Manager. | `dpkg -l \| grep vim` | Manages DEB packages on Debian based systems. |
| 6 | `install` | Install a package. | `apt install nginx` | Installs a software package. |
| 7 | `update` | Update package lists or upgrade packages. | `yum update` | Updates package lists or installed packages. |
| 8 | `remove` | Remove a package. | `apt remove nginx` | Removes a package from the system. |

### Package Managers by Distribution
- **Debian / Ubuntu:** `apt`, `dpkg`
- **RHEL / CentOS 7:** `yum`, `rpm`
- **Fedora / RHEL 8+:** `dnf`, `rpm`

### Common Operations Examples

**APT (Debian/Ubuntu):**
```bash
$ sudo apt update          # update list
$ sudo apt upgrade         # upgrade all
$ sudo apt install nginx   # install package
$ sudo apt remove nginx    # remove package
$ apt search docker
```

**YUM/DNF (RHEL/CentOS/Fedora):**
```bash
$ sudo yum repolist        # show repos
$ sudo yum list installed  # list packages
$ sudo yum install nginx   # install package
$ sudo yum update          # update packages
$ sudo yum remove nginx    # remove package
$ dnf search docker        # search package
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

| | RPM (Red Hat) | DEB (Debian) |
| --- | --- | --- |
| Used in | RHEL, CentOS, Fedora | Debian, Ubuntu |
| File extension | `.rpm` | `.deb` |
| Install | `rpm -ivh file.rpm` | `dpkg -i file.deb` |
| Query | `rpm -qa` | `dpkg -l` |
| Remove | `rpm -e package` | `dpkg -r package` |

> [!tip] Remember
> - Use ==sudo== with package commands.
> - Run ==update== before install/upgrade.
> - `apt` = Debian / Ubuntu · `yum` = RHEL / CentOS 7 · `dnf` = RHEL 8+ / Fedora
> - `rpm` & `dpkg` work on package files directly.
> - Always check package name before install/remove.

## 12. Shell Scripting Basics

Shell scripting automates repetitive work by saving commands to a file and running them.

### 1) Variables
- Used to store data.
- No spaces around `=` sign.
- Access value with `$` before name.
```bash
#!/bin/bash
name="Rakesh"
count=5
echo "Hello $name, count is $count"
```

### 2) If Condition
- Used for decision making.
- Common operators: `-eq`, `-ne`, `-gt`, `-lt`, `-ge`, `-le`
```bash
#!/bin/bash
read -p "Enter a number: " num
if [ $num -gt 10 ]; then
    echo "Number is greater than 10"
else
    echo "Number is 10 or less"
fi
```

### 3) Loops
- `for` loop: iterate over items.
- `while` loop: repeat while condition is true.

**For loop example:**
```bash
#!/bin/bash
for i in 1 2 3 4 5; do
    echo "Number: $i"
done
```

**While loop example:**
```bash
#!/bin/bash
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    count=$((count+1))
done
```

### 4) Functions
- Group commands into reusable blocks.
- Call by function name.
```bash
#!/bin/bash
greet() {
    echo "Hello $1! Welcome to Shell Scripting."
}
greet "Rakesh"   # function call
```

### 5) Arguments
- `$0` → script name.
- `$1`, `$2`, ... → positional arguments.
- `$#` → total number of arguments.
- `$@` → all arguments.
- `$*` → all arguments as a single string.
```bash
#!/bin/bash
echo "Script: $0"
echo "First Arg: $1"
echo "Second Arg: $2"
echo "Total Args: $#"
echo "All Args: $@"
```

### 6) Simple Script Example
Take user input, show date, and list files in current directory.

`myscript.sh`:
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

**Run the script:**
```bash
chmod +x myscript.sh
./myscript.sh
```

**Output (example):**
```text
Enter your name: Rakesh
Hello Rakesh!
Today's date and time:
Fri Oct 30 21:45:00 IST 2026
Files in current directory:
total 8
-rw-r--r-- 1 user user 0 file1.txt
-rw-r--r-- 1 user user 0 file2.txt
Script execution completed.
```

> [!tip] Remember
> - Give execute permission: `chmod +x script.sh`
> - Run script: `./script.sh`
> - Use `#` for comments.
> - Use meaningful variable and function names.

> [!tip] Tip
> - Start small, test often.
> - Break big tasks into small scripts.
> - Use variables, loops and functions to automate real work.

## 13. Linux in Real Projects

Linux runs most of modern IT. It's behind the tools, servers, pipelines, and platforms you use every day.

### Where is Linux Used?

**1) Software Development**
- Preferred OS for developers.
- Supports Git, Vim, GCC, Make, Node.js, Python, Java etc.
- Easy package management and environment setup.
- Lightweight, stable and highly customizable.

**2) Cloud & DevOps**
- Most Linux servers run on AWS, Azure and GCP.
- Used in CI/CD, monitoring, logging and automation.
- Core for Docker, Kubernetes, Terraform, Ansible.
- Used for building and managing cloud infrastructure.

**3) Cyber Security**
- Preferred by security professionals.
- Tools like Kali Linux, Parrot OS.
- Used for penetration testing, vulnerability assessment and digital forensics.
- Tools: Nmap, Wireshark, Metasploit, Burp Suite.

**4) Data Engineering**
- Linux handles big data workloads.
- Runs Hadoop, Spark, Kafka, Airflow, Hive etc.
- Stable, secure and high performance for large datasets.
- Widely used in data pipelines and analytics platforms.

**5) AI / Machine Learning**
- Linux is the go-to OS for AI/ML engineers.
- Supports Python, TensorFlow, PyTorch, CUDA etc.
- Works seamlessly on GPUs and high performance clusters.
- Used for model training, experimentation and deployment.

**6) Web Hosting**
- Most web servers run on Linux.
- Hosts Nginx, Apache, MySQL, PHP, Node.js applications.
- Reliable, lightweight, secure and cost-effective.
- Used for websites, web apps and APIs.

### Real Project Workflow
`Developer → Git → Linux Server → Docker → Jenkins → Kubernetes → Cloud`
1. **Developer** – writes code and tests locally on Linux.
2. **Git** – code is committed and pushed to repository.
3. **Linux Server** – code is pulled on Linux server for build and execution.
4. **Docker** – application is containerized using Docker.
5. **Jenkins** – builds, tests and creates artifacts.
6. **Kubernetes** – deploys and manages containers at scale.
7. **Cloud** – application runs on cloud (AWS / Azure / GCP).

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
> - Linux is not just an OS, it's the ==foundation of modern IT==.
> - From coding to deploying, Linux is ==everywhere==!
> - It helps in building reliable, scalable and secure systems.
> - Master Linux = Career Booster.

> [!quote]
> "Behind every great project, there is a powerful Linux system running silently."

## 14. Most Asked Linux Interview Questions

| # | Question | Concise Answer |
| --- | --- | --- |
| 1 | What is Linux? | Linux is an ==open-source== operating system based on the Linux kernel. It is stable, ==secure== and widely used in servers, cloud, desktops and embedded devices. |
| 2 | Kernel vs Shell? | • **Kernel** is the ==core== of the OS. It manages hardware, memory, processes and system resources.<br>• **Shell** is a command-line interface (CLI) that allows users to interact with the kernel. |
| 3 | Soft Link vs Hard Link? | • **Soft Link (Symbolic Link):** points to the path of original file. If original file is deleted, link breaks. (`ln -s`)<br>• **Hard Link:** points to the inode of original file. If original file is deleted, link still works. (`ln`) |
| 4 | chmod vs chown? | • `chmod`: changes file/directory permissions. Example: `chmod 755 file.txt`<br>• `chown`: changes ownership of a file/directory. Example: `chown user:group file.txt` |
| 5 | grep vs find? | • `grep`: searches for a pattern inside file content. Example: `grep "error" log.txt`<br>• `find`: searches for files/directories by name, type, size, time, etc. Example: `find /var -name "*.log"` |
| 6 | Process vs Thread? | • **Process** is an independent program with its own memory space.<br>• **Thread** is the smallest unit of execution within a process and shares the same memory space with other threads. |
| 7 | Zombie Process? | A terminated process whose parent has not yet read its exit status. It holds a PID entry in the process table but does not consume CPU or memory resources. |
| 8 | Cron Job? | Cron is a ==time-based job scheduler== in Linux. It runs commands or shell scripts automatically at specified time intervals. Edit cron using: `crontab -e` |
| 9 | Environment Variables? | Variables that store system or user specific values used by processes and shell. Examples: `HOME`, `PATH`, `USER`, `SHELL`. Use: `echo $PATH` or `export VAR=value` |
| 10 | Pipe (`\|`) vs Redirection (`>`)? | • **Pipe (`\|`):** sends output of one command as input to another. Example: `ls -l \| grep "txt"`<br>• **Redirection (`>`):** sends output of a command to a file. Example: `ls -l > out.txt` |

> [!tip] Remember
> - Practice commands daily.
> - Understand concepts, not just syntax.
> - ==Hands-on is the key== to master Linux.
> - Use `man` pages (e.g. `man ls`) for detailed info.

> [!tip] Tip
> - Use `man command` to read the manual.
> - Try commands in a Linux VM or cloud server.
> - Practice with real-world scenarios.
> - Make short notes and revise regularly.
> - Combine commands using pipes and redirection.

> [!quote]
> "The more you practice in Linux, the more powerful you become!"

## 15. Cheat Sheet & Roadmap

### Top 50 Linux Commands

| # | Command | Purpose |
| --- | --- | --- |
| 1 | `pwd` | Print working directory |
| 2 | `ls` | List files and directories |
| 3 | `cd` | Change directory |
| 4 | `mkdir` | Create a directory |
| 5 | `rmdir` | Remove an empty directory |
| 6 | `touch` | Create an empty file |
| 7 | `cp` | Copy files or directories |
| 8 | `mv` | Move or rename files |
| 9 | `rm` | Remove files or directories |
| 10 | `cat` | Display file contents |
| 11 | `less` | View file content (scrollable) |
| 12 | `head` | Show first lines of a file |
| 13 | `tail` | Show last lines of a file |
| 14 | `nano` | Edit files in terminal |
| 15 | `vi` | Edit files (advanced) |
| 16 | `chmod` | Change file permissions |
| 17 | `chown` | Change file owner |
| 18 | `ps` | Show running processes |
| 19 | `top` | Monitor system processes |
| 20 | `kill` | Terminate a process |
| 21 | `df` | Disk space usage |
| 22 | `du` | Directory space usage |
| 23 | `free` | Show memory usage |
| 24 | `uname` | System information |
| 25 | `hostname` | Show or set hostname |
| 26 | `whoami` | Show current user |
| 27 | `id` | Show user and group IDs |
| 28 | `grep` | Search text in files |
| 29 | `find` | Search files and directories |
| 30 | `locate` | Find files by name (updatedb) |
| 31 | `which` | Locate command path |
| 32 | `echo` | Display text |
| 33 | `history` | Show command history |
| 34 | `man` | Show manual of a command |
| 35 | `whatis` | Brief description of command |
| 36 | `alias` | Create command shortcut |
| 37 | `tar` | Archive files |
| 38 | `gzip` | Compress files |
| 39 | `gunzip` | Decompress .gz files |
| 40 | `zip` | Zip files |
| 41 | `unzip` | Unzip files |
| 42 | `scp` | Secure copy (remote files) |
| 43 | `ssh` | Secure shell login |
| 44 | `ping` | Check connectivity |
| 45 | `netstat` | Network connections |
| 46 | `ss` | Socket statistics |
| 47 | `curl` | Transfer data from URL |
| 48 | `wget` | Download files from web |
| 49 | `crontab` | Schedule tasks (cron jobs) |
| 50 | `clear` | Clear the terminal screen |

### Linux Learning Roadmap
1. **Basics** – Linux introduction, file system, navigation, basic commands
2. **Commands** – file operations, text processing, permissions, users & groups
3. **System Admin** – processes, services, logs & monitoring, scheduling (cron)
4. **Networking** – IP, DNS, SSH, networking tools, firewall basics, troubleshooting
5. **DevOps Tools** – Git, Docker, Jenkins, CI/CD basics
6. **Orchestration** – Kubernetes, YAML basics, deploy apps, scaling & HA
7. **Cloud** – AWS / Azure / GCP, Linux on cloud, automation, real projects

> [!tip] Remember
> - Practice commands daily.
> - Use `man <command>` for help.
> - Learn by doing real tasks.
> - Small steps, big progress!

> [!tip] Tips
> - Start with basic commands.
> - Be consistent and practice.
> - Make your own cheat sheet.
> - Try real projects and scenarios.
> - Use a Linux VM or cloud server.

> [!success] Best Practices
> - Use documentation.
> - Automate repetitive tasks.
> - Keep system updated.
> - Take regular backups.
> - Follow security best practices.
> - Practice troubleshooting.

> [!quote]
> "Linux is not just an OS, it's a skill that powers your ==DevOps== journey!"



## 16. User & Group Management

Commands for creating, modifying, and deleting users and groups, plus controlling access.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `useradd` | Create a new user. | `sudo useradd -m -s /bin/bash ali` | Creates user with home directory and shell. |
| 2 | `usermod` | Modify an existing user. | `sudo usermod -aG sudo ali` | Adds user to a group (e.g. sudo). |
| 3 | `userdel` | Delete a user. | `sudo userdel -r ali` | Removes user and home directory. |
| 4 | `passwd` | Set/change password. | `sudo passwd ali` | Prompts for a new password. |
| 5 | `chage` | Change password expiry info. | `chage -l ali` | Shows password aging details. |
| 6 | `groupadd` | Create a new group. | `sudo groupadd devs` | Creates group "devs". |
| 7 | `groupdel` | Delete a group. | `sudo groupdel devs` | Removes the group. |
| 8 | `groups` | Show groups of a user. | `groups ali` | Lists group memberships. |
| 9 | `id` | Show UID, GID and groups. | `id ali` | Displays user and group IDs. |
| 10 | `w` / `who` | Show logged-in users. | `w` | Shows users and what they are doing. |
| 11 | `last` | Show login history. | `last -n 10` | Shows last 10 login sessions. |
| 12 | `su` | Switch user. | `su - ali` | Switches to another user's shell. |
| 13 | `sudo` | Run command with elevated rights. | `sudo apt update` | Executes as root (per sudoers). |

### Key Account Files
- `/etc/passwd` – user account info (name, UID, GID, home, shell).
- `/etc/shadow` – ==encrypted passwords== and aging info (root only).
- `/etc/group` – group definitions and memberships.
- `/etc/sudoers` – sudo privileges (edit with ==`visudo`== only).

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
```

> [!warning] Important
> - Always use ==`-aG`== with `usermod` (append to group). Without `-a` the user is ==removed from all other groups==.
> - Never edit `/etc/sudoers` directly — use ==`visudo`== to avoid locking yourself out.
> - `userdel -r` deletes the home directory ==permanently==.

> [!tip] Remember
> - UID 0 = ==root==. UIDs 1–999 are usually system accounts. Regular users start from 1000.
> - Every user has one ==primary group== and optional supplementary groups.
> - `sudo` logs every command it runs — good for auditing.

## 17. Permissions & Ownership

Linux controls file access with read, write, and execute permissions for owner, group, and others.

### Permission Values

| Symbol | Meaning | Value |
| --- | --- | --- |
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |

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

### umask & ACLs
```bash
umask                      # shows default mask (e.g. 022)
umask 027                  # new files: 640, dirs: 750

getfacl file.txt               # view ACL
setfacl -m u:ali:r file.txt    # give user ali read access
setfacl -x u:ali file.txt      # remove ACL entry
```

> [!tip] Remember
> - Directories need ==x== to enter (`cd`), ==r== to list (`ls`), ==w== to create/delete inside.
> - Files need ==x== to run as a program or script.
> - ==777 is almost never acceptable== — it's a security risk.
> - SSH private keys must be ==600== or SSH refuses to use them.

> [!warning] Watch out
> A wrong recursive `chmod`/`chown` on system directories can ==break the whole system==. Always test on a copy first.

## 18. Systemd & Service Management

systemd is the init system and service manager on modern Linux. It handles booting, services, mounts, timers, and logs.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `systemctl start` | Start a service now. | `sudo systemctl start nginx` | Runs the service immediately. |
| 2 | `systemctl stop` | Stop a running service. | `sudo systemctl stop nginx` | Halts the service. |
| 3 | `systemctl restart` | Restart a service. | `sudo systemctl restart nginx` | Full stop + start. |
| 4 | `systemctl reload` | Reload config without stopping. | `sudo systemctl reload nginx` | Applies new config gracefully. |
| 5 | `systemctl enable` | Start service at boot. | `sudo systemctl enable nginx` | Creates boot symlink. |
| 6 | `systemctl disable` | Don't start at boot. | `sudo systemctl disable nginx` | Removes boot symlink. |
| 7 | `systemctl status` | Show service state. | `systemctl status nginx` | Active state, PID, recent logs. |
| 8 | `systemctl list-units` | List loaded units. | `systemctl list-units --type=service` | Shows running services. |
| 9 | `systemctl list-unit-files` | List unit files + boot state. | `systemctl list-unit-files --type=service` | Shows enabled/disabled. |
| 10 | `journalctl` | View systemd logs. | `journalctl -u nginx -f` | Follow logs of a service. |
| 11 | `systemd-analyze` | Analyze boot time. | `systemd-analyze blame` | Shows slowest units at boot. |
| 12 | `systemctl list-timers` | List scheduled timers. | `systemctl list-timers` | Modern cron alternative. |

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
> - ==`reload` vs `restart`==: reload keeps connections alive; restart drops them.
> - `enable` = at boot, `start` = now. Usually you want ==`enable --now`==.
> - Always run ==`daemon-reload`== after editing unit files.
> - `journalctl -f` is the modern `tail -f` for service logs.

## 19. Archive & Compression

Archiving bundles files together; compression shrinks them. tar is the standard tool, usually combined with gzip, bzip2, or xz.

| # | Command | Description | Example | What it does |
| --- | --- | --- | --- | --- |
| 1 | `tar -czf` | Create gzip archive. | `tar -cvzf backup.tar.gz folder/` | Creates a compressed archive. |
| 2 | `tar -x` | Extract archive. | `tar -xvzf backup.tar.gz` | Extracts to current directory. |
| 3 | `tar -t` | List archive contents. | `tar -tvf backup.tar.gz` | Shows files inside without extracting. |
| 4 | `gzip` | Compress a single file. | `gzip file.txt` | Creates `file.txt.gz`. |
| 5 | `gunzip` | Decompress .gz. | `gunzip file.txt.gz` | Restores the original file. |
| 6 | `bzip2` | Stronger compression. | `bzip2 file.txt` | Creates `file.txt.bz2`. |
| 7 | `xz` | Best compression ratio. | `xz file.txt` | Creates `file.txt.xz`. |
| 8 | `zip` / `unzip` | Zip archive (cross-platform). | `zip -r archive.zip folder/` | Zips folder recursively. |
| 9 | `zcat` | View .gz without extracting. | `zcat file.gz` | Prints decompressed content. |

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
| gzip | `.gz` | Fast | Good | Default choice |
| bzip2 | `.bz2` | Slower | Better | Legacy systems |
| xz | `.xz` | Slowest | Best | Software distribution |
| zip | `.zip` | Fast | Good | Sharing with Windows |

> [!tip] Remember
> - ==tar archives, gzip compresses== — `.tar.gz` = archive + compression together.
> - `-f` must come last because it consumes the filename argument.
> - List before extracting (`tar -tvf`) to avoid ==absolute paths / tar bombs==.
> - Use `zip` for Windows users, `tar.gz` for Linux backups.

## 20. Troubleshooting & System Logs

Troubleshooting means collecting evidence from logs, processes, and the network, then isolating the failing layer.

### Key Log Files

| Path | Contains |
| --- | --- |
| `/var/log/syslog` (or `messages`) | General system activity |
| `/var/log/auth.log` (or `secure`) | Logins, sudo, SSH attempts |
| `/var/log/kern.log` | Kernel messages |
| `dmesg` / `/var/log/dmesg` | Boot & hardware messages |
| `/var/log/boot.log` | Boot process output |
| `/var/log/cron` | Cron job execution |
| `/var/log/nginx/`, `/var/log/apache2/` | Web server logs |

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

# Who & when
last -n 20      # recent logins
lastb           # failed login attempts
w               # currently logged in + activity
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
```

> [!warning] Common real-world causes
> - ==Disk 100% full== → services crash, logs stop (`df -h`, `df -i`)
> - ==Wrong permissions/ownership== after manual edits (`ls -l`)
> - ==Service not enabled== → "worked yesterday, dead after reboot"
> - ==Firewall rule== blocking a port (`ufw status`, `ss -tulnp`)
> - ==DNS misconfiguration== → "internet works but names fail"

> [!tip] Remember
> - Logs carry ==timestamps== — always correlate the failure time with log entries.
> - Change ==one variable at a time== and document every change.
> - Stuck? Search the ==exact error message== from the logs.
> - Practice the loop: symptom → logs → isolate → fix → verify → document.

> [!quote]
> "Logs don't lie — learn to read them."



## Related Notes

- [[IT Support Interview Questions]]
