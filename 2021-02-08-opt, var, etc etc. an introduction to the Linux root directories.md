---
layout: post
title:  "opt, var, etc etc. an introduction to the Linux root directories"
categories: linux general
---

# opt, var, etc etc. an introduction to the Linux directories
{: style="text-align: center"}

Written by Nick Otter. 

# Contents

- [**Introduction**](#introduction)<br>
- [**The contents of /**](#the-contents-of-/)<br>

# Introduction 

Let's take a look at the Linux `/` directories and their purposes.

# The contents of /

This is a fresh install of RHEL 8. What's seen here is present in most other Linux distributions. 

```
[minikube@control-plane ~]$ cd /
```
```
[minikube@control-plane /]$ ls
bin   dev  home  lib64  mnt  proc  run   src  sys  usr
boot  etc  lib   media  opt  root  sbin  srv  tmp  var
```

Let's create a table of each directories purpose and any subdirectories of interest. It's very important to understand eaches initial purpose; for debugging, configuration and installation. I refer to the **FHS** wiki, instead of the docs. The Filesystem Hierarchy Standard is widely referenced and acknowledged. You can view it [here](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) and the complete docs on [linuxfoundation.org](https://refspecs.linuxfoundation.org/FHS_3.0/index.html). The latest version is **3.0** released on 3 June 2015. Most Linux distributions follow the Filesystem Hierarchy Standard and declare it their own policy to maintain FHS compliance.

So, here goes... I'm going to go through the table documented on the wiki and add any additional notes too.

| Directory  | Description |
| `/`        | Primary hierarchy root and root directory of the entire file system hierarchy. |
| `/bin`     | Essential command binaries that need to be available in single-user mode, including to bring up the system or repair it;[3] for all users, e.g., cat, ls, cp. |
| `/boot`    | Boot loader files, e.g., kernels, initrd. |
| `/dev`     | Device files, e.g., /dev/null, /dev/disk0, /dev/sda1, /dev/tty, /dev/random. |
| `/etc`     | Host-specific system-wide configuration files. |
| `/etc/opt` | Configuration files for add-on packages that are stored in `/opt`. |
| `/etc/xml` | Configuration files, such as catalogs, for software that processes XML. |
| `/home`    | Directories users login to. User directories populated by all contents of`/etc/skel` directory. |
| `/lib`     | Libraries essential for the binaries in `/bin` and `/sbin`. |
| `/lib64`   | Alternate format essential libraries. E.g. 64 bit or 32 bit depending on install format. |
| `/media`   | Mount points for removable media such as USB. |
| `/mnt`     | Temporarily mounted filesystems. Persistent mounts are configured in `/etc/fstab`. |
| `/opt`     | Optional application software packages. Not required by the system to function, and normally installed additionally by the user. |
| `/proc`    | VFS providing process and kernel information _as files_. Corresponds to **procfs** mount. Generally automatically generated and populated by the system, on the fly. |
| `/root`    | Home dir for the root user. |
| `/run`     | Run-time variable data: information about the running system since last boot e.g. cron, mount, docker, rsyslog. Files under this directory must be either removed or truncated at the begining of the boot process, but this is not necessary on systems that provide this directory as a `tmpfs`. |
| `/sbin`    | Essential system binaries e.g. **fsck**, **init**, **mkfs**. |
| `/srv`     | An old school directory. Out of use some of the time now. Data directory. For data such as data and scripts for web servers, data offered by FTP servers (FTP is (hopefully) a thing of the past.) |
| `/sys`    | Contains information about devices, drivers and some kernel features. |
| `/tmp`    | Directory for temporary files (see also `/var/tmp`). Often not preserved between system reboots and may be severely size-restricted. |
| `/usr`    | _Secondary hierarchy_ for read-only user data; contains the majority of (multi-)user utilities and applications. Should be shareable and read-only.|
| &nbsp;&nbsp;`/usr/bin`     | Non-essential command binaries (not needed in single-user mode); for all users. |
| &nbsp;&nbsp;`/usr/include` | **Header** files. Read more [here](https://en.wikipedia.org/wiki/Include_directive). |
| &nbsp;&nbsp;`/usr/lib` | Libraries for the binaries in `/usr/bin` and `/usr/sbin`. |
| &nbsp;&nbsp;`/usr/lib<qual>` | Alt-format libraries, e.g. `/usr/lib32` for 32-bit libriaries on a 64-bit machine. |
| &nbsp;&nbsp;`/usr/local`     | _Tertiary hierarchy_ for local data, specific to the host. Normally has further subdirectories e.g. `bin`, `lib`.
| &nbsp;&nbsp;`/usr/sbin`      | Non-essential system binaries e.g. daemons for various network services. |
| &nbsp;&nbsp;`/usr/share`     | Architectture-independent (shared) data. |
| &nbsp;&nbsp;`/usr/src`       | Source code e.g. the kernel source code with its header files. |
| `/var`     | Variable files: files whose content is expected to continually change during the normal operation of the system, such as logs and spool files. |
| &nbsp;&nbsp;`/var/cache` | Application cache data. Such data are locally generated as a result of time-consuming I/O or calculation. The application must be able to regenerate or restore the data. The cached files can be deleted without loss of data. |
| &nbsp;&nbsp;`/var/lib` | State information. Persistent data modified by programs as they run. Normally containing directories named after applications but no persistent data. |
| &nbsp;&nbsp;`/var/lock` | Lock files. Files keeping track of resources currently in use. |
| &nbsp;&nbsp;`/var/log` | Log files. Various logs. Log files here might be generated by the **rsyslog** daemon. |
| &nbsp;&nbsp;`/var/mail` | Mailbox files. In some distributions, these files may be located in the deprecated `/var/spool/mail`. |
| &nbsp;&nbsp;`/var/opt` | Variable data from add-on packagaes that are stored in `/opt`. |
| &nbsp;&nbsp;`/var/run` | Run-time variable data. This directory contains system information data describing the system since it was booted. In FHS 3.0, `/var/run` is replaced by `/run`; a system should either continue to provide a `/var/run` directory or provide a sym link from `/var/run` to `/run` for backwards compatibility. |
| &nbsp;&nbsp;`/var/spool` | Spool for tasks waiting to be processed, e.g. print queues and outgoing mail queue. 
| &nbsp;&nbsp;&nbsp;&nbsp;`/var/spool/mail` | Deprecated location for users' mailboxes. |
| &nbsp;&nbsp; `/var/tmp` | Temporary files to be **preserved between reboots**. |


---
Thanks. This article was written by Nick Otter.
