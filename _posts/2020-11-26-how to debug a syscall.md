---
layout: post
title:  "How to debug a syscall"
categories: linux kernel
---

# How to debug a syscall
{: style="text-align: center"}

# Contents

- [**Introduction**](#introduction)<br>
     - [Requirements](#equirements)<br>
- [**Strace**](#strace)<br>
     - [Strace output explained](#strace-output-explained)<br>
     - [Common syscall script return values explained](#common-syscall-script-return-values-explained)<br>
     - [Common posix syscall](#common-posix-system-calls)<br>
     - [What you can do with strace](#what-you-can-do-with-strace)<br>
<br><br><br><br>

# Introduction

![](https://user-images.githubusercontent.com/26765027/98005456-f41b9a80-1de8-11eb-9a7c-b4b284841bdd.png)

"But what's actually happening when I run that command..?" I've heard that a number of times. The answer is sometimes, a **`syscall`**. System calls are how a program enters the kernel to perform a task. It's widely covered. Here's some great blocks explaining syscalls from [The Definitive Guide to Linux System Calls](https://blog.packagecloud.io/eng/2016/04/05/the-definitive-guide-to-linux-system-calls/#what-is-a-system-call) by [Package Cloud](https://packagecloud.io/). 
```
When you run a program which calls open, fork, read, write (and many others) you are making a system call.
```

```
User programs (like your editor, terminal, ssh daemon, etc) need to interact with the Linux kernel so that the kernel can perform a set of operations on behalf of your user programs that they can’t perform themselves.

For example, if a user program needs to do some sort of IO (open, read, write, etc) or modify its address space (mmap, sbrk, etc) it must trigger the kernel to run to complete those actions on its behalf.
```

# Requirements

| Updated | `04/2020` |
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

<br><br><br>

# Strace

`strace` allows you to see the actual system calls to the kernel and their results when a command is run.

Let's take a look with `strace` at what happens when calling `cat` with a new file `foo`:

```
$ strace -tT cat foo
```

```
[root@rhel-8-1 ~]# strace -tT cat foo
08:42:01 execve("/usr/bin/cat", ["cat", "foo"], 0x7ffc470097b0 /* 44 vars */) = 0 <0.000510>
08:42:01 brk(NULL)                      = 0x557016302000 <0.000060>
08:42:01 arch_prctl(0x3001 /* ARCH_??? */, 0x7ffcb7452a40) = -1 EINVAL (Invalid argument) <0.000058>
08:42:01 access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory) <0.000067>
08:42:01 openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3 <0.000067>
08:42:01 fstat(3, {st_mode=S_IFREG|0644, st_size=79333, ...}) = 0 <0.000059>
08:42:01 mmap(NULL, 79333, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f3aa0873000 <0.000067>
08:42:01 close(3)                       = 0 <0.000376>
08:42:01 openat(AT_FDCWD, "/lib64/libc.so.6", O_RDONLY|O_CLOEXEC) = 3 <0.000029>
08:42:01 read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\2009\2\0\0\0\0\0"..., 832) = 832 <0.000018>
08:42:01 fstat(3, {st_mode=S_IFREG|0755, st_size=5993088, ...}) = 0 <0.000016>
08:42:01 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f3aa0871000 <0.000021>
08:42:01 mmap(NULL, 3942432, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f3aa029c000 <0.000024>
08:42:01 mprotect(0x7f3aa0455000, 2097152, PROT_NONE) = 0 <0.000045>
08:42:01 mmap(0x7f3aa0655000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b9000) = 0x7f3aa0655000 <0.000032>
08:42:01 mmap(0x7f3aa065b000, 14368, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f3aa065b000 <0.000022>
08:42:01 close(3)                       = 0 <0.000016>
08:42:01 arch_prctl(ARCH_SET_FS, 0x7f3aa0872540) = 0 <0.000016>
08:42:01 mprotect(0x7f3aa0655000, 16384, PROT_READ) = 0 <0.000028>
08:42:01 mprotect(0x5570153ca000, 4096, PROT_READ) = 0 <0.000024>
08:42:01 mprotect(0x7f3aa0887000, 4096, PROT_READ) = 0 <0.000026>
08:42:01 munmap(0x7f3aa0873000, 79333)  = 0 <0.000043>
08:42:01 brk(NULL)                      = 0x557016302000 <0.000017>
08:42:01 brk(0x557016323000)            = 0x557016323000 <0.000019>
08:42:01 brk(NULL)                      = 0x557016323000 <0.000015>
08:42:01 openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3 <0.000032>
08:42:01 fstat(3, {st_mode=S_IFREG|0644, st_size=217796096, ...}) = 0 <0.000017>
08:42:01 mmap(NULL, 217796096, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f3a932e7000 <0.000023>
08:42:01 close(3)                       = 0 <0.000016>
08:42:01 fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 0), ...}) = 0 <0.000018>
08:42:01 openat(AT_FDCWD, "foo", O_RDONLY) = 3 <0.000024>
08:42:01 fstat(3, {st_mode=S_IFREG|0644, st_size=0, ...}) = 0 <0.000016>
08:42:01 fadvise64(3, 0, 0, POSIX_FADV_SEQUENTIAL) = 0 <0.000017>
08:42:01 mmap(NULL, 139264, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f3aa084f000 <0.000023>
08:42:01 read(3, "", 131072)            = 0 <0.000076>
08:42:01 munmap(0x7f3aa084f000, 139264) = 0 <0.000047>
08:42:01 close(3)                       = 0 <0.000026>
08:42:01 close(1)                       = 0 <0.000019>
08:42:01 close(2)                       = 0 <0.000033>
08:42:01 exit_group(0)                  = ?
08:42:01 +++ exited with 0 +++
```

Not for the faint of heart. But it's not too complicated.
<br><br>

# Strace output explained

Let's take a look at the key parts of the ouput.


| `08:42:01` | `execve` | `( ... )` | `= 0` | `<0.000510>` |
|--        |--          |--                                                          |--   |--
| Absolute time stamp of syscall.<br> (Shown by the `-t` flag). | System call name (see `execve` man page [here](https://linux.die.net/man/2/execve)). | Arguments being passed to that system call (see `execve` man page [here](https://linux.die.net/man/2/execve)). | Return value from system call script executed. `0` means success. | Time spent in each syscall.<br> (Shown by the `-T` flag). |

Where to see the actual `execve` script? `tapset` of course. (Note the timestamp from when the syscall script was last run).

```
/usr/share/systemtap/tapset/linux/sysc_execve.stp
```
```
[root@rhel-8-1 linux]# ls -lu sysc_execve.stp
-rw-r--r--. 1 root root 6257 May 13 08:42 sysc_execve.stp
```

# Common syscall script return values explained

* `= 0` system call script has executed successfully, returning a `0`.

* `= -1` system call script has raised an error, returning a `-1`.
<br>

# Common posix syscalls

(Abbreviated from `webwurst` on [stackoverflow](https://stackoverflow.com/questions/6334515/how-to-interpret-strace-output)).

* `write()`<br> 
     &nbsp;copies the supplied buffer to kernelspace and returns the number of bytes actually written.

* `close()`<br> 
     &nbsp;closes the supplied descriptor and any associated resources with it in the kernel.

* `mmap()`<br> 
     &nbsp;normally used to allocate more memory for a process. The `malloc` and `calloc` library functions usually use it internally.

* `munmap()`<br> 
     &nbsp;frees the mmap'ped memory.

* `fstat()`<br> 
     &nbsp;returns various information that the filesystem keeps about a file - size, last modified, permissions, etc.
<br><br>

# What you can do with strace
 
 Trace specific system calls by using `trace=`. Like `network` calls in this example (memory calls would be `trace=memory`):
```
[root@rhel-8-1 ~]# strace -e trace=network nc localhost 2399
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
connect(3, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 3
connect(3, {sa_family=AF_INET, sin_port=htons(2399), sin_addr=inet_addr("127.0.0.1")}, 16) = -1 EINPROGRESS (Operation now in progress)
getsockopt(3, SOL_SOCKET, SO_ERROR, [0], [4]) = 0
```

Output to file with `-o`:
```
$ strace -e trace=network nc localhost 2399 -o log.txt
```

Monitor a process that's already running: 

(In this case a `netcat` session, which is pid `6359`. `accept()` is the system call to accept a connection on a socket ([read more](http://man7.org/linux/man-pages/man2/accept.2.html) ).
```
[root@rhel-8-1 ~]# strace -p 6359
strace: Process 6359 attached
select(5, [3 4], [], NULL, NULL)        = 1 (in [4])
accept(4, {sa_family=AF_INET, sin_port=htons(42444), sin_addr=inet_addr("127.0.0.1")}, [128->16]) = 5
close(3)                                = 0
close(4)                                = 0
fcntl(5, F_GETFL)                       = 0x2 (flags O_RDWR)
fcntl(5, F_SETFL, O_RDWR|O_NONBLOCK)    = 0
select(6, [0 5], [], NULL, NULL
```
---

Thanks. [The ultimate strace cheatsheet](https://linux-audit.com/the-ultimate-strace-cheat-sheet/), [The Definitive Guide to Linux System Calls](https://blog.packagecloud.io/eng/2016/04/05/the-definitive-guide-to-linux-system-calls/#what-is-a-system-call) and other articles were useful to write this. 
