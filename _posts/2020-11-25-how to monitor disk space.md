---
layout: post
title:  "How to monitor Disk Space"
categories: disk
---

# How to monitor Disk Space
{: style="text-align: center"}

Written by Nick Otter.

# Contents: 

- [**Introduction**](#introduction)<br>
    - [Requirements](#requirments)<br>
- [**df**](#df)<br>
    - [df cheatsheet](#df-cheatsheet)<br>
    - [df output](#df-output)<br>
- [**du**](#du)<br>
    - [du cheatsheet](#du-cheatsheet)<br>
    - [du output](#du-output)<br>
- [**How you can investigate large files or directories**](#how-you-can-investigate-large-files-directories)<br>
    - [ls lsof fuser example](#ls-lsof-fuser-example)<br>
- [**How to use auditctl**](#how-to-use-auditctl)<br>
    - [Understanding the audit.log file](#understanding-the-audit.log-file)<br>
- [**How to clear disk space**](#how-to-clear-disk-space)<br>
    - [Some commands to clear some disk space](#some-commands-to-clear-some-disk-space)

# Introduction 

Herein lies some hints, tricks and commands to keep on top of disk space for your Linux server. We're going to look at **`du`** **`df`** to get general stats on disk space on a system, investigate files that are taking up lots of disk space and then look at some nice ways to clear disk space.

So, onto **`df`**.

# Requirements

| Updated | `04/2020` | 
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

# df
_see file system disk space usage._

**`df`** will allow us to analyse disk space by **filesystem** and we will be able to see the **mount point** of each filesystem so we can associate a purpose against it's size.

Here are a couple of helpful flags for **`df`**.

* See file system type by using `-T`.
* See inodes instead of blocks by using `-i`.

# df cheatsheet

| `df -h`| See output as human readable.|
|`df -T`| See file system type. |
|`df -i`| See inodes. |

# df output

So, let's put it into action. Let's take a look at disk space used by filesystems on this `RHEL` `8` system.
```
[root@rhel-8-1 ~]# df -hT
Filesystem     Type      Size  Used Avail Use% Mounted on
devtmpfs       devtmpfs  4.9G     0  4.9G   0% /dev
tmpfs          tmpfs     4.9G     0  4.9G   0% /dev/shm
tmpfs          tmpfs     4.9G  1.5M  4.9G   1% /run
tmpfs          tmpfs     4.9G     0  4.9G   0% /sys/fs/cgroup
/dev/sda2      xfs        19G   14G  4.9G  74% /
/dev/sr0       iso9660   7.4G  7.4G     0 100% /repo
/dev/sda1      xfs      1014M  226M  789M  23% /boot
tmpfs          tmpfs     995M  1.2M  994M   1% /run/user/42
tmpfs          tmpfs     995M  6.8M  988M   1% /run/user/0
```

Ok great. But do we really need know all that info? Not really. If I was in a rush I'd just look for filesystems that's usage is `70` `%` or over as we'll need to clear some space on those ASAP to prevent performance issues. Let's use **`awk`** to speed that up.

```
[root@rhel-8-1 ~]# df -a | awk '+$5 >= 70 {print}'
/dev/sda2       18940160 13818084   5122076  73% /
/dev/sr0         7667190  7667190         0 100% /repo
```

Great. So we need to take a look at the root partition (**`/`**) and the `sr0` device **`/repo`** partition.`/dev/sr0` is actually the SCSI CD-ROM device in a Linux system, so we don't need to be concerned that that it is `100%` as that just means it is in use. `/` is using our hard disk, `sda2`. We'll need to clear some space on this as it's over 70% so could lead to some performance issues in no time or maybe something even more critical.

# du
_see disk space usage for specific files or directories._

With `df` we've seen that our root partition is using 73% of our hard drive `/dev/sda2`. This is high! We can now use **`du`** to analyse the root partition and find what's taking up the most of space.  

# du cheatsheet

| `--max-depth=1` | Show first column of result only. |
| `sort` | `coreutils` filter function. |
| `-h` | See output as human readable. |
| `-ck` | Show total block size.|

# du output

Let's investigate `/` with du. We can sort in order of size to ignore the really small stuff. 

`du -h --max-depth=1 / 2>/dev/null | sort -h | tail` should do the trick.

```
[root@rhel-8-1 ~]# du -h --max-depth=1 / 2>/dev/null | sort -h | tail
1.5M	/run
32M	/etc
54M	/opt
187M	/boot
579M	/root
742M	/tmp
2.8G	/var
7.4G	/repo
8.8G	/usr
21G	/
```

So, what can we see. Let's remember standard Linux usage and work out where best we can clear some space. Descending in size, let's make a presumption of the purpose of each directory base on fundamental Linux knowledge. `/usr` and `/var` are for applications. Var being tmp files subject to change and usr source files. So we maybe don't need to look in there to make space, `/repo` is the mountpoint for our external drive so not there. `/tmp` potentially just log files so we could look in there. `/root`... now this one stands out. Why is the /root users partition so big? Let's take a look.

```
[root@rhel-8-1 ~]# du -h --max-depth=1 /root 2>/dev/null | sort -h | tail
724K	/root/vmtouch
904K	/root/linux-ftools
2.6M	/root/go/bin
26M	/root/go/src
29M	/root/go
74M	/root/rhel-8.iso
96M	/root/another_test
96M	/root/foo
130M	/root/kdump
130M	/root/kdump/127.0.0.1-2020-04-28-01:31:54
```
 
Ah hah. Some pretty misc files and a bit of mismanagement is present. A couple of applications that should probably be moved to `/usr`. `foo` and `another test` look very dubious. A kdump kernel memory image file? Do we really need that? No I don't think so. For the sake of it, let's see the file's block size.

```
[root@rhel-8-1 ~]# du -ck /root/kdump/127.0.0.1-2020-04-28-01:31:54
132108	/root/kdump/127.0.0.1-2020-04-28-01:31:54
132108	total
```

# How you can investigate large files or directories

Sometimes you will find file or directory that is hogging disk space and you want to find out how they were generated or what process is currently using them to take further action.

Here are some commands that are helpful to do so. **N.B.** **`audtictl`** requires prior installation and configuration.

|||
|--|--
| `ls -la` | Show owners and time last modified for file.|
| `ls -lad` | Show owners and time last modified for directory.|
|`lsof` | Show process currently using a file.|
|`auditctl`|Track actions that have been performed on a file.|
|`/var/log/audit/audit.log`| Audit log file to see what actions have been performed on a file and by what users.|

# ls lsof fuser example

So.. using an example where `foo` is some data. Let's see if we can find out what created these large files so we can economise on disk space in future.  

Let's take a look at the size of foo, it's owner and permissions and the date and time it was last updated. (If we want to do the same for a directory just use `ls -lad`).
```
[root@rhel-8-1 ~]# ls -lah foo
-rw-r--r--. 1 root root 96M May  4 08:53 foo
```

Neat, okay. So we know root is the owner. But is there a way to see what process updated the file? Using `lsof` we can see what process is currently using the file, that's a good start. To be quick we can just use `fuser` to get the process id.

```
[root@rhel-8-1 ~]# lsof foo
COMMAND  PID USER   FD   TYPE DEVICE  SIZE/OFF     NODE NAME
tail    5611 root    3r   REG    8,2 100000000 16949133 foo
```
```
[root@rhel-8-1 ~]# fuser foo
/root/foo:            5611
```
However, `lsof` can't help us if the process **isn't** currently operating on the file. To track what processes use a file over a period of time we can use `auditctl`.

# How to use auditctl

`auditctl` is shipped with this distribution of RHEL 8 (`4.18`). To initialise `auditctl` to track a file called foo, run this command.

```
$ auditctl -w /root/foo -p warx -k foo
```

|`-w`| Add watch at path.|
| `-p warx` | Set permissions filter for watch.|
| `-k` | Set identifying key for rule/watch.|

To check the configuration has been implemented, run `auditctl -l`. 
```
[root@rhel-8-1 ~]# auditctl -l
-w /root/foo -p rwxa -k foo
```
Neat.

# Understanding audit.log

So with the auditctl trace configured, a log file will be generated by auditctl at `/var/log/audit/audit.log`. Querying the file name in the audit log file will now give us a list of all the actions performed on that file since auditctl was configured. 

Let's have a look at that trace.

```
[root@rhel-8-1 ~]# grep foo /var/log/audit/audit.log
type=SYSCALL msg=audit(1588597740.979:2298): arch=c000003e syscall=257 success=yes exit=3 a0=ffffff9c a1=7fff82657442 a2=0 a3=0 items=1 ppid=3384 pid=17800 auid=0 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts0 ses=3 comm="cat" exe="/usr/bin/cat" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key="foo"ARCH=x86_64 SYSCALL=openat AUID="root" UID="root" GID="root" EUID="root" SUID="root" FSUID="root" EGID="root" SGID="root" FSGID="root"
type=PATH msg=audit(1588597740.979:2298): item=0 name="/root/foo" inode=16949133 dev=08:02 mode=0100644 ouid=0 ogid=0 rdev=00:00 obj=unconfined_u:object_r:admin_home_t:s0 nametype=NORMAL cap_fp=0000000000000000 cap_fi=0000000000000000 cap_fe=0 cap_fver=0OUID="root" OGID="root"
```
A bit condensed, so let's break it down field by field in a table.

| `type=SYSCALL`| Shows that this audit message was triggered by a system call to the kernel. |
| `msg=audit(1588597740.979:2298)` | Timestamp and ID of the audit message. |
| `arch=c000003e` |  CPU architecture. |
| `syscall=257` | Type of system call sent to the kernel (257=`open_at`).|
| `pid=` | Process ID (PID) of the system call. |
| `ppid=` | Parent Process ID (PPID) of the the system call. |
| `auid=0` | UID of user who triggered the audit message (0 = `root`).|
| `uid=0` | UID of user who started the `type=SYSCALL` process (0 = `root`).|
| `comm="cat"` | Name of command that triggered the audit message.|
| `exe="/usr/bin/cat"` | Path of command that triggered the audit message.|
|`type=PATH`| Path passed to the system call as an argument. |
|`name="/root/foo"`| Full path or directory that was passed to the system call.|

Nice. 

# How to clear disk space 

Delete files! Here are some serial offender disk space hogs that I've come across in RHEL 8:
* yum cache.
* core dumps.
* `.log`, `.rpm`, `.tar`, `.tar.gz` files.

And below are some ideas of how best to delete them, mostly using `find`. 

# Some commands to clear some disk space

| `yum clean all` | Clean yum cache. |
| `rm -rf /var/cache/yum`| Clear orphaned yum data. |
| `find -regex ".*/core\.[0-9]+$" -delete`| Remove core dumps (after debugging). |
| `find /var -name "*.log" \( \( -size +50M -mtime +7 \) -o -mtime +30 \) -exec truncate {} --size 0 \;` | Trim logs. |

---

Thanks. [Clear disk space on Centos](https://www.getpagespeed.com/server-setup/clear-disk-space-centos) and [how to free disk space one linux systems](https://www.techrepublic.com/article/how-to-free-disk-space-on-linux-systems/) were helpful in writing this, among other articles. And do take a look at the `ncdu`(_disk usage analyzer with an ncurses interface_) [repository](https://dev.yorhel.nl/ncdu). This was written by Nick Otter.
